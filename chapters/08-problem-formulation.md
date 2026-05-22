# Chapter 7 — Problem Formulation: The Mission Before the Build

> The most expensive mistake in an AI-assisted build happens before the first prompt. Formulate the problem first.

---

## Learning outcomes

1. **(Understand)** Explain why problem formulation is the most important step in a conducted build.
2. **(Apply)** Use Ask Mode to interrogate a problem before writing a single specification prompt.
3. **(Analyze)** Identify the sections of a problem brief most likely to reveal a formulation gap.

---

## Opening

Seth set out to build a homework reminder.

He spent about an hour with Codex in Code Mode. He described what he wanted — a script that would read his class calendar and send him notifications about upcoming assignments. Codex produced a script. The script worked. It read the calendar. It produced notifications.

Then Seth used it for a week.

The notifications were not the notifications he wanted. The script told him about assignments two weeks out (too far ahead to be useful) and missed the day-of reminders (too late). The notifications arrived during the school day when he could not check them, instead of at the times when he could. Each notification was about a single assignment in isolation, when what Seth actually needed was a morning summary of "here is what's due in the next 48 hours, prioritized."

The script was technically correct. The script did what Seth had asked for. The *problem Seth had asked for help with* was the wrong problem — he had been thinking of the task as "build a homework reminder" when he should have been thinking of it as "build a daily morning summary of imminent assignments with same-day priority."

The hour with Codex produced a script that did not serve the need. The script went unused. The hour was wasted.

Five minutes upstream, before Codex saw the project, would have caught the framing issue. *"What am I actually trying to solve? What kind of reminder serves me? When do I check my notifications? What's the right level of aggregation?"* These are problem-formulation questions. They happen before the suggest, not during.

This chapter is the five minutes upstream.

<!-- → [DIAGRAM: The problem formulation gate — one sentence naming what the build is, where it sits, and what it produces. Below the gate: specification prompts. Above the gate: nothing.] -->

---

## What problem formulation is

Problem formulation is the work of *deciding what the build IS* before Codex sees it.

For an AI-assisted build, the formulation answers three questions:

1. **What does this build do?** Not "build a homework reminder" but "build a daily morning summary script that reads my calendar and produces a prioritized list of assignments due in the next 48 hours, delivered to my notification system at 7am."
2. **What does it touch?** The calendar (read-only). The notification system (write). Local config (read).
3. **What does it never touch?** The calendar's source (do not modify upstream). My grades file (out of scope). The notification system's general settings (only my notifications, only at my schedule).

If you can answer each in one sentence — short enough to fit on a notecard — you have formulated the problem. If you cannot, the formulation is not finished, and the Code Mode build will produce a script that solves the wrong problem.

The chapter's central operational rule:

> **No Code Mode prompt until the formulation passes the one-sentence test on all three questions.**

The rule is short, hard to remember to apply, and the single highest-return discipline in the book. The chapter is how to apply it.

---

## The Ask Mode interrogation

`Ask Mode` is the formulation tool. Use it *before* you commit to a frame.

Useful interrogation questions:

- **"What should I think about before building X?"** — surfaces considerations you have not had.
- **"What edge cases should I plan for in this kind of build?"** — flags failure modes you might miss.
- **"What's the typical shape of this kind of project?"** — gives you the average so you can decide where to deviate.
- **"What are common mistakes when building X?"** — preempts the dangerous middle.

For Seth's homework reminder, an Ask Mode interrogation might have produced:

- *"What kinds of reminders are most effective for high school students managing assignments? Daily summaries, per-assignment notifications, due-date countdowns, or something else?"*
- *"What considerations matter when sending notifications: timing, frequency, granularity, modality?"*
- *"What are common ways homework-reminder systems fail to be useful?"*

Codex would have surfaced timing and granularity issues. Seth would have read them. The framing would have shifted from "homework reminder" to "daily morning summary at 7am." The Code Mode build would have produced the script that actually served the need.

The Ask Mode interrogation typically takes 5–15 minutes. The Code Mode build it enables runs cleaner because the framing is right. The trade is dramatically favorable for builds that take more than an hour.

---

## The minimum viable spec

For builds where the one-sentence test feels too compressed, the next-step-up is a short written spec.

```markdown
# Spec — [build name]

## Problem
What does this build do? One paragraph.

## Architecture
The top-level design decisions. What components, what data flows.
Three architecture principles maximum.

## User flows
What does the user (or you, if it's a personal tool) do with it?
The key interactions, in order.

## User needs
Testable outcomes. Not "it should be fast" — "the morning summary
delivers at 7am ± 30 seconds." Not "it should be easy to use" —
"setup takes under 10 minutes for someone who has never seen the project."

## Out of scope
What this build does NOT do. Saving things for later. Adjacent
features that would be useful but are not this build.
```

Five sections. Half a page. 10–15 minutes to write. The spec is the formulation in operational form. Once it exists, Code Mode prompts can reference it.

For a personal tool like Seth's homework reminder, the spec might be:

```markdown
# Spec — Morning Homework Summary

## Problem
A script that reads my class calendar and produces a daily morning
summary of assignments due in the next 48 hours, delivered to my
notification system at 7am on school days.

## Architecture
- One Python script, run by cron at 7am Mon-Fri.
- Reads from my Google Calendar via the calendar API.
- Writes to my notification system via the Pushover API.
- No persistent state between runs.

## User flows
1. Cron triggers the script at 7am.
2. Script reads calendar events tagged "homework" with due dates in
   the next 48 hours.
3. Script produces a prioritized summary (overdue first, then today,
   then tomorrow).
4. Script sends the summary as a single notification.

## User needs
- Summary arrives within 30 seconds of 7am Mon-Fri.
- Summary contains every assignment due in the next 48 hours and no
  assignment due later.
- Summary is one notification, not one per assignment.
- Summary is readable on phone lock screen (under 200 characters).

## Out of scope
- Reminders for non-homework events.
- Snoozing or marking-done from the notification.
- Weekend delivery (Sat/Sun only on request).
- Per-class filtering (all classes, every day).
```

The spec is detailed enough to govern the Code Mode build. The Code Mode prompts reference sections of the spec: *"Per the User needs section: ensure the summary is under 200 characters and contains every assignment in the 48-hour window."* The spec is the persistent context the build executes against.

---

## What problem formulation protects against

A useful framing: formulation is the only step where mistakes are *cheap*. After you commit to a frame and start prompting, the cost of revising the frame grows quickly.

- **During formulation:** revising is free. You change the sentence or the spec section.
- **After the first Code Mode prompt:** revising means discarding one Codex response. Small cost.
- **After ten Code Mode prompts:** revising means discarding ten responses *and* starting from a context that has accumulated bias from the previous frame. Medium cost.
- **After you have run anything with side effects:** revising may not be possible. State has changed.

The formulation work is *front-loaded discipline that produces back-loaded savings*. Seth's lost hour on the homework reminder would have been a five-minute Ask Mode interrogation followed by a clean build. Five minutes upstream saved an hour of misdirected work plus a week of trying to use a script that did not fit.

The ratio is dramatically favorable in either direction. **Problem formulation is the most efficient operational discipline in the book.**

---

## Worked example: the homework reminder, done right

Same task as the opening, formulated properly.

**Ask Mode interrogation (10 minutes):**

Seth asks Codex about effective reminder design for student assignment management. The CLI surfaces:

- Timing matters more than granularity. Most students do not act on mid-day notifications during school hours.
- Aggregation reduces fatigue. A morning summary beats ten individual notifications.
- Prioritization (by urgency) is more useful than chronological listing.
- 48 hours is the useful window for most students; further out becomes noise.

Seth reads. Two of the four surfaced considerations are things he had not thought of. He shifts his framing.

**Formulation (one-sentence test):**

- *What does it do?* A daily 7am summary script for school-day mornings.
- *What does it touch?* Calendar (read), notification system (write).
- *What does it never touch?* Calendar source, non-homework events, weekends.

The one-sentence test passes. He writes the minimum viable spec (shown above). He commits the spec to the project.

**Code Mode build (45 minutes):**

The build references the spec at each step. The handoff conditions (Chapter 9) test against the User needs section. The build produces a working script. Seth uses it the next morning. The summary lands at 7am. Seth checks it on the way to school. He knows what is due. The script serves the need.

**The lesson:** the 10 minutes upstream produced a 45-minute build that worked, instead of a 60-minute build that did not. The formulation was the difference.

**The limit:** formulation does not catch the case where the framing is *right* but a specific implementation detail is wrong. Those are caught by the gate (Chapter 4) and the handoff conditions (Chapter 9). The discipline is layered.

---

## Common misconceptions

**"I'll figure out the formulation as I build."** Sometimes works. Fails for anything multi-step. The cost of formulating-in-place is consistently higher than the cost of formulating-upstream.

**"My prompt was clear; the CLI got it wrong."** Usually the prompt was clear *to you* but vague to the CLI. Formulation is the work of writing prompts that are clear from outside your head.

**"The minimum viable spec is over-engineering."** For a one-prompt fix, yes. For anything that will take more than an hour, the spec saves time. The threshold is the hour mark, roughly.

**"`gh copilot ask`-style interrogation just delays me."** It moves the decision-making upstream. Decisions made upstream cost less than decisions made during execution. The total time is usually less.

**"My formulation will be wrong the first time anyway."** Usually right. Iterate. The first formulation is the starting point; the second formulation, after one cycle of Ask Mode reveals what you missed, is usually close.

---

## Exercises

1. **(Apply)** Use Ask Mode to interrogate a project you are planning. Take the answer seriously — note one consideration the CLI surfaced that you had not thought of.

2. **(Apply)** Take a recent build that did not go as planned. Reformulate using the one-sentence test for the three questions. Compare to your original framing. What changed?

3. **(Analyze)** A provided minimum viable spec has a weak User needs section. Identify which needs are feature descriptions and rewrite them as testable outcomes.

---

## What would change my mind

The chapter's strong operational claim is that **upstream formulation produces materially better build outcomes** than starting from Code Mode prompts. If a controlled comparison — same set of multi-step builds, with and without upstream Ask Mode + formulation — found no measurable difference in outcome quality or build time, the formulation step becomes optional rather than load-bearing. The chapter would still recommend it for the supervisory-practice benefit; the urgency drops.

I expect the difference to be substantial on multi-step builds and negligible on trivial one-prompt tasks. The book's prescription scales with consequence.

---

## Still puzzling

- **The exact threshold below which formulation is overhead.** The hour-of-build heuristic is approximate. Some shorter builds with high stakes warrant more; some longer ones with low stakes warrant less. The right threshold per person varies with experience.

- **Whether Ask Mode is the right interrogation tool.** Some practitioners prefer to interrogate the problem in their own head or in a notes file, without involving the CLI. Both are defensible. The book uses Ask Mode because it surfaces considerations the practitioner might not have generated independently.

- **The relationship between the spec (Chapter 7) and AGENTS.md (Chapter 6).** The spec is per-build; AGENTS.md is per-project. A well-maintained AGENTS.md does some of the formulation work — the rules and lessons are already encoded. Whether AGENTS.md reduces the need for per-build formulation, or whether it complements it, is open. The book's working answer: complements. Both, each in their place.

---

## AI Wayback Machine

🕰️ **Frederick Brooks** (1931–2022) — software engineer whose *The Mythical Man-Month* (1975) and *No Silver Bullet* (1986) established that the most expensive bugs in software are bugs of *what was built*, not *how it was built*.[^1] Brooks's argument was that the cognitive work of *deciding what the system should do* is the irreducible difficulty of software engineering — and no tool eliminates it. Compilers, debuggers, IDEs, and now AI coding assistants reduce the *accidental* difficulty (the typing, the syntax, the wiring). They leave the *essential* difficulty — the formulation work — exactly where Brooks left it: with the human.

The chapter is Brooks applied to Codex. Codex reduces the accidental difficulty of writing code; the essential difficulty — deciding what the code should do, in this project, with this scope, given these constraints — remains yours. Brooks's *Mythical Man-Month* warned that the most expensive bugs come from building the wrong thing. The chapter's formulation discipline is the operational form of building the right thing.

---

## Bridge

You have a problem formulation. Chapter 8 teaches you to write the Codex prompts that are *specifications*, not requests — the prompts that convert the formulation into code Codex can produce reliably.

---

[^1]: Brooks, F. P. *The Mythical Man-Month*. Addison-Wesley, 1975. See also "No Silver Bullet: Essence and Accidents of Software Engineering." *IEEE Computer* 20, no. 4 (1987): 10–19.
