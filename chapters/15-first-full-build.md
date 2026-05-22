# Chapter 15 — Your First Full Build: From Problem to Verified Output

*You have the discipline. Here is the project. Run it.*

---

Not Seth's build.

Yours.

There is a particular moment in learning physics — Feynman described it, and every physicist recognizes it — when the equations you have been reading stop being marks on a page and become something you can see. You are not reading about the problem anymore. You are inside it. The symbols have weight. The variables have meaning that they did not have when you first encountered them.

That moment does not come from reading more carefully. It comes from the first time you sit down with a problem you did not design and work through it without a safety net. The framework becomes yours at the moment you use it on something real.

This is that moment.

---

## The Shape of the Project

The chapter does not prescribe a specific project. It prescribes a shape, and the choice within that shape is yours.

The shape has four requirements.

**Multi-step.** At least five Code Mode prompts. Below this threshold, the planning gate is overhead and the handoff-condition discipline has nothing to exercise against. The dangerous middle does not show up in one-prompt builds. It shows up in builds with enough steps that an assumption made in step two becomes a silent wrong answer in step five.

**Has aesthetic dimensions.** Either visual — a small web app, a data visualization — or stylistic — a tool whose output another human reads, where formatting and voice and structure all matter. The aesthetic dimension forces IJ in a way that purely mechanical tasks do not. When the output has to be good for a person, not just correct for a machine, you cannot accept fluent-but-wrong.

**Touches your real life.** Something you would actually use or share. A personal automation, a small portfolio piece, a tool for a class or a club. The stakes do not have to be high. They have to be real enough that you notice when the output is wrong for your situation.

**Scope-appropriate.** Doable in a three-to-five hour single sitting or across two evenings. Not a semester project. Not a one-prompt exercise. The scope is calibrated to the discipline's cost: enough steps to exercise every capacity, short enough that the first conducted build does not become its own blocking obstacle.

<!-- → [TABLE: Project shape requirements — four rows, two columns: requirement / what it exercises. Multi-step → dangerous middle and handoff conditions; Aesthetic dimensions → IJ on output quality; Real life → PA on situational correctness; Scope-appropriate → full discipline without scope-creep. Quick reference for evaluating your project choice.] -->

Candidate projects from past students, to make the shape concrete: a personal-finance summary tool that processes bank CSVs into monthly summaries; a reading-list manager that tracks what you have read and what friends recommended; a study-tracker for a single class that identifies the topics where you have spent the least time; a small portfolio website; a homework-summary script that reads your calendar and produces a morning briefing; a vocab study tool for a class in a language you are learning.

Pick one of these, or pick something else that fits the shape. The brief is orientation. The choice is yours.

---

## The Sequence, Applied Without Scaffolding

Here is the book in fourteen steps. Each step names the chapter where the discipline lives. Apply each to your project.

**1. Ask Mode interrogation** (Chapter 7). Before you commit to a frame, investigate. Ask Codex what considerations matter for a problem like yours. Note what surfaces that you had not thought of. This is where Seth discovered the net-worth view. This is where the spec becomes possible.

**2. One-sentence problem formulation** (Chapter 7). Three sentences: what does this build do, what does it touch, what does it never touch. If you cannot write each in one sentence, the scope is not small enough yet.

**3. Minimum viable spec** (Chapter 7). Five sections: problem, architecture, user flows, user needs, out of scope. Half a page. The spec is a contract, not documentation. Everything the build does traces to this half-page.

**4. AGENTS.md** (Chapter 6). The persistent record that makes EI scale across the session. Write it before you build. Revise it twice during. The first version will be incomplete; the build will surface what you did not know to write down.

**5. DESIGN.md and PROJECT.md** if the project has creative or aesthetic dimensions (Chapter 10). If the output has a voice, a format, a visual identity — those decisions belong here, not distributed across prompts.

**6. Ask Mode plan** (Chapter 12). Ask Codex to propose an implementation plan against the spec. Codex drafts; you edit.

**7. Plan review** (Chapter 12). Find at least one assumption Codex got wrong. Correct it. A plan approved without correction is a plan you have not read.

**8. First Code Mode prompt with five-element specification** (Chapter 9). The specific task, the invariants, the context, the output format, the negative constraint. The specification is the mechanism by which this prompt generates output for your situation rather than for the average situation.

**9. Read the output; predict before reading** (Chapter 6, PA). Before you run the code, predict what it will do. The prediction is not always right. The gap between your prediction and the result is information.

**10. Handoff condition check** (Chapter 13). The handoff condition was specified in the prompt. Did the output meet it? This is mechanical verification — not judgment, just checking.

**11. Plausibility audit on the result** (Chapter 6, PA). Does the result feel right in the context of your project? If PA fires, investigate before accepting.

**12. Update AGENTS.md with lessons learned** (Chapter 6). What did this step teach you that belongs in the persistent record? Even one entry per step accumulates into a project memory that the next build benefits from.

Steps 8 through 12 repeat for every step in the build. Steps 1 through 7 happen once, upstream. Steps 13 and 14 happen once, at the end.

**13. Three-pass verification** (Chapter 13). After the build is complete: the mechanical pass (does it run, do the tests pass), the plausibility pass (does the whole thing feel right for the actual use), the dangerous-middle pass (what could be silently wrong that the tests did not catch).

**14. Post-build learning document** (Chapter 13). Five sections. One page. Written now, at the end of the build, before the lessons fade.

<!-- → [DIAGRAM: The fourteen-step sequence as two phases — upstream (steps 1–7, happens once) and per-step loop (steps 8–12, repeats) and closing (steps 13–14, happens once). Flowchart with the loop explicitly shown. Shows the structure of the build before the reader is inside it.] -->

---

## What Success Looks Like

Not a perfect build.

The discipline does not produce perfect builds. It produces builds where the failures are visible, catch-able, and recoverable. The measure of a successful first conducted build is not zero errors. It is whether you can account for the errors that occurred, what the discipline did or did not catch, and what you would do differently.

Specifically, success looks like this.

You can account for every Codex prompt that ran. What it asked for, why you asked it at that step, what alternative you considered, what you would change about the specification in hindsight.

Your AGENTS.md grew during the build. At least one entry was added after the build surfaced something you did not know to write in advance. The file is not the spec you wrote before you knew what the project would teach you. It is the spec plus what you learned.

Your post-build document contains a specific "what I would do differently" that names a real decision you would reverse — not a generic "I would plan more carefully" but a specific step, a specific assumption, a specific negative constraint you wish you had written.

The supervisory capacities were labeled in the build log at the major moments. PA catches. PF revisits. EI integration checks. Not every step needs a label. The moments that mattered do.

The project works for the use you built it for.

A build that meets these criteria is the chapter's success. Whether it took three hours or six is less important than whether it taught you what the discipline costs and what it provides.

<!-- → [TABLE: Success criteria checklist — five rows, one per criterion. Columns: criterion / what it looks like if met / what it looks like if skipped. Designed as a self-assessment tool after the build is complete.] -->

---

## What It Will Probably Not Be Like

A few things to expect, because the first conducted build is not what students expect it to be.

The first attempt will produce at least one failure the discipline catches. A handoff condition that fails on first run. A plausibility audit that fires on something the spec did not anticipate. A revert-and-respecify on a step where the second specification was needed because the first one was still ambiguous. These are not signs of a failed build. They are signs of the discipline working. The failure surfaced in the plan review or the handoff check or the plausibility audit, not at hour four when the output is wrong in a way that takes a day to unwind.

The build will take longer than you estimate. The first conducted build always does. The planning takes more time than expected because the formulation is genuinely hard. The plan review surfaces more corrections than expected because reading a plan critically is a skill that takes practice. The post-build document takes longer than expected because writing "what I would do differently" requires actually deciding what you would do differently, which requires more reflection than it sounds. By the third conducted build, each of these steps is faster. The first one is not.

You will be tempted to skip the post-build document. Write it now, at the end of the build, before the lessons fade. Thirty minutes. One page. The document is the artifact that consolidates what the build taught you into something the next build benefits from. Without it, the learning is in the experience and not recoverable. With it, the learning is in the record.

Your AGENTS.md will be revised at least twice during the build. The first version will be incomplete; the build will surface things you did not know to write down. This is the correct behavior of AGENTS.md. It is not a spec you write once before the build; it is a record you maintain through it.

---

## Why This Step Cannot Be Skipped

There is a principle Dewey articulated in 1938 that is still the best description of what happens at this step.

Dewey's claim was that cognitive structures — the things that constitute durable learning, as opposed to temporary retention — are built through engaged practice on real tasks, not through reading about practice or being told about practice.[^1] The reading is necessary. It builds the framework. But the framework becomes operative only when you use it on something that is not a worked example. The gap between understanding and practice is closed by building, not by reading more carefully.

The post-build learning document is the record of this transformation. Not the record of what you built — the record of what the build built in you. The working project is verification that the discipline produced something. The document is the primary artifact.

This is why the chapter exists as a chapter and not as an appendix. The capstone is where the framework becomes yours. Chapters 1 through 13 built the framework. Chapter 14 makes it practice. A practitioner who has read all thirteen chapters but not built is a different person, with a different capability, than a practitioner who has read thirteen chapters and built once. The build is the step that closes the distance.

<!-- → [INFOGRAPHIC: Two practitioners side by side — left: has read chapters 1–13 but not built; right: has read chapters 1–13 and completed the capstone. Contrast their relationship to the five supervisory capacities: framework known vs. framework practiced. Simple two-column layout, no data, conceptual only.] -->

---

## What You Have

By the end of this chapter, you have five things.

A working Codex project that does something useful in your life. Not a demo. Not a tutorial exercise. Something you actually use, or share, or submit, or put in a portfolio.

An AGENTS.md with lessons-learned entries from this build. The file that started as a set of rules you wrote in advance and became the record of what the build taught you.

A build log noting where the supervisory capacities fired. The PA catches that saved you from a plausible-but-wrong output. The PF revisits that corrected the frame mid-build. The EI checks that caught drift before it propagated.

A post-build learning document. Five sections. One page. Honest. The document that contains the thing you would do differently, named specifically, so that the next build benefits from this one.

A practice. The discipline you have run once is now something you have done, not just read. The next time you sit down with Codex, you will know what conducting it would look like. The build after this one will be faster. The one after that faster still. The overhead is front-loaded once, then amortized across every build that follows.

---

## What You Do Not Have

You do not have a finished discipline.

This is the important thing to say at the end of the book, and Feynman said something like it at the end of every course he taught: knowing the framework is the beginning, not the end. The framework is now yours in the sense that you have used it. It is not yet yours in the sense of reflex — the way an experienced programmer reads code without sounding it out, or a musician reads notation without consciously placing their fingers. That level of internalization takes more builds, more practice reviews, more AGENTS.md revisions across projects you have not yet started.

What you have is the beginning of the practice. The framework is operational. The capacities are named and have fired at least once in a real build. The dangerous middle is something you have seen, not just read about. The post-build document is in your files, not just described in a chapter.

The next build starts from here, and it starts at a different level than the first one did.

---

## Closing

You built it. You know what every line does. You know why every decision was made. You know what you would do differently.

That is what learning looks like. That is what Codex is for.

The next build is yours.

---

## Exercises

**LLM Exercises**

1. **(Create)** Complete your full conducted build, end to end. Submit the post-build learning document alongside the working project. The document is the primary artifact; the project is verification that the discipline produced something.

2. **(Evaluate)** Return to the paragraph you wrote in Chapter 0, exercise 2 — the calculator-and-arithmetic paragraph. Rewrite it now, with the build behind you. What changed?

3. **(Evaluate)** Which of the five supervisory capacities was hardest to exercise consistently in your build? Design a practice exercise targeting that specific capacity for your next build.

---

## What Would Change My Mind

The chapter's claim is that completing the conducted build produces a step-change in supervisory capacity — that the practitioner who has built one full project with the discipline is materially more reliable on subsequent Codex use than the practitioner who has only read about the discipline. If a controlled comparison found no measurable difference between students who completed Chapter 14 and students who completed Chapters 0 through 13 without the capstone, the chapter becomes formative rather than load-bearing. The framework would still be worth building; the case for the capstone as the closing step weakens.

I expect the difference to be substantial because the capstone is where the framework becomes practice. Reading produces the framework; building consolidates it. But the claim is empirical and the evidence is currently practice-based.

---

## Still Puzzling

The right scope for the capstone build. The chapter describes a shape; the specifics are yours. Where the right size is varies by reader, by prior programming experience, by available time. The thirty-minute threshold from Chapter 12 is a working heuristic. For the capstone, the relevant threshold is probably "complex enough that every supervisory capacity fires at least once."

Whether the post-build document should be shared. Sharing produces accountability for honesty. It also produces the incentive to perform. The working answer: candid and private; redacted if shared.

What comes after the chapter. Most readers will do a second build, then a third, and the discipline will become reflex over months. Some will move to the companion books for Claude Code or GitHub Copilot CLI. Some will return to design specific classroom activities. The book ends; the practice continues.

---

## AI Wayback Machine

🕰️ **John Dewey** (1859–1952) — philosopher of education whose *Experience and Education* (1938) argued that learning is the transformation of the learner through purposeful experience, not the deposit of information into the learner.[^1] Dewey's claim was that the cognitive structures that constitute durable learning are built through engaged practice on real tasks — not through reading about practice or being told about practice. The post-build learning document is Dewey applied to AI-assisted coding: the record that the experience changed the person, not just the codebase. The full conducted build is the experience. The document is the record of what the experience built. Dewey was writing about classroom learning; the form scales to the practitioner's first capstone build.

The book ends here.

You are now the practitioner.

The next build is yours.

---

[^1]: Dewey, J. *Experience and Education*. Macmillan, 1938. The Kappa Delta Pi reprint (1998) is the standard recent edition.
