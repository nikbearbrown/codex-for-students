# Chapter 4 — Conducting, Not Prompting: The Core Idea

> Programming as conducting. Codex does what it's superhuman at. You do what only you can.

---

## Learning outcomes

1. **(Understand)** Explain the difference between prompting Codex and conducting a build with Codex.
2. **(Apply)** Use Ask Mode for planning and Code Mode for execution — and explain why the gate between them matters.
3. **(Understand)** Explain what a handoff condition is and why it matters.

---

## Opening

This is Seth.

The orchestra metaphor came to me halfway through a build that was going wrong, and once I had it I could not unsee it.

I was using Codex to build a small piece of a personal project — a script that would process my AP CS submissions and generate a summary for me to review before submitting. Codex was producing functions. The functions worked. I was reading them, accepting them, moving on. The session was fast. I was, I thought, conducting.

About thirty minutes in, the output of one function did not match what I had expected. Not in a way that was obviously wrong — the function had done what I asked it to do, but what I had asked for was not what I needed. I rolled back. I tried a different prompt. The new function had the same issue but with a different surface. I rolled back again. I tried a third prompt. By this point my session was cluttered with three failed attempts, and the fourth attempt was operating against context that was now confused.

That was when I caught myself. I had not been conducting. I had been *typing*. The CLI was the orchestra; I was supposed to be the conductor; instead I had been a person trying to play the instruments alongside the orchestra, getting in the way of the music because I had not stepped back to actually *direct*.

The conductor does not play. The conductor reads the score, knows what the music is supposed to sound like, listens to what the orchestra produces, and intervenes when a wrong note is about to play. The orchestra's job is technique; the conductor's job is *meaning*. I had been mixing the two roles, and the result was a build that drifted because no one was holding the meaning.

This chapter is the discipline that keeps the roles separated. The operational form is the **Ask Mode → Code Mode gate**. The metaphor is the orchestra. The point is that you are not the typist; you are the director.

<!-- → [DIAGRAM: The Ask Mode / Code Mode gate. Human in Ask Mode: interrogate, plan, formulate. Gate: plan reviewed and approved. Human in Code Mode: execute against plan, verify output. Editorial style. No color.] -->

---

## Ask Mode and Code Mode

Codex has two main operational modes. Knowing the difference is the chapter's content.

### Ask Mode

Codex operates **read-only**. It reads your project files. It answers your questions. It proposes plans. It does not modify any file. It does not run any command that has side effects on your system.

Use Ask Mode for:

- **Interrogating the codebase.** *"What does this function do?"* *"Where would I add a new feature?"* *"What conventions does this code follow?"*
- **Proposing plans.** *"How would you implement X?"* *"What steps would this build need?"*
- **Disambiguating decisions.** *"What's the difference between using a generator vs. a list comprehension here?"* *"Should this be a method on the class or a free function?"*
- **Exploring options.** *"What are three different ways to structure this?"*

Ask Mode is the cognitive register for *understanding before doing*. The OpenAI engineering retrospective is explicit on this: *"For large changes, start by prompting Codex for an implementation plan using Ask Mode."*[^1]

### Code Mode

Codex executes. It writes files. It modifies files in place. It runs commands. It iterates against tests.

Use Code Mode for:

- The implementation work, *after* the Ask Mode plan has been reviewed and approved.

That is it. Code Mode is the *execution register*. Everything that requires deciding what to build goes in Ask Mode. Everything that builds the decided thing goes in Code Mode.

### The gate

The discipline:

> **Nothing goes from Ask Mode to Code Mode until you have reviewed the plan.**

The rule is the chapter. Everything else is help with following the rule.

The plan-review step is the gate. Read the Ask Mode plan. Look for assumptions Codex made that you have not verified. Look for steps whose dependencies are different from what you assumed. Correct the plan in Ask Mode (cheap) rather than in Code Mode (expensive). When you are satisfied, approve and switch to Code Mode.

The switch is deliberate. In the Codex CLI, the command is explicit. In the Codex app, the toggle is visible. The friction is intentional — switching from Ask to Code should feel like a decision, not a default.

---

## Why "looks good" is not the gate

The most common failure of the discipline: skim the Ask Mode plan, see that it looks reasonable, approve, regret.

The plan in front of you, when Codex returns it, has the property that fluent output always has — it looks more complete than it is. The plan reads as "I have thought about this carefully." The plan was produced by pattern completion against your prompt and the project context. The thinking was real; it was averaged across the most-probable interpretations of what you asked for.

Your job is to find the place where the most-probable interpretation diverges from *your* interpretation.

A practical pattern: in every Ask Mode plan, look for one specific assumption Codex made that you did not explicitly state. Codex always makes some assumptions; the average plan contains two or three. Find at least one. Either confirm it (and now you have made the assumption explicit) or correct it (and now the plan does what you wanted, not what Codex guessed).

If you cannot find an assumption, look harder. If you genuinely cannot find one, approve — but be primed for the fact that the build will surface the assumption later as a handoff failure, and you will remember this moment.

---

## What a handoff condition is

Within a multi-step Code Mode build, the gate extends per step.

After Step N completes, before Step N+1 begins, you check a **handoff condition** — a specific, testable, binary check that the state of the system is what Step N was supposed to produce. The condition is the operational form of "Step N is done."

Strong handoff conditions are:

- **Specific** — name a file, a count, a value, a state.
- **Testable** — you can run a check and produce an answer.
- **Binary** — pass or fail.

"Looks good" is none of these. "Tests pass" is partial (mechanical, not scope or intent). The strong condition is a sentence: *"The new function exists at `auth/login.py`, imports only from the standard library, passes the three test cases I wrote, and does not modify any file outside `auth/`."*

Chapter 9 owns handoff conditions as its full chapter. For now: every significant Code Mode step has a handoff condition you wrote *before* the step ran. The condition is the per-step extension of the Ask Mode → Code Mode gate.

---

## Worked example: the same build, two ways

The task: add a small CLI that runs grading on a single submission file.

**Path A: typing without the discipline.**

```
Seth: "Add a CLI to run grading on a single submission."
Codex: [Code Mode] writes cli.py with argparse, imports the grading module,
       calls grade() on the submitted file, prints the result.
Seth: runs it.
       Realizes the grading module isn't found because it's in a
       different directory.
Seth: "Fix the import path."
Codex: changes the import.
Seth: runs it.
       Realizes the grade() function expects a different argument shape.
Seth: "Use the right argument shape."
Codex: changes the call.
Seth: runs it.
       Output is correct.
       Total time: 25 minutes. Three corrections.
       Session context is cluttered with the failed attempts.
```

**Path B: with the discipline.**

```
Seth: [Ask Mode] "I want to add a small CLI that runs grading on a single
       submission file. Read the existing grading module and propose a plan."
Codex: [Ask Mode] returns a plan. Step 1: create cli.py in the project root.
       Step 2: import the grading module from src.grading. Step 3: parse
       arguments with argparse, expecting --submission. Step 4: call
       grading.grade_submission(submission_file) and print the result.
Seth: reviews. Notices the function name is wrong — the existing function
       is grade(submission_path, rubric_path), not grade_submission(file).
       Notices Step 2's import path is wrong because the project uses absolute
       imports from src. Corrects both in the plan, mentions the rubric_path
       argument needs a default.
Codex: [Ask Mode] revises the plan. Confirms the corrections.
Seth: approves the revised plan. Switches to Code Mode.
Codex: [Code Mode] writes cli.py per the plan.
Seth: runs it. Output is correct.
       Total time: 12 minutes. No corrections in Code Mode.
       Session context is clean.
```

Same task. Same Codex. Different times. The difference is that Path B did the cheap work in Ask Mode (correcting assumptions in the plan) instead of the expensive work in Code Mode (rolling back failed implementations).

The lesson: the gate is what turns Code Mode into a clean execution of a reviewed plan, instead of a series of guesses against an unreviewed plan.

The limit: the gate does not eliminate failures. Codex sometimes produces output in Code Mode that surprises you even when the plan was reviewed. The handoff condition catches those (Chapter 9). The gate is the first line of defense; the handoff condition is the second.

---

## Best-of-N as a supervisory tool

A practical move from the practitioner literature.

When you are at a step where the right answer is not obvious — multiple reasonable approaches exist, and you are not sure which is best — generate **multiple Codex responses** for the same prompt and evaluate them all.

Operationally: run the same Ask Mode plan request twice in separate sessions. Compare the two plans. Note where they differ. The differences are the places where the answer is not determined by the spec — they are the places where supervisory judgment is needed.

For Code Mode steps that produce output you have to live with (a function design, a class structure, a UI layout), the same move works. Generate two implementations. Compare. Choose. Note *why* you chose.

This pattern is called **Best-of-N** in the practitioner literature. As of mid-2026, it is not a named user-facing feature in Codex — the technique exists; the button does not. The book uses it as a *practice* rather than as a product. The practice is: when the answer requires judgment, generate multiple candidates so you have material to judge against.

The selection is the supervisory work. The candidates are the material. Codex produces the candidates; you produce the selection. This is the **Humans + AI division of labor** in operation for judgment-intensive steps.

---

## Common misconceptions

**"Ask Mode is for beginners."** No. Senior OpenAI engineers prescribe Ask-Mode-first specifically because *they* have learned not to skip it. The discipline is for everyone using agentic AI.

**"I can keep the plan in my head."** Sometimes, for trivial tasks. For anything multi-step, the plan-in-writing (or plan-in-your-Ask-Mode-window) is the cheap-to-revise version; the plan-in-your-head is the version that drifts during execution and produces the messy Path A from the worked example.

**"The gate slows me down."** Sometimes, on tiny tasks. For real builds, the math is dramatically favorable — the 13 minutes Path B saved over Path A in the worked example is one example among many. The friction of the gate is paid back the moment you avoid a single rollback.

**"Best-of-N is overkill for student work."** Use it when the right answer requires judgment. Skip it when the spec determines the answer. For most student work, this means using it on the design steps (which structure, which approach) and skipping it on the implementation steps (write the function that does X).

**"The orchestra metaphor is just a metaphor."** It is also operational. The conductor's job — read the score, listen to the orchestra, intervene before wrong notes play — is exactly the work the supervisory capacities (Chapter 5) name. The metaphor is doing real work in the chapter; it is not decoration.

---

## Exercises

1. **(Apply)** Take a prompt you've used in the past week. Rewrite it as an Ask Mode interrogation followed by a Code Mode specification. Run the new version. Compare the result to the original.

2. **(Apply)** Write a handoff condition for a Codex task in a current project. The condition should be specific, testable, and binary — not "looks good," not "tests pass."

3. **(Analyze)** Given a provided Codex transcript, identify where the Ask Mode / Code Mode gate was skipped and what broke as a result. Trace the consequences.

---

## What would change my mind

The chapter's central operational claim is that **the Ask Mode → Code Mode gate, applied per significant build, materially reduces wasted Code Mode work** and produces better outputs than starting from Code Mode directly. If a controlled comparison — same set of tasks, with and without the gate — found no measurable difference in build time or output quality, the gate becomes optional rather than load-bearing. The book would still teach it for the supervisory-practice benefit; the urgency softens.

I expect the difference to be substantial because the gate moves the cheap work (assumption correction) upstream of the expensive work (rollback after failed implementation), and the math is dramatically favorable when the upstream work catches even one assumption that would have produced a rollback.

---

## Still puzzling

- **The exact threshold below which the gate is overhead.** A one-line change to fix a typo does not need the gate. A multi-file refactor does. The middle cases are fuzzy. The book's working heuristic: any change you would not bet a full hour on getting right in one shot.

- **How the gate interacts with Codex's "Agents" mode or other multi-step autopilot surfaces.** The Codex app has modes where Codex chains steps with limited human intervention. The book argues against using those for student work — the discipline is the point. Whether the autopilot modes can be used safely with the gate applied at the *plan* level (and trust delegated thereafter) is open.

- **Best-of-N: when does it become a habit vs. overhead?** For most students, occasionally — on the steps where judgment matters. The book does not prescribe frequency; the prescription is "when the answer requires judgment that the spec does not fully constrain, generate multiple candidates."

---

## AI Wayback Machine

🕰️ **Herbert Simon** (1916–2001) — Nobel-laureate polymath whose concept of **bounded rationality** is the deep frame for the gate. Simon argued that real human decision-makers operate within real cognitive limits and that good systems are ones that *extend those limits without removing the work that constitutes good decision-making*.[^2] The Ask Mode → Code Mode gate is bounded rationality applied to AI-assisted coding. Codex extends your pattern-completion capacity (you can produce code you couldn't write from memory); the gate preserves your supervisory capacity (you check the plan against your situation before code is written). The combination — tool extends, gate preserves — is exactly Simon's prescription for human-machine collaboration. Without the gate, the tool extends *and* erodes — extending what you can do today, eroding what you could do tomorrow. With the gate, the tool extends *and* preserves. Simon would recognize the trade-off and the resolution.

---

## Bridge

You have the gate. Chapter 5 names the five things the human must never delegate — the supervisory capacities the discipline keeps yours, no matter how fluent Codex becomes.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Simon, H. A. *The Sciences of the Artificial*. MIT Press, 3rd ed., 1996.
