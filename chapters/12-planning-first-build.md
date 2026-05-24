# Chapter 12 — Planning Your First Conducted Build

*Before Codex sees a single prompt, you know exactly what you are building, why, and which steps belong to you.*

---

The first fully conducted build I did, I sat at my desk for forty minutes without typing a single prompt to Code Mode.

I had AGENTS.md open in one window. I had a scratch file open in another. I had Codex in Ask Mode. And I was thinking — formulating, interrogating, planning. Past-me would have started typing two minutes in. The current me knew that the typing was downstream, and that the typing would go badly if the thinking was not done first.

The thinking took forty minutes. The build took ninety. Two hours and ten minutes total, in place of the three hours of stop-start-rewind work it would have been without the planning — and a better result at the end.

That is the whole chapter. Forty minutes of planning. Ninety minutes of building. Not because planning is virtuous, but because planning is faster. The question is not whether to plan. The question is how.

---

## Six Steps, Front-Loaded

The planning sequence has six steps. Each takes a few minutes. Together they produce the artifacts the build will execute against.

The sequence is front-loaded by design. Everything before Step 6 is thinking work. Step 6 is the first Code Mode prompt. The investment is repaid by every prompt that does not need to be rolled back because the planning caught what would have made it wrong.

![Horizontal flow of five planning phases — Question, Constraints, Plan, Approval, Build — connected by arrows, with Approval highlighted as the gate.](images/12-planning-first-build-fig-01.png)

*Figure 12.1 — The planning sequence. Everything before Build is thinking work; the highlighted gate is plan review.*

**Step 1: Ask Mode interrogation.** Use Ask Mode to investigate the problem space before committing to a frame. Ask Codex what considerations matter. Note the ones you had not thought of. This is PF in its earliest form — not writing a spec, but discovering what the spec needs to contain.

**Step 2: One-sentence problem formulation.** Three questions, one sentence each: what does this build do? What does it touch? What does it never touch? If you cannot answer each in one sentence, the formulation is not finished. The discipline is not about making the sentences short — it is about making the scope small enough that a sentence fits it.

**Step 3: Minimum viable spec.** Five sections: problem, architecture, user flows, user needs, out of scope. Half a page. Fifteen minutes. The spec is not documentation; it is a contract between you and the build. Everything the build does traces back to this half-page.

**Step 4: Ask Mode plan.** Ask Codex to propose an implementation plan given the spec. Codex returns a sequence of steps with dependencies. This is Codex drafting the conductor's score — which you will then edit.

**Step 5: Plan review.** Read the plan. Find at least one assumption Codex made that you have not verified. Correct it. Repeat until you cannot find any more assumptions. This step is where the conducting discipline is most concentrated, and where the most time is recovered downstream.

**Step 6: First Code Mode prompt.** Now you start the build. The prompt references the plan; the plan references the spec; the spec references AGENTS.md. The chain is complete.

| # | Step | Artifact produced |
|---|---|---|
| 1 | Ask Mode interrogation | Notes on the considerations you hadn't thought of |
| 2 | One-sentence problem formulation | Three sentences: what it does, what it touches, what it never touches |
| 3 | Minimum viable spec | Half-page contract — problem, architecture, user flows, user needs, out of scope |
| 4 | Ask Mode plan | Codex's draft sequence of steps with dependencies |
| 5 | Plan review | The same plan, with the unstated assumptions corrected in your words |
| 6 | First Code Mode prompt | The build begins — prompt → plan → spec → AGENTS.md |

---

## A Worked Planning Session

The best way to understand the sequence is to watch it run. Here is Seth's planning for an asset-budget tracker for Haunt & Harvest — a script that reads his Godot export logs and produces a per-build markdown summary of asset sizes, scene counts, and audio file counts so he can spot regressions before he ships a release.

**Step 1: Ask Mode interrogation (10 minutes).**

Seth asks Codex about effective game-asset-budget tracking design. The interrogation surfaces things he had not considered: categorization approaches (auto-by-folder vs. manual tags), recurring large-asset detection across builds, the difference between a per-build view and a delta-versus-last-build view, multi-platform export handling (Windows + Mac + Linux). Seth had been thinking only about a per-build snapshot. The delta-versus-last-build view is what he actually wants.

Ten minutes in Ask Mode changes the thing he is building. That is what the interrogation step is for.

**Step 2: One-sentence formulation (3 minutes).**

- *What does it do?* A script that reads Godot export logs and produces a per-build markdown summary of asset sizes, scene counts, and audio file counts, with deltas against the previous build.
- *What does it touch?* The export-log CSVs Godot writes (read), a local SQLite database for build history (read/write), a markdown output file (write).
- *What does it never touch?* The Godot project files themselves. The cloud. Either one would make this tool a different kind of tool than Seth wants.

**Step 3: Minimum viable spec (15 minutes).**

```markdown
# Spec — Haunt & Harvest Asset Budget Tracker

## Problem
A local Python script that processes Godot export logs from each release
build into a per-build markdown summary, with delta-versus-last-build detail.

## Architecture
- Single Python script + a SQLite database.
- Export logs in ~/HauntAndHarvest/exports/; output to
  ~/HauntAndHarvest/budgets/v{build}.md.
- One module per concern: parse, dedupe, categorize, summarize.

## User flows
1. I export a new build from Godot; the engine writes its export log.
2. I run the script.
3. It reads the new log, deduplicates assets against existing SQLite
   build history, categorizes new assets by folder convention (auto if
   rules match; "uncategorized" otherwise).
4. It writes a summary markdown file for the most recent build.
5. I review the markdown; manually categorize anything "uncategorized";
   re-run.

## User needs
- Idempotent: running twice produces the same output.
- Dedup is reliable across export-log format differences between
  Windows and Mac builds.
- Categorization rules are in a config file I can edit.
- Output is one markdown file per build, under 200 lines, readable on phone.

## Out of scope
- Automatic build triggering.
- Mobile (Android/iOS) exports.
- Per-shader complexity analysis.
- Audio compression suggestions (separate tool).
```

The spec is one page. It took fifteen minutes. Notice what it does: it makes the out-of-scope decisions as explicit as the in-scope decisions. The "never touch the Godot project files" from Step 2 becomes a section heading. The delta-versus-last-build view Seth discovered in Step 1 becomes the problem statement. The spec is the formulation hardened into a contract.

**Step 4: Ask Mode plan (5 minutes).**

Seth asks Codex to propose an implementation plan given the spec. Codex returns seven steps with dependencies:

1. Set up SQLite schema; migration script.
2. Export-log parser per platform (format detection on first run, then cached).
3. Deduplication logic against existing build history.
4. Categorization rule engine.
5. Per-build summary generator with deltas.
6. Main script orchestration.
7. Tests for each module.

**Step 5: Plan review (5 minutes).**

Seth reads the plan. Three things are wrong.

First: Step 2 assumes a single export-log format. Seth's Windows and Mac exports produce logs with different column orders. He corrects: "Step 2 needs to handle two distinct platform log formats. Format detection is per-file based on header row matching."

Second: Step 4 implies categorization rules are hardcoded. The spec says they should be in a config file. He corrects.

Third: Step 5 does not specify the markdown output format. The spec says under 200 lines, with sections for total budget, asset count by folder, audio file counts, and an uncategorized list. He adds it.

The plan is now in his words. He approves.

**Step 6: First Code Mode prompt.**

Seth writes a five-element specification for the SQLite schema (per Chapter 9) and starts the build. The build that follows is ninety minutes. Total planning: thirty-eight minutes. Total conducted build: two hours and eight minutes.

Compare that to the alternative. Past-Seth would have opened a new file and started typing after two minutes of thought. Past-Seth would have spent three hours stop-starting, discovered the two-platform log problem at hour two, found the hardcoded categorization at hour two and a half, realized at the end that the output format was undefined. Same build. Worse outcome. More time.

The planning did not add forty minutes. It removed ninety.

| Step | Time | Artifact produced |
|---|---|---|
| Ask Mode interrogation | 10 min | Notes — surfaced delta-vs-last-build view and the Windows/Mac log split |
| One-sentence formulation | 3 min | Three sentences: what it does, what it reads/writes, what it never touches |
| Minimum viable spec | 15 min | One-page spec with five sections, including explicit Out-of-scope list |
| Ask Mode plan | 5 min | Codex's seven-step plan with dependencies |
| Plan review | 5 min | Three corrections — platform log formats, config-file rules, output format |
| First Code Mode prompt | — | Five-element spec for the SQLite schema; build begins |

![Two side-by-side panels. Left: Codex's original seven-step plan with three red dots marking buried assumptions. Right: Seth's corrected version with three inline italic annotations — scope cut for the single-format assumption, added for the config-file requirement, added for the explicit output format — plus a v2 deferral.](images/12-planning-first-build-fig-02.png)

*Figure 12.2 — Annotated plan diff. The three corrections Seth made in Step 5 — each one worth more than the five minutes the review took.*

---

## Reading the Plan Critically

Step 5 — plan review — is where most students spend too little time. The plan looks reasonable. You want to get to the building. You skim it and approve.

This is the move that produces the dangerous middle at hour two.

The plan-review moment is when assumptions are cheap to catch. Once Codex is in Code Mode, assumptions are baked into the output. Finding them there costs debugging time plus the time to re-specify and regenerate. Finding them in the plan costs nothing but a sentence of correction.

Two practices help.

The first: open the plan in your text editor. In the Codex CLI, `Ctrl+G` opens the plan for editing. Remove steps Codex should not take. Add steps Codex missed. Save and return. The plan is now what you approved, not what Codex proposed. This is EI in its most literal form — you are taking ownership of the arc.

The second: find at least one assumption Codex got wrong. Every plan contains at least one. Seth's plan had three. Sometimes the assumption is small — a file path Codex would have written differently than you would. Sometimes it is larger — a format assumption that would have caused a silent failure at hour two. The review goal is to find the one that matters most. If you cannot find one, look harder. If you genuinely cannot find one after careful reading, approve — but be primed for the build to surface it later.

The discipline of finding at least one assumption before approving is what makes the planning gate non-negotiable for multi-step builds. A plan approved without correction is a plan you have not read.

---

## The Planning Gate

The rule the planning sequence produces:

> No Code Mode prompt runs until the spec exists, the Ask Mode plan has been reviewed, the formulation has passed the one-sentence test, and at least one assumption in the plan has been corrected.

This is the planning gate. It is non-negotiable for builds long enough to need a plan. For one-shot prompts — a small refactor, a one-function addition — the gate is overhead and you skip it. The threshold is rough but functional: any build whose execution would take more than thirty minutes from first prompt to last verification. Below that, the formulation-and-gate from Chapter 7 is enough. Above it, you need the plan.

The gate is not a ceremony. It is a forcing function for the thinking that the build requires anyway. The question is whether you do the thinking before Code Mode runs (cheap) or after the output surprises you (expensive). The gate forces the former.

---

## The Structural Reason Planning Works

It is worth naming the principle beneath the planning sequence, because it explains why the sequence produces better outcomes and not just that it does.

Code Mode generates output calibrated to the most probable interpretation of your prompt. When the prompt references a spec and a plan, the probability space narrows dramatically — because the spec and the plan replace Codex's defaults with your actuals. The spec specifies what done looks like for this project. The plan specifies the sequence in which steps become done. The prompt is now the least ambiguous it can be, because the ambiguity was resolved upstream.

Without the spec and plan, Code Mode is generating against your one-sentence prompt and its own averaged prior. With them, it is generating against a half-page of project-specific constraints that you have verified. The output is more targeted, the errors are fewer, and the errors that remain are the ones the plan could not anticipate — which is the irreducible minimum, and is what Chapter 13 addresses.

Planning is not overhead added to building. Planning is the work that makes building reliable.

![Two stacked horizontal timelines. The top — planned — shows a steady forty-minute thinking block followed by a ninety-minute build block, finishing at 2:10. The bottom — unplanned — shows four forward-work segments interrupted by three rework loops, finishing at 3:00.](images/12-planning-first-build-fig-03.png)

*Figure 12.3 — Planned versus unplanned. Same build, two trajectories. The planned timeline is steady because assumptions were caught upstream; the unplanned one is jagged because they surfaced as rework.*

---

## What Planning Is Not

**Planning is not documentation.** The spec is not a requirements document. It is a contract for this build, written in thirty minutes, discarded after the build is done. If it helps document the project later, that is a side effect.

**A longer spec is not a better spec.** The minimum viable spec is the goal. Half a page. One paragraph per section. The test is whether the spec can govern the build, not whether it is comprehensive. A three-page spec for a ninety-minute build is almost always the wrong ratio.

**Planning as you go is not planning.** Plan-in-flight means formulating mid-execution, which mixes thinking work with generation work and degrades both. The front-loaded model exists because these are different kinds of work that do better when separated.

**Planning is not surrendering the discipline to Codex.** The Ask Mode plan is Codex's draft; your review is the discipline. The plan-review step is supervisory work — IJ and EI both fire during it. Codex proposes; you approve or correct. Both steps are required.

**The ratio changes with experience.** For a first conducted build, planning is thirty to forty percent of total build time. By the third build, it is closer to ten to fifteen percent — because the spec template is familiar, the interrogation is faster, and the plan-review eye is sharper. The overhead is front-loaded once, then amortized.

![A U-shaped line chart. X-axis is minutes spent planning from zero to ninety; y-axis is total session time. The curve dips at around thirty to forty-five minutes — the sweet spot — and rises at both extremes. Zone labels: too little (rework loops at hour two), too much (over-specified, slow start).](images/12-planning-first-build-fig-04.png)

*Figure 12.4 — The planning-time U-curve. Both extremes lengthen the session — too little planning spends the cost downstream as rework; too much spends it upstream as over-specification.*

---

## Christopher Alexander's Insight

In 1964, Christopher Alexander published *Notes on the Synthesis of Form* — a book about architecture that argued the designer's first work is understanding the problem, not proposing solutions.[^1] His observation: solutions developed without first understanding the problem are technically correct and do not serve the people who inhabit them. The building stands. The people inside it are not well-served.

Alexander was writing about buildings. The planning gate is Alexander applied to AI-assisted builds. The formulation, the spec, the plan, the review — all of this is the first work, before the building. Code Mode is not the beginning of the build. It is the end of the planning.

The builds that go wrong in the dangerous middle are almost always the builds where Code Mode started before the problem was understood. The function is correct. The project it lives in is not well-served. Alexander saw this in buildings sixty years ago. The same failure mode runs in AI-generated code today.

---

## Exercises

**LLM Exercises**

1. **(Apply)** Produce an Ask Mode interrogation, one-sentence formulation, minimum viable spec, and Ask Mode plan for the project you will build in the next chapter. Fits on one page.

2. **(Analyze)** Review your plan. Identify the three steps most likely to hit the dangerous middle. For each, write a strong handoff condition that would catch the failure before it propagates.

3. **(Evaluate)** Is your spec ready to govern a build? Name the weakest section. Fix it before you build.

---

## What Would Change My Mind

The chapter's strong operational claim is that the planning gate produces materially better outcomes for multi-step builds than starting directly in Code Mode. If a controlled comparison found no measurable difference in build time, correction rate, or output quality on matched multi-step projects, the gate softens from non-negotiable to recommended. The planning sequence would still be worth teaching; the case for applying it above a thirty-minute threshold weakens.

I expect the difference to be substantial, because planning catches dangerous-middle conditions before they cost real time, and because the post-build documentation is dramatically easier when the plan exists. But the claim is operational and the evidence is currently practice-based, not controlled.

---

## Still Puzzling

The exact threshold above which planning is worth the overhead. Thirty minutes is a working heuristic. Some builds cross the threshold structurally — multi-module, multi-file, consequential outputs — regardless of clock time. Some are short but risky. The threshold probably tracks consequence and architectural complexity more than raw duration.

Whether the spec and the plan should live in separate files. Some practitioners keep them separate (spec as context, plan as execution document); some inline the plan as the spec's implementation section. Either works. The book does not prescribe.

How the plan evolves during the build. The working answer: update the plan at the end of the build to reflect what actually happened. The updated plan becomes the basis for the post-build document.

---

## AI Wayback Machine

🕰️ **Christopher Alexander** (1936–2022) — architect and design theorist whose *Notes on the Synthesis of Form* (1964) argued that good design begins with a clear statement of the problem before any solution is attempted.[^1] Alexander's project, refined through *A Pattern Language* (1977) and *The Timeless Way of Building* (1979), was that the practitioner's first work is understanding the problem the design must solve — and that solutions developed without this understanding produce things that are technically correct and do not serve the people who inhabit them. The planning gate is Alexander applied to AI-assisted builds. The formulation, the spec, the plan, the review — all of it is the first work, before the building. Alexander was writing about buildings. The discipline is the same.

---

## Bridge

The plan is complete. The spec is approved. The next chapter executes the plan.

---

[^1]: Alexander, C. *Notes on the Synthesis of Form*. Harvard University Press, 1964. See also *A Pattern Language* (Oxford University Press, 1977).

---
