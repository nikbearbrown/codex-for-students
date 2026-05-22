# Chapter 9 — Handoff Conditions and the Dangerous Middle

> Not "looks good." A specific, testable condition that must be true before the next step begins.

---

## Learning outcomes

1. **(Understand)** Explain what a handoff condition is and why "looks good" fails as one.
2. **(Apply)** Write handoff conditions for a set of provided Codex tasks.
3. **(Analyze)** Identify the "dangerous middle" — tasks where Codex's output requires specific verification that isn't in the obvious checklist.

---

## Opening

Seth approved a Codex output. The Ask Mode plan had been reviewed. The five-element specification was clean. The Code Mode implementation looked correct — the function was the right shape, the tests Seth had asked for were present and passing, the diff touched only the files the specification had named.

Seth proceeded to the next step.

Six days later, downstream of three subsequent build steps, something broke. Seth traced backward. The original function had a subtle issue: it accepted an optional argument with a default value, and the default value was *mutable*. The classic Python mistake. The function's first call set the default; subsequent calls inherited the modified default. The function's tests had run individually and passed (each test got a fresh default), but the function's behavior in the real application, where it was called repeatedly, accumulated state in a way the tests did not exercise.

The Code Mode output had passed the handoff condition Seth wrote — "tests pass." The condition Seth had *needed* to write was *"the function has no mutable default arguments and the tests cover the repeated-call case."* He had not written that condition because he had not thought of it. The mutable-default-argument issue is exactly the kind of subtle Python footgun that students learn about by encountering it once — and Seth had now encountered it.

This is the **dangerous middle**: the case where the Ask Mode plan was reviewed, the specification was clean, the Code Mode output was correct against the handoff condition you wrote, and the output was *still wrong* because the condition you needed to write was one you did not know to write.

This chapter is the dangerous middle, named.

<!-- → [DIAGRAM: The handoff condition as a gate between build steps. Step N → [Handoff condition check] → Step N+1. If check fails: /rewind to step N checkpoint. If check passes: proceed. Editorial style.] -->

---

## What a handoff condition is

A handoff condition is a binary check between build steps. After Step N completes, before Step N+1 begins, the check answers: *is the state of the system what Step N was supposed to produce?*

Three properties.

**Specific.** Not "looks right." Not "tests pass" (alone). Something with a function name, a file path, a count, a behavior that can be checked.

**Testable.** A check you can run and that produces an answer. A test suite. A linter. A `git diff`. A manual inspection of a specific file.

**Binary.** Pass or fail. Yes or no. "Mostly worked" is not a handoff condition.

For each major step in a build, write at least one handoff condition. The condition is written *before* the step runs — so that the step's results can be checked against it. Writing the condition *after* the step, against the results, is post-hoc validation, which catches some failures but not the ones where the results look fine and are not.

Strong vs. weak handoff conditions:

| Step | Weak | Strong |
|------|------|--------|
| Add a function | "Tests pass" | "Function exists at `module/path.py`; signature matches spec; tests for success/error/edge cases all pass; no mutable default arguments; no modification to existing files outside the spec's invariants" |
| Refactor a class | "Code still works" | "All existing tests still pass; the public interface of the class is unchanged (no method names changed, no argument orders shuffled); no new dependencies added; the diff is under 200 lines" |
| Add a new endpoint | "Endpoint responds" | "Endpoint at `/api/foo` returns 200 on valid input and 400 on invalid input; the endpoint is authenticated (uses the existing auth middleware); no changes to the auth middleware itself; integration test covers both cases" |

The strong conditions are not heroic. They are concrete. Each takes thirty seconds to write and thirty seconds to verify.

---

## What the dangerous middle is

The class of failures where:

- The Ask Mode plan was reviewed.
- The five-element specification was clean.
- The Code Mode output passed the handoff conditions you wrote.
- The output was still wrong, because the condition you needed was one you did not think to write.

The dangerous middle is the hardest failure mode to catch. Three subtypes worth recognizing:

**Subtype 1: subtle language footguns.** Mutable default arguments in Python. JavaScript's `==` vs. `===`. Implicit conversions, scope rules, async-mode interactions. The CLI's output is syntactically correct and idiomatically reasonable; the footgun is what catches you.

**Subtype 2: convention misalignments.** Codex generates code in the most-common convention for the language/framework. Your project has a less-common convention. The output is technically correct against the average convention; it conflicts with your project's specific convention in a way that surfaces only when something downstream depends on the project's convention.

**Subtype 3: edge-case omissions.** Codex generates code for the cases the specification described. The cases the specification did *not* describe — and that the test suite did not cover — are unhandled. The unhandled case appears in production months later.

For each subtype, the protection is the same shape: *write handoff conditions that explicitly check the failure mode you suspect could happen, even if the spec did not require it.* The discipline is anticipating the footguns and adding checks for them.

The protection that is *not* sufficient: assuming Codex's output is correct because the spec was clean. The spec governs what Codex produces; the dangerous middle is in the *gap* between what the spec required and what would *actually* serve the situation.

---

## When a condition fails: `/rewind`, do not correct forward

Codex has a `/rewind` command (in the Codex CLI; the Codex app has an equivalent). It restores the session and code state to a checkpoint before the failing step.

The discipline: when a handoff condition fails, do not patch the result with a follow-up prompt. `/rewind` to before the failing step. Revise the specification to include the failed condition as a negative constraint. Run the step fresh.

The reasoning: forward correction pollutes the session context. Each correction adds the failed attempt to Codex's working memory. After two corrections, Codex is reasoning against a context that contains more failed attempts than successful pattern. The third attempt is less likely to work than the first.

OpenAI's engineers describe the rule the same way: *"If you've corrected Codex more than twice on the same issue, the context is cluttered. Start fresh with a more specific prompt."*[^1] After two failed corrections, use `/clear` (which discards the session entirely) or `/rewind` (which restores to a checkpoint) and start the step with a tighter spec.

The cost of starting fresh feels high. It is consistently lower than the cost of continuing with polluted context.

<!-- → [TABLE: Strong vs. weak handoff conditions — two columns. Five examples. Left: weak. Right: strong. No color.] -->

---

## The STOP block

A pattern from the practitioner literature: include explicit conditions in your specification under which Codex must pause and ask before continuing.

For a grading-tool feature build:

> *"STOP and confirm before: (a) modifying any file outside `grading/`, (b) introducing any new dependency, (c) generating any output that contains a numeric or letter grade, (d) modifying the rubric format."*

The STOP block is a handoff condition that fires *during* Code Mode rather than at the end of a step. It catches the scope creep that the negative constraint was supposed to prevent. Codex respects STOP blocks reliably when they are explicit and concrete.

The STOP block is the protection against the dangerous middle that emerges during a single step's execution — the case where Codex starts to do something the negative constraint should have prevented but did not. By Chapter 11 (the planning phase of a real build), the STOP block is essential for the steps with the highest consequence horizons.

---

## Best-of-N as a verification tool

A different protection against the dangerous middle: generate multiple Codex responses for the same task; evaluate all of them; select the one that best matches your situation.

When two responses agree on a problematic framing (both produce a function with the same mutable default argument), the problem is in *the specification* — Codex is being asked to do something whose most-probable answer has the issue. The fix is upstream: revise the spec.

When two responses disagree on a problematic framing (one uses a mutable default, one uses None), you have material for judgment. You evaluate, select, and have caught the issue without having had to anticipate it explicitly.

Best-of-N as a *technique* (not a Codex feature button as of mid-2026) is the supervisory tool for judgment-intensive steps. It is the move that exercises Plausibility Auditing at scale.

---

## Common misconceptions

**"Exit 0 / tests pass is a strong condition."** No. Tests catch what the tests cover. The dangerous middle is what the tests do not cover.

**"I'll catch the dangerous middle by being careful."** Vague carefulness misses it. The discipline of *writing conditions in advance* is what catches.

**"Forward correction works for me."** Sometimes, on small fixes. The math turns against you on multi-step builds. After three corrections, the context is unrecoverable.

**"PA always catches the dangerous middle."** Sometimes. PA fires when you have domain knowledge to notice. For domains you are new to, PA is less reliable. Handoff conditions and the STOP block are the protections that do not require pre-existing domain mastery.

**"The dangerous middle is rare."** It is the most common failure mode for AI-assisted coding in the book's reader population. The chapter exists because the failure mode is frequent enough to deserve its own chapter.

---

## Exercises

1. **(Apply)** Write handoff conditions for five Codex tasks in a provided build sequence. Each condition: specific, testable, binary.

2. **(Analyze)** A provided build transcript shows Codex crossing into the dangerous middle. Identify the exact moment and the handoff condition that would have caught it.

3. **(Create)** Add a STOP block to a Codex specification for a task in your current project that touches a dangerous middle.

---

## What would change my mind

The chapter's central claim is that **the dangerous middle is real, frequent, and catchable by the discipline** — handoff conditions, the STOP block, the `/rewind` rule, Best-of-N as a technique. If a controlled comparison found that students using the discipline produced builds with *no fewer* dangerous-middle failures than students not using it, the chapter's prescription weakens. The chapter would still recommend the discipline; the case for "every consequential step" softens.

I expect the difference to be substantial because the dangerous middle's failure mode is invisible without explicit checking, and the discipline is the explicit-checking practice.

---

## Still puzzling

- **The exact threshold for "stop and restart."** Two failed corrections is the book's number. Some practitioners use three. Some use one. The right number probably depends on session length, model generation, and personal practice.

- **How much of the dangerous middle is catchable by automated tools.** Linters catch some (the mutable-default-argument footgun is a known lint rule in Python). Type checkers catch others. Whether the discipline can be partially automated as a layer underneath, leaving the practitioner to focus on the genuinely interpretive cases, is open.

- **Whether plausibility auditing develops with the discipline.** The book's working answer is yes — practicing PA on every output strengthens it. Whether the strengthening is measurable, and at what rate, is not directly studied.

---

## AI Wayback Machine

🕰️ **Grace Hopper** (1906–1992) — computer scientist and US Navy Rear Admiral who developed COBOL and the A-0 compiler and who insisted that **"correct" must be defined before it can be verified.** Hopper's account of programming was that the practitioner's discipline is in *specifying correctness explicitly* — not assuming the absence of errors equals correctness. Her famous warning, *"The most dangerous phrase in the language is 'we've always done it this way,'"* applies directly to handoff conditions that default to "tests pass."[^2] The dangerous middle is exactly the failure mode "we've always done it this way" produces — the condition that was met before is checked again, and the case where the unchecked condition matters is the case where the failure occurs. Hopper's insistence on explicit verification criteria is the handoff condition principle stated at the founding of software engineering. The chapter's discipline is hers, restated for Codex and applied with care.

---

## Bridge

You have the full conducting discipline for code builds. Chapter 10 applies the same discipline to creative work — the Brutalist three-file system for projects with aesthetic decisions.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Hopper, G. *Various lectures and interviews*, 1980s. The "we've always done it this way" phrase is quoted across her recorded talks and is widely attributed.
