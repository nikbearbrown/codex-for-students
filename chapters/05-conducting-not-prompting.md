# Chapter 5 — Conducting, Not Prompting: The Core Idea

*The orchestra does not need a better typist. It needs someone who knows what the music is supposed to sound like.*

---

Here is what went wrong.

I was thirty minutes into a build — a script to process AP CS submissions and generate a summary before I submitted grades. Codex was producing functions. The functions worked. I was reading them, accepting them, moving on. The session was fast. I thought I was conducting.

Then one function's output did not match what I expected. Not obviously wrong — it had done what I asked for, but what I had asked for was not what I needed. I rolled back. I tried a different prompt. The new function had the same problem with a different surface. I rolled back again. By the third attempt, the session context was cluttered with failed tries, and the fourth prompt was operating against a context that was now confused.

I stopped. I looked at what I had been doing for thirty minutes. I had not been conducting. I had been *typing*. The difference turned out to be everything.

---

## The metaphor that is also a mechanism

A conductor does not play the instruments. A conductor reads the score, holds the intended sound in their head, listens to what the orchestra actually produces, and intervenes when what is being played diverges from what the score says it should sound like. The orchestra provides technique — the physical production of notes. The conductor provides *meaning* — the judgment of whether those notes are the right ones, in the right order, at the right tempo.

The division of labor is clean. The orchestra is superhuman at technique. The conductor is irreplaceable for meaning. Neither can do the other's job.

![Two-panel illustration. Left: conductor stands at the podium, baton raised, with the orchestra around the stage. Right: the same conductor sits in the violin section playing alongside the musicians; the podium is empty and dashed in red.](images/05-conducting-not-prompting-fig-01.png)
*Figure 5.1 — Right seat, wrong seat: the failure mode is being in the wrong seat*

Codex is the orchestra. You are the conductor. The session that went wrong was a session where I had stopped conducting and started playing alongside the orchestra — trying to write parts of the code myself while Codex was writing other parts, prompting in real time against problems that had not been thought through, producing a build that drifted because no one was holding the meaning.

The metaphor is not decoration. It names the actual failure mode. When the conductor stops conducting and starts playing, the orchestra does not stop — it keeps producing notes. The notes are technically correct. But no one is checking whether they are the *right* notes for this piece. The build accumulates. The drift accumulates. The rollback eventually comes.

The question is: what does conducting actually look like in a Codex session? The answer is the **Ask Mode → Code Mode gate**.

---

## Two modes and why they are different things

Codex has two operational modes. The discipline lives in the boundary between them.

**Ask Mode** is read-only. Codex reads your project files, answers questions, proposes plans. It does not modify any file. It does not run any command with side effects on your system. Nothing changes. You are interrogating; Codex is responding.

Use Ask Mode to understand before doing. *What does this function do? Where would I add a new feature? What conventions does this codebase follow? How would you implement X? What are three different ways to structure this?* These are the conductor's questions — questions about what the score says before the orchestra starts playing.

The OpenAI engineering retrospective is explicit: *"For large changes, start by prompting Codex for an implementation plan using Ask Mode."*[^1] Senior engineers who use Codex every day have learned, from experience, not to skip this step. That is the tell. The people who pay the cost of skipping the gate are the ones most insistent that the gate exists.

**Code Mode** executes. Codex writes files, modifies files in place, runs commands, iterates against tests. Things change. The system moves from one state to another.

Use Code Mode for one thing: executing the plan that Ask Mode produced and you reviewed.

That is it. Every decision about *what to build* belongs in Ask Mode. Every act of *building the decided thing* belongs in Code Mode. The modes are not a suggestion about workflow. They are a separation of two categorically different kinds of work — understanding and execution — that produce different kinds of errors when mixed.

| Property | Ask Mode | Code Mode |
|---|---|---|
| What Codex does | Reads project files, answers questions, proposes plans | Writes files, modifies files in place, runs commands, iterates against tests |
| What changes in the system | Nothing | Files, state, side effects |
| What the human's job is | Interrogate, formulate, review the plan, find the unstated assumptions | Execute the reviewed plan, check handoff conditions, verify output |
| What goes wrong if you skip it | Code Mode begins on an unreviewed plan whose assumptions surface only as rollbacks | The plan never gets converted into a working artifact; understanding without execution |

---

## The gate

Between Ask Mode and Code Mode, there is a gate. The discipline is:

**Nothing goes from Ask Mode to Code Mode until you have reviewed the plan.**

Review means reading the plan with a specific purpose: finding the assumptions Codex made that you did not explicitly state. Every Ask Mode plan contains some. The average plan contains two or three. They are not failures — they are the places where the spec was underspecified and Codex filled in the most probable interpretation. Your job is to find them.

Find one. Either confirm it (you have now made an implicit assumption explicit, and the plan is stronger) or correct it (you have now caught a divergence between what Codex will build and what you need, before any code is written). The correction in Ask Mode costs a sentence. The correction in Code Mode costs a rollback.

If you cannot find an assumption, look harder. If you genuinely cannot find one after looking, approve. But be primed: the assumption you did not find will surface later as a handoff failure, and you will remember this moment.

"Looks good" is not the gate. The gate is a specific review with a specific purpose, not a skim followed by an optimistic interpretation of fluent prose.

---

## Why "looks good" is not the gate

The Ask Mode plan has the property that all fluent output has: it looks more complete than it is.

The plan reads as careful thinking. It was produced by pattern completion against your prompt and the project context. The thinking is real; it is averaged across the most-probable interpretations of what you asked for. The problem is that the most-probable interpretation and *your* interpretation are often close but not identical — and the gap between them is invisible until the code is written and the tests run.

This is the fluency trap again, in a different register. The plan feels like it covers your case. The plan covers the average case. You are not the average case; you are this specific project, this specific problem, these specific constraints that did not all make it into the prompt.

The gate is what converts "this plan reads well" into "this plan is correct for my situation." The gate is the supervisory act. The conductor listening to the rehearsal, noticing where the orchestra is playing the wrong tempo, saying so before the performance starts — not after.

---

## Handoff conditions

Within a multi-step Code Mode build, the gate extends per step.

After Step N completes, before Step N+1 begins, you check a **handoff condition** — a specific, testable, binary statement of what Step N was supposed to produce. If the condition is met, Step N+1 proceeds. If not, Step N+1 does not begin.

Strong handoff conditions share three properties. They are **specific** — they name a file, a value, a count, a state, not a vibe. They are **testable** — you can run a check and get an answer. They are **binary** — pass or fail, not "mostly fine."

"Looks good" is none of these. "Tests pass" is partial — tests check mechanical correctness, not scope or intent. The strong condition is a sentence: *The new function exists at `auth/login.py`, imports only from the standard library, passes the three test cases I wrote, and does not modify any file outside `auth/`.*

| Condition | Specific? | Testable? | Binary? |
|---|---|---|---|
| "Looks good" | No | No | No |
| "Tests pass" | Partial — which tests, against what scope? | Yes | Yes |
| "Function exists at `auth/login.py`, imports only stdlib, passes three named tests, no files modified outside `auth/`" | Yes | Yes | Yes |

The handoff condition is written *before* the step runs. This is important. Writing the condition after the step runs, when you can see the output, produces a condition that describes what happened rather than what was supposed to happen. The condition written before is a commitment; the condition written after is a rationalization.

Chapter 9 owns handoff conditions in full. For now: every significant Code Mode step has a condition you wrote before the step ran. The condition is the per-step extension of the Ask Mode → Code Mode gate.

---

## The same build, two ways

The task: add a small CLI that runs the article-review tool on a single draft file.

**Without the discipline.**

```
Prompt: "Add a CLI to run the review on a single article draft."
Codex writes cli.py — argparse, imports the articles module, calls review(),
prints the result.
Run it. Import not found. Wrong directory.
"Fix the import path."
Codex fixes the import.
Run it. review() expects different arguments.
"Use the right argument shape."
Codex fixes the call.
Run it. Output correct.
Time: 25 minutes. Three corrections. Cluttered session context.
```

**With the discipline.**

```
[Ask Mode] "I want a CLI that runs the article review on a single draft file.
Read the existing articles module and propose a plan."
Codex returns a plan. Four steps. Imports from src.articles. Calls
review_article(draft_file).

Review: the function name is wrong — it is review(draft_path,
target_path), not review_article(file). The import path is wrong —
the project uses absolute imports from src. The target argument
needs a default.

Correct the plan in Ask Mode. Codex confirms.

[Code Mode] Codex writes cli.py per the revised plan.
Run it. Output correct.
Time: 12 minutes. No corrections in Code Mode. Clean session context.
```

Same task. Same Codex. Thirteen minutes faster. No rollbacks.

The thirteen minutes is not the interesting number. The interesting number is zero — the number of rollbacks in Code Mode when the assumptions were caught in Ask Mode. The gate moved the cheap work (correcting the plan) upstream of the expensive work (rolling back the implementation). The math is favorable whenever catching one assumption saves one rollback, which is almost always.

![Three-stage flow. Left card: Ask Mode (Human — interrogate, plan, formulate). Center card: The Gate (plan reviewed and approved), bordered in red. Right card: Code Mode (Human — execute, check handoff, verify output). Arrows connect the three stages.](images/05-conducting-not-prompting-fig-02.png)
*Figure 5.2 — The Ask Mode / Code Mode gate*

---

## Best-of-N

A practical move for steps where the right answer is not obvious.

When multiple reasonable approaches exist and you are not sure which is best, generate multiple Codex responses for the same prompt and evaluate them against each other. Run the same Ask Mode plan request twice in separate sessions. Compare the two plans. The places where they differ are the places where the answer is not determined by the spec — they are the places where supervisory judgment is needed.

For Code Mode steps that produce output you have to live with — a function design, a class structure — the same move works. Generate two implementations. Compare. Choose. Note why you chose.

This is called **Best-of-N** in the practitioner literature. As of mid-2026 it is a practice, not a product feature — there is no button. The practice is: when the answer requires judgment that the spec does not fully constrain, generate multiple candidates so you have something to judge against. Codex produces the candidates; you produce the selection. The selection is the supervisory work. The candidates are the material.

The conductor does not compose the notes. The conductor chooses among the interpretations the orchestra offers, or asks for the passage again with a different tempo, and makes the choice that serves the score. Best-of-N is that move, in operational form.

---

## What the discipline is not

**It is not for beginners.** Senior OpenAI engineers use Ask-Mode-first specifically because they have paid the cost of skipping it. The discipline is for everyone who has found Codex doing something unexpected and wished they had caught it earlier.

**It is not slow.** The gate costs time only when no assumptions need correcting — and then it costs very little. When assumptions do need correcting, it saves the time of a rollback, which is always more expensive. The friction is paid back at the first avoided rollback.

**It is not about distrust.** The gate is not skepticism about Codex's ability to write code. Codex writes code well. The gate is about the conductor's job: holding the meaning of what is being built, and checking that the orchestra's technically correct notes are the right notes for this piece.

**It is not optional for multi-step builds.** For a one-line fix, skip the gate. For anything you would not bet a full hour on getting right in one shot, the gate is the first line of defense. The handoff condition is the second.

---

## What would change my mind

The chapter's central operational claim: the Ask Mode → Code Mode gate, applied per significant build, materially reduces wasted Code Mode work and produces better outputs than starting from Code Mode directly.

What would soften that claim: a controlled comparison — same tasks, with and without the gate — that found no measurable difference in build time or output quality. If the assumption-correction work in Ask Mode does not actually prevent rollbacks in Code Mode, the gate becomes recommended practice rather than load-bearing. I expect the difference to be substantial because the math is favorable whenever upstream work catches even one assumption that would have produced a rollback. That is nearly always.

What remains open: the exact threshold below which the gate is overhead. A typo fix does not need it. A multi-file refactor does. The middle cases are fuzzy. Working heuristic: any change you would not bet a full hour on getting right in one shot.

Also open: how the gate interacts with Codex's multi-step autopilot surfaces, where Codex chains steps with limited human intervention. The book argues against those for student work. Whether they can be used safely with the gate applied at the plan level is an open question.

---

## What is still puzzling

**When Best-of-N becomes a habit vs. overhead.** The book does not prescribe frequency. Use it when the answer requires judgment the spec does not fully constrain. Skip it when the spec determines the answer. For most student work: use it on design steps (which structure, which approach), skip it on implementation steps (write the function that does X).

**The long-horizon effect of skipping the gate.** The worked example shows a single build. Over a semester, a student who never uses the gate accumulates session context that is cluttered with failed attempts and corrections in Code Mode. The correction habits that Code-Mode-first produces — try, fail, reprompt — are themselves a form of skill formation. But it is the wrong skill. Reprompting against failed output is not the same as reviewing a plan before execution. The student who skips the gate is practicing triage, not architecture.

---

## AI Wayback Machine

🕰️ **Herbert Simon** (1916–2001) — Nobel-laureate economist and cognitive scientist whose concept of **bounded rationality** is the deep frame for the gate.[^2] Simon argued that real decision-makers do not optimize against infinite information — they operate within real cognitive limits, and good systems extend those limits without removing the decision-making work that constitutes good judgment.

The Ask Mode → Code Mode gate is bounded rationality applied to AI-assisted coding. Codex extends your pattern-completion capacity — you can produce code you could not write from memory, at a pace you could not sustain unaided. The gate preserves your supervisory capacity — you check the plan against your actual situation before the code is written. The combination is Simon's prescription: the tool extends, the gate preserves.

Without the gate, the tool extends and erodes — extending what you can do today, eroding the judgment you would exercise tomorrow. With the gate, the tool extends and preserves. Simon would recognize the trade-off immediately. The gate is not a tax on Codex's power. It is what keeps the power yours.

---

## Bridge

You have the gate. The next chapter names the five things the human must never delegate — the supervisory capacities the discipline keeps yours, no matter how fluent Codex becomes.

---

## Exercises

**Warm-up**

1. *(Tests: Ask Mode vs. Code Mode distinction)* List three actions that belong in Ask Mode and three that belong in Code Mode. For each, state in one sentence why it belongs where you placed it.

2. *(Tests: what makes a handoff condition strong)* You are given three handoff condition candidates for a step that adds a new authentication function: (a) "looks good," (b) "tests pass," (c) "function exists at `auth/login.py`, accepts `username` and `password`, returns a boolean, passes five named test cases, modifies no file outside `auth/`." Explain what each candidate does and does not check, and which one you would use.

3. *(Tests: the gate rule)* A classmate says: "I read the Ask Mode plan and it looked reasonable, so I switched to Code Mode." Is this the gate? What specifically is missing?

**Application**

4. *(Tests: assumption-finding in a real plan)* You ask Codex in Ask Mode: "Add a function that sends a weekly summary email to all users." Codex returns a four-step plan. Identify at least two assumptions Codex likely made that you did not state in your prompt. For each: name the assumption, state whether you would confirm or correct it, and explain what would go wrong in Code Mode if you did not catch it first.

5. *(Tests: writing a handoff condition before execution)* Choose a multi-step Codex task from a current or recent project. Write a handoff condition for one significant step — before running it. The condition must be specific, testable, and binary. Submit the condition alongside the step description.

6. *(Tests: Best-of-N as supervisory judgment)* You are designing the data model for a new feature. You run the same Ask Mode plan request twice and get two different proposals. Describe a process for choosing between them that constitutes supervisory judgment rather than a coin flip. What would you look for in the diff between the two plans?

**Synthesis**

7. *(Tests: gate + handoff conditions together)* Walk through the "two ways" worked example from the chapter and identify every point at which a handoff condition should have been written in Path B. For each point: write the condition, and explain what failure it would have caught if Path B had gone wrong at that step.

8. *(Tests: the fluency trap in the planning register)* The chapter argues that "looks good" is not the gate because Ask Mode plans have the same property as all fluent output — they look more complete than they are. Explain this claim in your own words, connecting it to the fluency trap from Chapter 2. Then describe a specific inspection move (what you would actually look for when reviewing a plan) that converts "looks good" into a genuine gate check.

**Challenge**

9. *(Open-ended)* The chapter's working heuristic for when the gate is required is: "any change you would not bet a full hour on getting right in one shot." Design a more precise decision rule for when to use Ask Mode first. Your rule should be checkable (you can apply it in under 30 seconds at the start of a session), handle the edge cases the heuristic leaves fuzzy, and be based on properties of the task rather than your confidence level. Explain the trade-offs of your rule against the chapter's heuristic.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Simon, H. A. *The Sciences of the Artificial*. MIT Press, 3rd ed., 1996.

---

## Prompts

Use these prompts with Claude to generate interactive D3 v7 versions of the figures in this chapter. Each produces a standalone HTML file you can open in a browser and modify freely.

**Prerequisites:** Load `brutalist/CLAUDE.md` and `brutalist/DESIGN.md` into your Claude project context before using these prompts. They define the stack, naming conventions, color system, and typography the figures use.

---

### Figure 5.1 — Right seat, wrong seat

Build a two-panel illustration in D3 v7. Two equally-sized `--color-fill` panels side by side, each with a monospace ALL CAPS header (`PANEL A — AT THE PODIUM`, `PANEL B — IN THE SECTION`). In Panel A, draw a simple stage line, a rectangular podium centered on it in `--color-white` with `--color-ink` outline, a stick-figure conductor on the podium with one arm raised holding a baton, and six small circles to either side representing the orchestra. In Panel B, draw the same stage line and an empty podium dashed in `--color-red` with an italic label `empty`. A stick-figure conductor (in `--color-red`) sits in the violin row holding a small rectangular violin; flanked by two other violinists in `--color-secondary`; additional orchestra dots on the far side. One-line caption under each panel. Hovering either panel shows a tooltip explaining what the seat does and does not do.

> Reference implementation: `d3/05-conducting-not-prompting-fig-01.html`

---

### Figure 5.2 — The Ask Mode / Code Mode gate

Build a three-stage horizontal flow in D3 v7. Three cards from left to right. Left card (`--color-fill`): `ASK MODE · Human`, with three short arrow-bulleted lines (`→ interrogate`, `→ plan`, `→ formulate`) and an italic `--color-secondary` line `no files change.`. Center card (`--color-white` with `--color-red` border): `THE GATE`, with bold red title `plan reviewed and approved` and two italic `--color-secondary` lines `find one assumption.` / `confirm or correct.`. Right card (`--color-fill`): `CODE MODE · Human`, with three arrow-bulleted lines (`→ execute`, `→ check handoff`, `→ verify output`) and an italic line `files, state, effects.`. Connect the three cards with `marker-end` arrows. Hovering any stage shows a tooltip explaining what that stage does and what goes wrong if it is skipped. Dashed footer rule plus two caption lines: the gate is a specific review with a specific purpose; correction in Ask Mode costs a sentence, in Code Mode costs a rollback.

> Reference implementation: `d3/05-conducting-not-prompting-fig-02.html`
