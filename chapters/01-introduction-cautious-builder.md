# Chapter 1 — Introduction: The Cautious Builder
*Doing the cognitive work the tool will happily do for you — if you let it.*

> Meet Seth. He noticed something his friends didn't.

---

Here is something that happened in an AP Computer Science class, and it is worth sitting with before we say anything else.

A student — call him Seth — was sitting across from a friend who had just finished the problem set in about thirty seconds. The assignment was to trace a small event-driven program — a Godot-style signal handler with a publisher node, two subscribers, and an emit order to follow. The friend pasted the prompt into Codex, accepted what came back, pasted it into the editor, and was already on his phone. The code compiled. It passed the test cases. The friend got an A.

Two weeks later there was an in-class quiz. No laptop. No Codex. The quiz had a variant of the same problem — slightly different emit order, needed to be traced by hand. The friend froze. Wrote something. Crossed it out. Turned in a quiz about a third complete.

Seth had done the same problem set. He had used Codex, too. But he had asked Codex to walk through the signal flow step by step, predicted each subscriber's response before reading the answer, run the code with print statements at each connect and emit to watch the order resolve, then modified one connection to see what would change. The homework took him an hour and a half. On the quiz he wrote out the variant from scratch in twenty minutes.

Same homework grade. Different quiz score. Six weeks later: two different practitioners.

The thing Seth saw has a name. It is the **homework/quiz gap** — and it is the central failure mode of AI-assisted learning when the AI substitutes for the cognitive work the homework was meant to develop. The friend had borrowed capability from Codex. Seth had built it. The traced answer looked identical; the learning was not.

This book is about how to use Codex so that you build capability rather than borrow it. The discipline has a name too: *conducting*. The rest of the book is how.

![Two-point timeline showing Seth's arc from observer ("watches friends") to practitioner ("builds the discipline").](images/01-introduction-cautious-builder-fig-01.png)
*Figure 1.1 — Seth's arc from observer to practitioner*

---

## Why borrowed capability fails on the quiz

Let me be precise about what is happening, because the mechanism matters.

When Seth's friend typed the assignment into Codex and pasted the result, Codex did the thing the homework was designed to make the friend do. Not assist — replace. The cognitive work of building a mental model of signal flow, of predicting which subscriber fires when, of debugging when a connection is missed — that work is the homework. The correct trace is the *evidence* that the work happened. If the trace appears without the work, there is no evidence of learning because there was no learning.

This is not a new problem dressed in new clothes. It is the same problem that existed when students could buy summaries of books they were supposed to read, or copy a friend's lab report, or look up the answer key before the problem was attempted. What's new is how fast the gap opens and how fluent the borrowed result looks. The friend's traced answer was not suspicious. It was clean. It compiled. It passed tests. It looked exactly like what a student who understood event-driven code would produce.

The Bastani et al. randomized controlled trial (which Chapter 2 covers properly) measured this with precision. Students who used AI assistance during problem sets scored 17 percentage points lower on unassisted exams than their practice scores predicted. Students who used AI assistance *with guardrails* — something closer to what this book teaches — showed no such gap. The gap is not a story about AI being bad. It is a story about what happens when the wrong part of the work gets delegated.

![Line chart of Bastani et al. — three groups (no AI, AI without guardrails, AI with guardrails) plotted from practice score to unassisted exam. The AI-without-guardrails line rises 48 points on practice and drops 17 points below baseline on the exam.](images/01-introduction-cautious-builder-fig-02.png)
*Figure 1.2 — The Bastani RCT: practice score vs. unassisted exam*

The mechanism underneath this is not subtle. You learn to trace event-driven code by tracing it — by holding the connection graph in your head, advancing one emit at a time, predicting which subscribers fire, finding out you were wrong, adjusting. That is the cognitive work that builds the mental model. If Codex does the trace, the model does not form. The quiz is not a different kind of test than the homework; it is the same test with the scaffolding removed. When the scaffolding was Codex, removing it leaves nothing.

---

## What conducting is

Codex is an agentic system. You give it a task. It plans the work. It writes files. It runs commands. It iterates against tests. The autonomy is the feature, and it is also the reason you need a different discipline before you start than you would need with a search engine, a calculator, or even a code-completion tool.

The metaphor this book uses is the conductor.

A conductor does not play the instruments. The orchestra is faster, more technically precise, has rehearsed passages the conductor has never touched. The conductor's job is not technique — the orchestra has more of it. The conductor's job is *meaning*: the whole performance held toward an intent, individual sections corrected when they are technically right but interpretively wrong, the gap between what the score says and what the music should feel like bridged in real time.

For Codex, the gap the conductor bridges is between *what you meant* and *what Codex understood you to mean*. That gap is where builds break. Codex will execute your specification faithfully. If the specification was incomplete, it will fill the gaps with its best guess. If the specification was ambiguous, it will resolve the ambiguity in some direction. It will not ask you to clarify unless you built the discipline of asking it to ask. The orchestra plays the notes you gave them.

Conducting, operationally, has a specific form: the **Ask Mode → Code Mode gate**.

Codex has two modes. *Ask Mode* is read-and-plan: Codex reads your project, answers questions, proposes an approach, but does not modify files or execute commands. *Code Mode* is execute: Codex writes, runs, iterates. The discipline is simple to state and requires practice to hold: **nothing moves from Ask Mode to Code Mode until you have reviewed and approved the plan.**

This is not a bureaucratic step. It is the moment where you think. When Codex proposes a plan, reading that plan is the act of checking whether you understand what it is about to do. If you cannot read the plan and form an expectation — *that will create a new function here, that will modify this file here, that will call the API once here and once in the loop there* — then approving the plan means ceding the judgment to Codex. The gate is the place where the conductor listens before the orchestra plays.

![Two-node flow: Ask Mode (plan, read, propose) connected by an arrow labeled "human reviews plan" to Code Mode (write, execute, iterate).](images/01-introduction-cautious-builder-fig-03.png)
*Figure 1.3 — The Ask Mode → Code Mode gate*

Chapter 4 makes the gate concrete — what to look for in a plan, what to ask when something is unclear, what to do when the plan looks right but the scope seems larger than the task required. Here, the gate is just the thing to hold in mind: *nothing executes until I have reviewed the plan.*

---

## What this book is not

A few orientations, because the AI-tools space is dense with adjacent material that this book is not.

**This is not an AI ethics book.** There are good books about the ethics of AI assistance in education. This is not one of them. The book treats you as a builder to equip, not a potential plagiarist to manage. Whether and when to use Codex is your decision; the book is for the cases when you do.

**This is not a prompt engineering guide.** Prompt engineering guides optimize for output quality: how to get Codex to write better code, faster. This book optimizes for *capability retention* — using Codex in a way that produces working code *and* builds the practitioner who produced it. Those are not the same objective, and optimizing for one can actively harm the other. The homework/quiz gap is what you get when you optimize purely for output.

**This is not a developer course.** The Vanderbilt and Coursera Codex materials are excellent for developers who want to delegate like a senior engineer. This book is for students who want to conduct like a composer — preserving the cognitive friction that builds skill while directing a powerful machine through real builds. The target capability is different.

**This is not a Python tutorial.** AP-CS-level experience is the assumed floor. You should be able to read a function, trace a loop, and understand what a method call does. You do not need to know Codex, the Codex CLI, AGENTS.md, or the Boondoggling framework. The book teaches all of those.

---

## How the book is built

Three acts. Fourteen chapters.

**Act One (Chapters 1–3)** establishes the problem. Chapter 1 is this introduction. Chapter 2 gives the homework/quiz gap an empirical foundation — the Bastani RCT, the Kosmyna EEG study (brain connectivity drops by up to 55% during AI-assisted writing compared to unassisted writing), the Anthropic 2026 coding-skills study. Chapter 3 maps what you are good at versus what Codex is better at — not to assign permanent roles but to name the trade space clearly. Chapter 4 explains why technical fluency with the tool *without* the conducting discipline is the specific danger zone, and why school is not yet teaching the discipline most students need.

![Horizontal timeline of the book's three acts — Act One (problem, Ch 1-3), Act Two (discipline, Ch 4-10), Act Three (build, Ch 11-14).](images/01-introduction-cautious-builder-fig-04.png)
*Figure 1.4 — The three-act arc of the book*

**Act Two (Chapters 4–10)** teaches the discipline. Chapter 4 introduces the Ask Mode → Code Mode gate in operational detail. Chapter 5 names the five things the human does that Codex cannot: Plausibility Auditing, Problem Formulation, Tool Orchestration, Interpretive Judgment, Executive Integration. Chapter 6 introduces AGENTS.md — the file Codex reads at every session, the place where your project's persistent context lives and where the human's preferences and constraints are encoded so that Codex is not starting from zero each time. Chapter 7 is problem formulation — the work that happens *before* the first prompt. Chapter 8 is the five-element specification format. Chapter 9 covers handoff conditions and the dangerous middle of a build, when Codex is mid-execution and something has gone slightly wrong. Chapter 10 is the Brutalist three-file system for creative builds — a lightweight structure that keeps scope contained.

**Act Three (Chapters 11–14)** is the build. Chapter 11 plans your first conducted project. Chapter 12 runs it. Chapter 13 verifies it. Chapter 14 hands you the build and asks you to conduct it end-to-end, then produce the post-build learning document that converts the experience of building into the capacity to teach building.

Read the chapters in order the first time. After that, they can be consulted independently during a live build.

---

## The five things you do that Codex cannot

Chapter 5 treats this in detail. Here, briefly, because it reframes the book's purpose.

Codex is better than you at a specific class of tasks: translating well-formed specifications into working code, finding syntax errors, knowing the standard library, recalling API patterns, iterating against test output. On those tasks, fighting Codex is a waste of your time.

You are better than Codex at a different class of tasks — tasks that don't look like tasks because they happen before any code gets written:

**Plausibility Auditing:** Knowing whether the output makes sense given the domain. Codex can write a sorting function that passes all tests and is still wrong for your specific use case in a way that only someone who understands the use case can catch.

**Problem Formulation:** Turning a vague goal into a specification precise enough that Codex can execute against it without filling the gaps badly. This is the work most students skip, which is why their Codex sessions go sideways.

**Tool Orchestration:** Deciding which tools to use and in what order — not just Codex, but the whole environment: tests, linters, version control, external APIs. Codex will reach for whatever tool it knows; deciding whether that's the right reach is human work.

**Interpretive Judgment:** Deciding when the technically correct solution is wrong for your context, your users, your constraints. Codex does not have your context. You do.

**Executive Integration:** Holding the project toward a goal across multiple Codex sessions, across days, when the scope has shifted and the original plan needs adjustment. Codex starts fresh each session. You don't.

| Capacity | What it means | What goes wrong when Codex substitutes for it |
|---|---|---|
| Plausibility Auditing | Hearing whether the output matches the domain before the tests can catch it | Passing code that's wrong for your use case ships forward — sorts run, GPAs round, nothing throws |
| Problem Formulation | Turning a vague goal into a specification precise enough to execute against | Codex fills the gaps from its training distribution, not from your actual constraints |
| Tool Orchestration | Choosing which tools — tests, linters, version control, external APIs — to bring in and in what order | Codex reaches for whatever it knows; the reach goes unaudited and the wrong tool gets normalized |
| Interpretive Judgment | Deciding when the technically correct answer is wrong for your users or context | Plausible, "correct" output that quietly violates the thing you actually cared about |
| Executive Integration | Holding the build's whole across multiple sessions, days, and scope shifts | Each session restarts the framing; drift accumulates and the original goal blurs |

The conducting discipline is the practice of exercising these five capacities deliberately, on every build, so they become reflex. That is why the book exists alongside the Codex documentation rather than replacing it. The documentation tells you what Codex can do. This book tells you what you need to keep doing yourself.

---

## Seth, eighteen months later

Seth is the co-author of this book. He is that AP CS student, eighteen months after the quiz — but the practice on which the discipline rests didn't happen in AP CS. It happened on his own builds.

Seth is a self-taught game developer in Troy, Missouri. By the time he sat for that quiz, he had already shipped a mobile arcade game to Google Play (*Bubble Pop*, with AdMob integration and full Play Console paperwork), built a Roblox/Luau horror game with cinematic intro and modular networked architecture (*Midnight Fuel*), and was mid-migration of a co-op horror survival game from Unreal Engine to Godot 4 (*Haunt & Harvest*) — a system-by-system rewrite of AI, inventory, and networked co-op. He runs *Zebonastic*, a Next.js platform on which he publishes weekly on horror game psychology and adrenaline mechanics. He works across GDScript, Lua/Luau, Python, JavaScript, Java, and C#. He used Codex on every one of those projects.

The conducting discipline in this book was worked out under that pressure: real deadlines on real software, with the agentic tool sitting next to him for hours at a time. The AP CS classroom is where the pattern *surfaced* — where he saw, in his friend's frozen quiz, the cost of borrowing capability. The discipline itself was built on the games.

The book is written in two voices. Seth's voice, when the chapter is doing narrative work or recounting a specific build moment. The author's voice, when the chapter is doing framework work. The shift is signaled in the text.

That distinction matters for what the book is doing. A framework written only by adults about how students should behave has a certain shape. A framework developed by a practitioner who noticed the pattern on his own work, tested the discipline on shipped builds, and then helped articulate the structure — that has a different shape, and a different authority. The discipline in this book has Seth's fingerprints on it specifically because he developed it under the constraints students actually have: deadlines, partial knowledge, teachers still calibrating to AI, classmates borrowing capability as fast as possible.

---

## What you will take from this book

Not a single capstone project mapped out from the start. By Chapter 14 you will have built whatever your conducted build chooses to be: a personal automation tool, a small web app, a data-cleaning script, a homework helper. The discipline applies; the project is yours.

What you will *have*, beyond the project:

An **AGENTS.md** for your own work — the file that encodes your project's context, constraints, and your preferences so that Codex is not starting from zero at every session.

A **vocabulary** for the five supervisory capacities, precise enough that you can use it in build logs and in conversation with other engineers — including the engineers you will eventually work alongside.

A **practice**: the Ask Mode → Code Mode gate, applied per significant change, until reviewing a plan before approving it is as automatic as reading a function signature before calling it.

A **post-build learning document** for your first full conducted build — five sections, honest, the artifact that converts the experience into the capacity to explain it to someone else. The Feynman test, applied to your own build.

---

## What would change my mind

The book rests on two load-bearing claims, and it is worth saying plainly what evidence would soften them.

The empirical claim: AI-assisted practice *without* the conducting discipline produces a measurable gap between practice performance and unassisted performance. The Bastani et al. number is 17 percentage points on the unassisted exam. If a large follow-up RCT — contemporary frontier model, multiple domains, adequate sample — fails to replicate the gap, the urgency of the book's argument drops. The discipline still helps; the case for it becomes pragmatic rather than empirical.

The operational claim: the Ask Mode → Code Mode gate, plus the five supervisory capacities, plus AGENTS.md, materially reduce the gap for students who apply them. If a controlled study compared students using this discipline to students using Codex without it and found no measurable difference in unassisted-task performance after a semester — the discipline becomes recommended practice rather than load-bearing infrastructure. The book's framing would need revision.

Either finding would require revising this chapter. That sentence is not rhetorical. It is what intellectual honesty looks like when you are making empirical claims.

---

## Still puzzling

Three things the book asserts but does not directly measure:

How much of the discipline transfers between agentic coding tools. The book is written for Codex. The companion books in this series teach the same discipline for Claude Code and for GitHub Copilot CLI. Whether the discipline practiced in one tool transfers cleanly to another is assumed rather than measured.

Whether the homework/quiz gap appears in non-CS subjects with the same shape. Bastani measured math. Kosmyna measured essay writing. Anthropic measured Python learning. The gap appears in every domain measured so far. Whether it appears at the same magnitude in AP Chemistry lab work, AP Literature analysis, or foreign language acquisition is an open question.

What the right age floor is for the conducting discipline. The book assumes technical fluency at roughly the AP CS level. Whether the framework works for younger students — or works differently — has not been tested.

---

## AI Wayback Machine

🕰️ **Norbert Wiener** (1894–1964) — mathematician who founded **cybernetics**, the study of control and communication in animals and machines. Wiener's question — *what does the machine do to the human who uses it?* — first asked in *The Human Use of Human Beings* (1950, revised 1954), is the question this book asks of Codex.[^1] Wiener was writing before the personal computer, before the agentic AI, before the homework/quiz gap had a name. The form of the question scales. The agentic coding tool is a control system in Wiener's sense; student-plus-Codex is a feedback loop; what the loop produces in the student is exactly what the book measures. Wiener believed — correctly, the studies suggest — that tools change the cognitive structure of the humans who use them, and that the discipline of using the tool well is what determines whether the change is augmentation or atrophy.

---

## Bridge

The feeling Seth had — watching the friend ace homework and freeze on the quiz — is real. Chapter 2 gives it a name, a number, and a neurobiological mechanism.

---

## Exercises

**Warm-up**

1. *(Targets: borrowed vs. built capability)* Before reading further: list three things you have built with AI assistance in the last month. For each one, ask yourself — could you reproduce the core logic right now, without the AI, if someone asked you to? Write "yes," "partially," or "no" next to each. Keep this list. You will return to it in Chapter 14.

2. *(Targets: the homework/quiz gap)* In one paragraph, describe the difference between using a calculator to solve a problem and learning arithmetic. Do not look anything up. Write from what you already think. Keep it — the same question appears at the end of the book, and comparing your two answers is the point.

**Application**

3. *(Targets: Ask Mode → Code Mode gate)* Think of the last time you used an AI tool to write or modify code. Reconstruct the sequence: did you review a plan before anything was executed, or did execution begin immediately? If you did review a plan, what did you actually check? If you didn't, what would you have needed to check to have a clear expectation of the output?

4. *(Targets: five supervisory capacities)* Take one of the three items from Exercise 1. Identify which of the five capacities — Plausibility Auditing, Problem Formulation, Tool Orchestration, Interpretive Judgment, Executive Integration — you exercised during that build, and which the AI exercised instead of you. One sentence per capacity is enough.

**Synthesis**

5. *(Targets: borrowing vs. building + gate discipline)* Seth's friend and Seth both used Codex. The difference was not which tool they used — it was what cognitive work each kept for themselves. Write a one-paragraph description of what a "conducted" version of your most recent AI-assisted coding session would have looked like. What would you have done differently at the start, in the middle, and before approving any execution?

**Challenge**

6. *(Targets: empirical claims + intellectual honesty)* The chapter names two load-bearing claims and says what evidence would soften them. Pick one claim. Design a study — in a paragraph, informally — that would actually test it. What would you measure? Who would the participants be? What would a null result look like, and what would a confirming result look like?

---

[^1]: Wiener, N. *The Human Use of Human Beings: Cybernetics and Society*. Houghton Mifflin, 1950; revised 1954. The 1954 edition is the standard citation.

---

## Prompts

Use these prompts with Claude to generate interactive D3 v7 versions of the figures in this chapter. Each produces a standalone HTML file you can open in a browser and modify freely.

**Prerequisites:** Load `brutalist/CLAUDE.md` and `brutalist/DESIGN.md` into your Claude project context before using these prompts. They define the stack, naming conventions, color system, and typography the figures use.

---

### Figure 1.1 — Seth's arc from observer to practitioner

Build a two-point timeline in D3 v7. A horizontal arrow runs across the canvas; two filled circles sit on it as anchors. Above each anchor place a small `--color-fill` card with a monospace ALL CAPS label (`OBSERVER`, `PRACTITIONER`) and a one-line bold title (`Watches friends`, `Builds the discipline`). Below each anchor write three short lines: two plain-text in `--color-secondary`, one italic. Left card describes the friend who froze on the quiz; right card describes the eighteen-months-later builder shipping games. Tooltips on each card give the longer story. Dashed horizontal divider beneath, then a single-line footer about what the rest of the book teaches.

> Reference implementation: `d3/01-introduction-cautious-builder-fig-01.html`

---

### Figure 1.2 — The Bastani RCT: practice score vs. unassisted exam

Build a small-multiples line chart in D3 v7. X axis: two categorical points — `Practice score`, `Unassisted exam`. Y axis: points relative to baseline, domain about `[-30, 55]`. Plot three lines connecting the two points: a flat slate line at zero (`No AI`), a near-flat `--color-mark` line slightly above zero (`AI + guardrails`), and a heavy `--color-red` line that runs from `+48` down to `-17` (`AI, no guardrails`). Use `scaleLinear` for Y, `scalePoint` for X. Endpoint dots, value labels at each endpoint, dashed zero baseline. A single italic annotation mid-chart names the 65-point divergence. Hovering any dot shows the group's reading and a short note. Footer caption cites Cohen's d and p-value.

> Reference implementation: `d3/01-introduction-cautious-builder-fig-02.html`

---

### Figure 1.3 — The Ask Mode → Code Mode gate

Build a two-node flow in D3 v7. Two rectangular `--color-fill` cards positioned left and right, each with a monospace ALL CAPS label (`ASK MODE`, `CODE MODE`), a bold title (`Plan, read, propose` / `Write, execute, iterate`), and two short lines of body text. Between them, a horizontal arrow with marker-end pointing right and an italic mid-arrow label `human reviews plan`. Hovering either card or the arrow label shows a tooltip explaining what that stage does. Dashed footer rule, then a one-line caption: the gate is the moment where the conductor listens before the orchestra plays.

> Reference implementation: `d3/01-introduction-cautious-builder-fig-03.html`

---

### Figure 1.4 — The three-act arc of the book

Build a horizontal timeline in D3 v7. A single arrow runs across the canvas. Three `--color-fill` cards sit above it, sized to suggest each act's chapter range — Act One narrower, Act Two wider, Act Three narrower. Each card has a monospace ALL CAPS label (`ACT ONE`, `ACT TWO`, `ACT THREE`), a bold title (`The problem`, `The discipline`, `The build`), a monospace chapter range in `--color-mark` (`CH 1 — 3`, `CH 4 — 10`, `CH 11 — 14`), and three short lines of description. Hovering any card shows the longer chapter-by-chapter detail. Dashed footer rule, then a single-line footer caption.

> Reference implementation: `d3/01-introduction-cautious-builder-fig-04.html`
