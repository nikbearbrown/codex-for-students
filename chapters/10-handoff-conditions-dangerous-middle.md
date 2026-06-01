# Chapter 10 — Handoff Conditions and the Dangerous Middle
*The failure that survives a clean specification is the one you didn't know to check for.*

> Not "looks good." A specific, testable condition that must be true before the next step begins.

---

Seth approved a Codex output.

The Ask Mode plan had been reviewed. The five-element specification was clean. The Code Mode implementation looked correct — the function was the right shape, the tests Seth had asked for were present and passing, the diff touched only the files the specification had named. Seth wrote "tests pass" as his handoff condition, checked it, and proceeded to the next step.

Six days later, downstream of three subsequent build steps, something broke. Seth traced backward. The original function had a subtle issue: it accepted an optional argument with a default value, and the default value was *mutable*. The classic Python mistake. The function's first call set the default; subsequent calls inherited the modified default. Each test had run individually and passed — each test got a fresh default — but the function's behavior in the real application, where it was called repeatedly in sequence, accumulated state in a way the tests did not exercise.

The Code Mode output had passed the condition Seth wrote. The condition Seth had *needed* to write was: *the function has no mutable default arguments, and the tests cover the repeated-call case.* He had not written that condition because he had not thought of it. Mutable default arguments are exactly the kind of Python footgun students learn about by encountering it once.

Seth had now encountered it.

This is the **dangerous middle**: the case where the plan was reviewed, the specification was clean, the output passed the handoff conditions you wrote, and the output was still wrong — because the condition you needed was one you did not think to write.

![The handoff condition as a gate between build](images/10-handoff-conditions-dangerous-middle-fig-01.png)
*Figure 10.1 — The handoff condition as a gate between build*

---

## What a handoff condition is

A handoff condition is a binary check between build steps. After Step N completes, before Step N+1 begins, the check answers: *is the state of the system what Step N was supposed to produce?*

Three properties, each necessary:

**Specific.** Not "looks right." Not "tests pass" by itself. Something with a function name, a file path, a count, a behavior that can be checked. If you cannot point to the exact thing being verified, the condition is not specific enough.

**Testable.** A check you can run that produces an answer: a test suite, a linter, a `git diff`, a manual inspection of a specific file at a specific line. If the check requires judgment about whether the result "seems fine," it is not a handoff condition — it is an impression.

**Binary.** Pass or fail. Yes or no. "Mostly worked" means the condition failed; go back.

The condition is written *before* the step runs, so that the step's results can be checked against something fixed. Writing the condition *after* the step — against the results you see — is post-hoc validation. It catches some failures, but not the ones where the results look fine and are not. Post-hoc validation cannot catch the dangerous middle, because the dangerous middle looks fine.

Here is the shape of the difference:

| Step | Weak | Strong |
|------|------|--------|
| Add a function | "Tests pass" | "Function exists at `module/path.py`; signature matches spec; tests for success/error/edge cases all pass; no mutable default arguments; no modification to existing files outside the spec's invariants" |
| Refactor a class | "Code still works" | "All existing tests still pass; the public interface is unchanged (no method names changed, no argument order shuffled); no new dependencies added; the diff is under 200 lines" |
| Add a new endpoint | "Endpoint responds" | "Endpoint at `/api/foo` returns 200 on valid input and 400 on invalid input; the endpoint uses the existing auth middleware; no changes to the auth middleware itself; integration test covers both cases" |

The strong conditions are not heroic. They are concrete. Each one takes thirty seconds to write and thirty seconds to verify.

| Weak | Strong |
|---|---|
| "Tests pass" | "Function exists at `articles/feedback.py`; signature matches the spec; success / error / edge-case tests all pass; no mutable default arguments; no files modified outside the spec's invariants" |
| "Code still works" | "All existing tests still pass; public interface is unchanged (no renamed methods, no reshuffled argument order); no new dependencies; diff is under 200 lines" |
| "Endpoint responds" | "`/api/foo` returns 200 on valid input and 400 on invalid input; uses the existing auth middleware untouched; integration test covers both cases" |
| "Refactor looks clean" | "Public API surface in `articles/__init__.py` is byte-identical to pre-refactor; internal callers updated; `pytest -q` is green; no `pandas` introduced where stdlib was used before" |
| "AGENTS.md updated" | "AGENTS.md has a new dated entry under Lessons learned naming the convention; the rule appears in Architecture as a one-line invariant; the file still parses as valid markdown" |

---

## The dangerous middle, named

The dangerous middle is not the failure where the plan was wrong or the specification was sloppy. It is the failure that survives a good plan and a clean specification and still gets through — because the gap between what the specification required and what would *actually* serve the situation is where it lives.

Three subtypes worth recognizing.

**Subtle language footguns.** Mutable default arguments in Python. JavaScript's `==` vs. `===`. Implicit conversions, scope rules, async-mode interactions. The output is syntactically correct and idiomatically reasonable; the footgun is invisible until it fires. Codex does not produce these by malice — it produces them because the most-common pattern in training data is the pattern that happens to have the footgun, and no specification excluded it.

**Convention misalignments.** Codex generates code in the most-common convention for the language or framework. Your project has a less-common convention. The output is technically correct against the average convention; it conflicts with your project's specific convention in a way that surfaces only when something downstream depends on it. The AGENTS.md entry that covers the convention is the protection here — Chapter 7. But AGENTS.md cannot cover conventions you have not yet made explicit.

**Edge-case omissions.** Codex generates code for the cases the specification described. The cases the specification did *not* describe — and that the test suite did not cover — are unhandled. The unhandled case appears weeks later, when a user does something you did not anticipate and the function has no answer for it.

The common thread: each subtype survives a specification that is clean for the cases it covers. The dangerous middle is in the uncovered cases. The protection is writing handoff conditions that explicitly check for the failure mode you suspect, even when the specification did not require you to.

The protection that is *not* sufficient: assuming the output is correct because the specification was clean. The specification governs what Codex produces; the dangerous middle is in the gap between what the specification required and what the situation actually needs.

![Three dangerous-middle subtypes as a taxonomy ](images/10-handoff-conditions-dangerous-middle-fig-02.png)
*Figure 10.2 — Three dangerous-middle subtypes as a taxonomy *

---

## When a condition fails: `/rewind`, not forward correction

Codex has a `/rewind` command. It restores the session and code state to a checkpoint before the failing step. When a handoff condition fails, the discipline is: do not patch the result with a follow-up prompt. `/rewind` to before the failing step. Revise the specification to include the failed condition as a negative constraint. Run the step fresh.

The reasoning is about context pollution. Each forward correction adds the failed attempt to Codex's working context. After two corrections, Codex is reasoning against a context that contains more failed attempts than successful pattern. The third attempt is less likely to work than the first would have been with a tighter specification.

OpenAI's own engineers state the rule directly: *"If you've corrected Codex more than twice on the same issue, the context is cluttered. Start fresh with a more specific prompt."*[^1] After two failed corrections, use `/clear` (which discards the session entirely) or `/rewind` (which restores to a checkpoint) and run the step with a tighter spec.

The cost of starting fresh feels high in the moment. It is consistently lower than the cost of continuing with polluted context. A session that has been forward-corrected twice is not the same session it was at the start; the outputs it produces in that state are less reliable than the outputs a fresh session would produce from a better-specified prompt.

The rule of thumb: two corrections is the limit. After the second failed correction on the same issue, stop. Rewind. The problem is in the specification, not in Codex's execution.

---

## The STOP block

There is a protection against the dangerous middle that fires *during* Code Mode rather than at the end of a step. Include explicit stop conditions in your specification — conditions under which Codex must pause and ask before continuing.

For an article-review-tool feature build:

> *"STOP and confirm before: (a) modifying any file outside `articles/`, (b) introducing any new dependency, (c) generating any output that rewrites the article's prose, (d) modifying the target/length-rule format."*

The STOP block is the handoff condition for scope creep. It catches the case where Codex starts to do something the negative constraint should have prevented — a case that happens not because Codex ignores the constraint, but because the constraint was specified at the task level and the execution path went through a sub-step that the constraint did not explicitly name.

Codex respects STOP blocks reliably when they are explicit and concrete. The block should name specific files, specific operations, specific output contents. Not "stop if you're about to do something big" — Codex cannot evaluate that. Specific: "stop before modifying any file outside `articles/`."

The STOP block is the protection for steps with high consequence horizons — steps where a wrong turn is expensive to reverse, where the dangerous middle produces downstream failures that take days to trace, where the cost of a false start significantly exceeds the cost of a brief pause to confirm. In the planning phase of a real build (Chapter 11), identifying which steps need STOP blocks is part of the plan.

---

## Best-of-N as a verification tool

There is a different protection against the dangerous middle that works for judgment-intensive steps: generate multiple Codex responses for the same task, evaluate all of them, and select the one that best matches your situation.

When two responses agree on a problematic framing — both produce a function with the same mutable default argument, say — the problem is upstream, in the specification. Codex is being asked to do something whose most-probable answer has the issue. The fix is to revise the spec: add the constraint that the condition you missed was supposed to exclude.

When two responses disagree — one uses a mutable default, one does not — you have material for judgment. You evaluate, select the one without the footgun, and have caught the dangerous middle without having had to anticipate it explicitly in the handoff condition. This is Plausibility Auditing exercised at scale: you are comparing outputs rather than auditing one output in isolation.

Best-of-N as a technique is not a Codex feature button as of mid-2026; it is a practice. You run the same task prompt twice (or three times, for high-stakes steps), read all the results, and apply your judgment to the selection. The overhead is real. For steps where the dangerous middle is likely — steps in domains where your depth is shallow, steps that touch the most-trafficked parts of the codebase, steps where a wrong turn would require an expensive rewind — the overhead is worth it.

The selection itself is where the supervisory capacities converge. Plausibility Auditing tells you which response is more likely to be correct. Interpretive Judgment tells you which response better fits the specific situation the specification described. Executive Integration tells you which response better serves the project's goals across the whole build, not just this step. The Best-of-N moment is the moment all five capacities are in play simultaneously.

---

## The Grace Hopper principle

There is a principle behind the handoff condition discipline that predates Codex by about seventy years, and naming it changes how you think about what the discipline is for.

Grace Hopper — computer scientist, US Navy Rear Admiral, developer of COBOL and the A-0 compiler — insisted throughout her career that *"correct" must be defined before it can be verified.* Her account of programming was that the practitioner's discipline is in specifying correctness explicitly, not assuming that the absence of obvious errors equals correctness. Her warning, stated across her recorded talks: *"The most dangerous phrase in the language is 'we've always done it this way.'"*[^2]

The dangerous middle is what "we've always done it this way" produces. The handoff condition that has always worked — "tests pass" — gets used again on a step where it is not sufficient. The condition that was met before is checked again. The case where the unchecked condition matters is the case where the failure occurs.

Hopper's insistence on explicit verification criteria is the handoff condition principle stated at the founding of software engineering. The chapter's discipline is hers, restated for Codex and applied with care: define what "correct" means *before* the step runs. Not after. Before.

The definition does not need to be exhaustive. It needs to be explicit. The dangerous middle lives in the gap between what you *assumed* to be true and what you *stated* to be checkable. Hopper's point — and the chapter's point — is that the gap closes only when you write the condition down.

---

## What would change my mind

The chapter's central claim is that the dangerous middle is real, frequent, and catchable by the discipline: handoff conditions written in advance, the STOP block, the `/rewind` rule, Best-of-N as a technique. If a controlled comparison found that students using the discipline produced builds with no fewer dangerous-middle failures than students not using it, the prescription weakens. The chapter would still recommend the discipline; the urgency of applying it to every consequential step softens.

I expect the difference to be substantial. The dangerous middle's failure mode is invisible without explicit checking; the discipline is the explicit-checking practice. The two variables are directly related.

---

## Still puzzling

The exact threshold for "stop and restart." Two failed corrections is the book's number. Some practitioners use three. Some use one. The right number probably depends on session length, the model generation, and what the failed corrections are — a syntax error is recoverable; a wrong architectural choice compounding into context is not.

How much of the dangerous middle is catchable by automated tools. Linters catch some of it — the mutable-default-argument footgun is a known lint rule in Python; `ruff` flags it. Type checkers catch others. Whether the discipline can be partially automated as a layer underneath, leaving the practitioner to focus on the genuinely interpretive cases, is open and probably the most interesting near-term question in this space.

Whether Plausibility Auditing develops with the discipline. The book's working answer is yes — practicing PA on every output strengthens it over time. Whether the strengthening is measurable, and at what rate, is not directly studied.

---

## AI Wayback Machine

🕰️ **Grace Hopper** (1906–1992) — computer scientist and US Navy Rear Admiral who developed COBOL and the A-0 compiler and who insisted that **"correct" must be defined before it can be verified.** Hopper's account of programming was that the practitioner's discipline is in specifying correctness explicitly — not assuming the absence of errors equals correctness. Her warning, *"The most dangerous phrase in the language is 'we've always done it this way,'"* applies directly to handoff conditions that default to "tests pass."[^2] The dangerous middle is exactly what that phrase produces: the condition that worked before, reused, on a step where it is not sufficient. Hopper's insistence on explicit verification criteria is the handoff condition principle stated at the founding of software engineering. The chapter's discipline is hers, restated for Codex: define "correct" before the step runs, not after.

![Grace Hopper](../images/grace-hopper-wqk.png)

*Puppet Art by [Nik Bear Brown](https://www.nikbearbrown.com/).*

---

## Bridge

You have the full conducting discipline for code builds. Chapter 11 applies the same discipline to creative work — the Brutalist three-file system for projects where the decisions are aesthetic as well as technical.

---

## Exercises

**Warm-up**

1. *(Targets: three properties of a handoff condition)* Take the following three conditions and classify each as specific/testable/binary or not, and explain what is missing from the ones that fail: (a) "The code looks good." (b) "All pytest tests pass and the diff touches only `articles/target.py`." (c) "The refactor mostly worked but there's one edge case I'll fix later."

2. *(Targets: weak vs. strong conditions)* Rewrite this weak condition as a strong one: "The new user-authentication endpoint works." Your rewrite should be specific, testable, and binary. Name the file path, the HTTP status codes, the middleware being used, and at least one thing the endpoint must *not* do.

**Application**

3. *(Targets: writing conditions before the step)* Take a Codex task you are about to run — or one you have run recently — and write the handoff condition for it *before* looking at the output. Then check the output against the condition. Did it pass? If it failed, what would the revised specification need to say?

4. *(Targets: STOP block)* Write a STOP block for a step in your current project that touches a file or module you would not want Codex to modify without confirmation. The block should name at least three specific things Codex must stop before doing. Apply it to your next session.

**Synthesis**

5. *(Targets: dangerous middle subtypes + protection)* Seth's mutable-default-argument failure is a language footgun. Identify one example from your own experience (or construct a plausible one) for each of the other two subtypes: a convention misalignment and an edge-case omission. For each: name the gap between what the specification would have said and what the situation actually needed. Name the handoff condition that would have caught it.

**Challenge**

6. *(Targets: /rewind discipline + context pollution)* The chapter claims that two forward corrections is the limit before context is too polluted to recover cleanly. Design a small experiment to test this claim: what would you measure, what would a confirming result look like, and what would a disconfirming result look like? You do not need to run the experiment — design it well enough that someone else could.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Hopper, G. *Various lectures and interviews*, 1980s. The "we've always done it this way" phrase is quoted across her recorded talks and is widely attributed.
