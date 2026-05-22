# Chapter 8 — Problem Formulation: The Mission Before the Build

*The most expensive mistake in an AI-assisted build happens before the first prompt.*

---

Here is a script that worked.

It read a class calendar. It produced notifications about upcoming assignments. It ran without errors. Every function did what it was asked to do. By any mechanical measure, the build was a success.

Seth used it for a week and then stopped using it.

The notifications arrived during the school day, when he could not check them. They covered assignments two weeks out — too far to act on — and missed the day-of reminders — too late to be useful. Each notification was about a single assignment in isolation, when what he actually needed was a morning summary: *here is what is due in the next 48 hours, prioritized.* The script was technically correct. It solved the wrong problem.

The hour Seth spent with Codex was not wasted because Codex failed. It was wasted because Seth had not asked himself, before typing the first prompt, what problem he was actually trying to solve. He had been thinking of the task as "build a homework reminder" when the real task was "build a daily 7am summary of imminent assignments." Those are different problems. Codex built the first one faithfully.

Five minutes upstream would have caught it.

---

## What problem formulation is

Formulation is the work of deciding what the build *is* before Codex sees it.

This sounds obvious. It is almost never done. The natural impulse when you have a clear idea in your head is to open the CLI and start prompting — the idea feels complete, the first prompt feels obvious, and the formulation work feels like delay. The impulse is wrong. The idea in your head is not complete. It is a sketch that feels complete because you are inside it.

The formulation makes the sketch explicit. Explicit means written down, in enough precision that someone who is not inside your head can tell whether the build they are looking at is the right one. Codex is not inside your head. The prompt is the only communication you have with it. If the prompt encodes the sketch rather than the formulation, Codex builds the sketch.

The discipline is a one-sentence test on three questions:

**What does this build do?** Not "build a homework reminder" — that is the sketch. "A script that reads my Google Calendar for events tagged 'homework,' filters to the next 48 hours, produces a prioritized summary, and delivers it to my notification system at 7am on school days" — that is the formulation.

**What does it touch?** The calendar (read-only). The notification system (write). Local config (read). These are the system boundaries. Knowing them in advance prevents the build from quietly acquiring dependencies you did not intend.

**What does it never touch?** The calendar's source. The grades file. The notification system's general settings. Everything out of scope should be named explicitly, because if you do not name it, Codex may reasonably include it.

<!-- → [TABLE: one-sentence test — three rows, two columns. Column 1: question ("What does it do?", "What does it touch?", "What does it never touch?"). Column 2: Seth's sketch answer vs. Seth's formulation answer for each. Student should see exactly where the sketch breaks down and what the formulation adds.] -->

If you can answer each in one sentence — short enough to fit on a notecard — you have formulated the problem. If you cannot, the formulation is not done. The chapter's central rule:

**No Code Mode prompt until the one-sentence test passes on all three questions.**

---

## Why formulation fails silently

The Seth story is not unusual. It is the typical failure mode. And the reason it is typical is that the sketch genuinely feels like a formulation from inside. The feeling of clarity is not evidence of clarity. It is the fluency trap in yet another register.

When you have been thinking about a problem for a while, you have accumulated a private context — the timing constraints, the use case, the specific morning workflow that would make the tool useful. That context is in your head. It is not in the prompt. Codex, reading the prompt, fills in the missing context with the most-probable interpretation. The most-probable interpretation of "homework reminder" is something that reminds you about homework. It is not wrong. It is not what you needed.

The gap between the most-probable interpretation and your interpretation is usually small and invisible until the build runs. Then it is expensive: you have a working script that does the wrong thing, a session context that has accumulated around the wrong frame, and the cost of revision is no longer the cost of a sentence — it is the cost of discarding ten Code Mode responses and starting again.

Formulation is cheap because it happens before any of that accumulation. A sentence revised costs nothing. A Code Mode session scrapped costs an hour.

---

## The Ask Mode interrogation

The fastest path to a formulation is not staring at a blank page. It is asking Codex, in Ask Mode, to surface what you have not thought of.

Five questions that work:

*"What should I think about before building X?"* Surfaces considerations you have not had. For Seth's reminder, this would have surfaced timing, aggregation, and the school-hours problem before the first Code Mode prompt.

*"What are common failure modes in this kind of build?"* Flags the traps practitioners have already fallen into. "Homework reminders that send too many notifications become noise within a week" is the kind of thing Codex can surface from the practitioner literature. Seth did not know this. He found it out by living it.

*"What edge cases should I plan for?"* Reveals where the spec needs to be more precise. "What happens on a day with no assignments?" is not a question Seth would have asked himself. It is the question that determines whether the morning notification says "nothing due" or simply does not arrive.

*"What's the typical shape of this kind of project?"* Gives you the average so you can decide where to deviate. Most homework-reminder implementations are per-assignment, not summary-based. Knowing the average makes the choice to deviate explicit rather than accidental.

*"What's the minimal version of this that would be useful?"* Cuts scope ruthlessly before Code Mode begins. Seth's full spec turned out to have a useful minimal version: one Python script, one cron job, one Pushover notification. Everything else was out of scope.

The interrogation typically takes 5–15 minutes. It does not replace the formulation — it feeds it. You read the Ask Mode responses, you find the two considerations you had not thought of, you revise the sketch into a formulation. Then you write it down.

---

## The minimum viable spec

For anything that will take more than an hour to build, the one-sentence test is necessary but not sufficient. The answers need a container.

The minimum viable spec is that container. Five sections, half a page, 10–15 minutes to write:

```markdown
# Spec — [build name]

## Problem
What does this build do? One paragraph.

## Architecture
The top-level design decisions. What components, what data flows.
Three architecture principles maximum.

## User flows
What does the user do with it?
The key interactions, in order.

## User needs
Testable outcomes. Not "it should be fast" — "the morning summary
delivers at 7am ± 30 seconds." Not "it should be easy to use" —
"setup takes under 10 minutes for someone who has never seen the project."

## Out of scope
What this build does NOT do.
```

The User needs section is where most specs fail. Feature descriptions ("it should be fast," "it should be readable") are not testable. Testable outcomes are specific, measurable, and falsifiable: "the summary arrives within 30 seconds of 7am," "the summary contains every assignment due in the next 48 hours and no assignment due later," "the summary is one notification, not one per assignment."

The difference matters because the Code Mode prompts can reference the User needs section directly: *"Per the User needs section: ensure the summary is under 200 characters."* The spec is the persistent context the build executes against. Without it, the persistent context is the accumulated Code Mode session — which drifts.

<!-- → [TABLE: feature descriptions vs. testable outcomes — two columns, six rows. Left column: vague feature descriptions ("it should be fast," "easy to use," "readable," "complete," "reliable," "simple"). Right column: testable reformulations of each. Student should be able to convert any feature description from column one into a testable outcome using column two as a model.] -->

---

## The economics of formulation

A useful way to think about why the discipline is worth it.

Mistakes in a build have a cost that scales with *when in the build they are caught*. The same mistake — the wrong framing — costs different amounts depending on where you catch it.

During formulation, the cost is zero. You change a sentence.

After the first Code Mode prompt, the cost is one discarded response.

After ten Code Mode prompts, the cost is ten discarded responses plus a session context that has accumulated around the wrong frame. You are not just restarting; you are starting from a confused position.

After side effects have run — files written, APIs called, state changed — revision may not be fully possible. You are debugging a build that was solving the wrong problem while also dealing with the mess the build left behind.

<!-- → [CHART: cost-of-revision curve — x-axis: stage of build (formulation, first prompt, tenth prompt, after side effects). y-axis: cost of catching a framing mistake. Curve rises steeply after "first prompt" and approaches "very high / irreversible" at "after side effects." Student should see that the cheap window is narrow and closes fast.] -->

The formulation work is front-loaded discipline that produces back-loaded savings. Seth's lost hour would have been a ten-minute Ask Mode interrogation and a five-minute spec, followed by a 45-minute build that produced a script he actually used. The ratio is not subtle. **Problem formulation is the most efficient discipline in the book.**

The reason it is skipped is not that it is hard. It is that it feels like delay when you have a clear idea. The impulse to start building is strong. The impulse is wrong whenever the build will take more than an hour, because the formulation work will always cost less than the cost of scrapping a misdirected build.

---

## The homework reminder, done right

Same task. Formulated properly.

**Ask Mode interrogation.**

Seth asks Codex what to think about before building a homework reminder system. The response surfaces four things: timing matters more than granularity (students do not act on mid-day notifications during school hours); aggregation reduces fatigue (one morning summary beats ten individual notifications); prioritization by urgency is more useful than chronological order; 48 hours is the useful window, further out becomes noise.

Two of those four Seth had not thought of. He shifts his framing.

**One-sentence test.**

*What does it do?* A daily 7am summary script for school-day mornings, covering the next 48 hours, prioritized by urgency, delivered as a single notification.

*What does it touch?* Calendar (read), notification system (write).

*What does it never touch?* Calendar source, non-homework events, weekends by default.

The test passes. He writes the spec. He commits it to the project.

**Code Mode build.**

Each prompt references the spec. The handoff conditions test against the User needs section. The build takes 45 minutes. The script lands at 7am the next morning. Seth checks it on the way to school. He knows what is due. The script serves the need.

The formulation produced a build that worked the first time it was used. The difference between Path A (one hour, unused script) and Path B (ten minutes upstream, 45-minute build, useful tool) is not Codex's behavior — Codex performed identically in both. The difference is the framing it was given.

<!-- → [INFOGRAPHIC: side-by-side timeline — Path A and Path B. Path A: 60-minute Code Mode build → unused script → wasted hour. Path B: 10-minute Ask Mode interrogation + 5-minute spec → 45-minute Code Mode build → working tool used daily. Annotate the formulation window in Path B to show where the investment is made.] -->

---

## What formulation does not protect against

Formulation catches the case where the frame is wrong before the build begins. It does not catch everything.

It does not catch the case where the frame is right but a specific implementation detail is wrong — a function that does the right thing with the wrong argument shape, a data structure that is correct in theory and slow in practice. Those are caught by the gate from Chapter 5 and the handoff conditions from Chapter 9.

It does not catch scope creep that happens *during* the build — the temptation to add the weekend delivery feature while you are in Code Mode anyway. That is caught by the Out of scope section of the spec, and by the discipline of treating the spec as a commitment rather than a suggestion.

It does not eliminate iteration. The first formulation is usually close but not final. The Ask Mode interrogation surfaces some of what you did not know; the first Code Mode session surfaces the rest. The book's prescription is to iterate on the formulation before Code Mode, not to achieve perfection before the first prompt. Close-but-final is good enough. The goal is to eliminate the case where the frame is *wrong*, not to achieve the case where the frame is *perfect*.

---

## What would change my mind

The chapter's strong operational claim: upstream formulation using Ask Mode interrogation and a minimum viable spec produces materially better outcomes on multi-step builds than starting directly from Code Mode prompts.

What would soften that claim: a controlled comparison — same set of multi-step builds, with and without upstream formulation — that found no measurable difference in outcome quality or build time. I expect the difference to be substantial on anything that takes more than an hour. I expect it to be negligible on one-prompt tasks. The book's prescription scales with consequence.

What remains open: the exact threshold below which formulation is overhead. The hour-of-build heuristic is approximate. Some shorter builds with high stakes warrant more formulation; some longer ones with low stakes warrant less. The right threshold varies with experience and with the reversibility of the build's side effects.

---

## What is still puzzling

**Whether Ask Mode is the right interrogation tool.** Some practitioners prefer to interrogate the problem in their own head or in a notes file, without involving the CLI. Both are defensible. The book uses Ask Mode because it surfaces considerations the practitioner would not have generated independently — the practitioner's blind spots are exactly where the useful surfacing happens. But if you are experienced enough with a class of build that you have already internalized the common failure modes, the notes-file approach is faster.

**The relationship between the per-build spec and the per-project AGENTS.md.** A well-maintained AGENTS.md encodes the lessons and rules from previous builds. It does some formulation work automatically — the project conventions, the scope boundaries, the known failure modes are already in the file. Whether this reduces the need for per-build formulation or only complements it is open. The book's working answer: complements. AGENTS.md handles the project-level context; the per-build spec handles the build-specific decisions. Both, each in their place.

**When to iterate on the formulation vs. when to commit.** The first formulation is rarely perfect. The question is when to stop refining and start building. The book's heuristic: commit when the one-sentence test passes and the User needs section contains no feature descriptions. If those two conditions are met, the remaining uncertainty is better resolved by a careful Code Mode step than by more formulation time.

---

## AI Wayback Machine

🕰️ **Frederick Brooks** (1931–2022) — software engineer and author of *The Mythical Man-Month* (1975) and "No Silver Bullet" (1986).[^1] Brooks's central argument was that the difficulty of software engineering has two components: *accidental* difficulty (the typing, the syntax, the compilation, the debugging tools) and *essential* difficulty (the intellectual work of deciding what the system should do). Every generation of tools — assemblers, compilers, IDEs, version control — has reduced accidental difficulty. None has reduced essential difficulty, because essential difficulty is not a tool problem. It is a thinking problem.

Codex is the most powerful accidental-difficulty reducer in the history of the field. It writes the code. It fixes the syntax. It proposes the architecture. It finds the bug. The accidental difficulty of producing a working program has dropped to near zero for anyone who can formulate the problem well.

Brooks saw this coming, conceptually, forty years before the tools existed. *No Silver Bullet* predicted that no single development — including AI — would reduce the essential difficulty of software engineering, because the essential difficulty is not the code. It is the judgment about what the code should do. The formulation is that judgment. Brooks is still right. The chapter is Brooks applied to Codex.

---

## Bridge

You have a formulation. The next chapter teaches you to write the Codex prompts that are *specifications* — the prompts that convert the formulation into code Codex can produce reliably, step by step, with handoff conditions that catch what the formulation left open.

---

## Exercises

**Warm-up**

1. *(Tests: one-sentence test)* Apply the three-question one-sentence test to the following sketch: "Build a script that tracks my reading." Write one sentence for each question — what it does, what it touches, what it never touches. Then identify the single most important thing you had to decide in order to write each sentence.

2. *(Tests: feature descriptions vs. testable outcomes)* Rewrite the following User needs as testable outcomes: (a) "it should be easy to set up," (b) "it should be fast," (c) "it should handle edge cases gracefully." For each, state what you would measure and what threshold would count as passing.

3. *(Tests: what formulation is not)* A classmate says: "My prompt was really detailed — three full paragraphs. That counts as formulation." What is the difference between a detailed prompt and a formulation? What would you ask the classmate to find out whether the formulation has actually been done?

**Application**

4. *(Tests: Ask Mode interrogation)* Choose a project you are planning or have recently completed. Run the five Ask Mode questions from the chapter against it. Write down the single most useful thing each question surfaced. Did any question surface something you had not considered? Which one, and what was it?

5. *(Tests: minimum viable spec)* Write a minimum viable spec for one of the following builds: (a) a script that backs up your project files to a cloud folder every night, (b) a CLI tool that checks whether your assignment is under the word limit before you submit, (c) a personal tool of your choice. The spec must have all five sections. The User needs section must contain no feature descriptions — only testable outcomes.

6. *(Tests: economics of formulation)* A build takes 90 minutes in Code Mode and produces a working script. You then realize the frame was wrong — the script solves the right problem but delivers the output in the wrong format for your actual workflow. Estimate the total cost of catching this error at this stage versus at the formulation stage. Account for: time discarded, session context, any state that may have been changed. Then state what formulation move would have caught the framing issue before Code Mode began.

**Synthesis**

7. *(Tests: formulation + gate + handoff conditions together)* The chapter distinguishes three layers of discipline: formulation (Chapter 8), the Ask Mode → Code Mode gate (Chapter 5), and handoff conditions (Chapter 9). For Seth's homework reminder build, identify which errors each layer would have caught — and which errors fall through all three layers. What is the residual risk after all three disciplines are applied?

8. *(Tests: sketch vs. formulation in the wild)* You are given a project brief written by another student. Identify the parts of the brief that are sketch and the parts that are formulation. For each sketch element, write the Ask Mode question you would use to convert it to a formulation. Explain why the formulation version is more useful to Codex than the sketch version.

**Challenge**

9. *(Open-ended)* The chapter argues that the hour-of-build threshold is the right heuristic for when formulation is worth the investment. Design a better decision rule — one that accounts for reversibility of side effects, stakes of the outcome, and your prior experience with this class of build. Your rule should be checkable in under a minute at the start of a session. Explain what the hour heuristic gets right and where your rule improves on it.

---

[^1]: Brooks, F. P. *The Mythical Man-Month*. Addison-Wesley, 1975. See also "No Silver Bullet: Essence and Accidents of Software Engineering." *IEEE Computer* 20, no. 4 (1987): 10–19.
