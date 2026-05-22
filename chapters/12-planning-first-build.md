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

<!-- → [DIAGRAM: The planning sequence — Ask Mode interrogation → Problem formulation → Spec → Ask Mode plan → Review and approve → Code Mode execution. Phase gates labeled. Editorial style.] -->

**Step 1: Ask Mode interrogation.** Use Ask Mode to investigate the problem space before committing to a frame. Ask Codex what considerations matter. Note the ones you had not thought of. This is PF in its earliest form — not writing a spec, but discovering what the spec needs to contain.

**Step 2: One-sentence problem formulation.** Three questions, one sentence each: what does this build do? What does it touch? What does it never touch? If you cannot answer each in one sentence, the formulation is not finished. The discipline is not about making the sentences short — it is about making the scope small enough that a sentence fits it.

**Step 3: Minimum viable spec.** Five sections: problem, architecture, user flows, user needs, out of scope. Half a page. Fifteen minutes. The spec is not documentation; it is a contract between you and the build. Everything the build does traces back to this half-page.

**Step 4: Ask Mode plan.** Ask Codex to propose an implementation plan given the spec. Codex returns a sequence of steps with dependencies. This is Codex drafting the conductor's score — which you will then edit.

**Step 5: Plan review.** Read the plan. Find at least one assumption Codex made that you have not verified. Correct it. Repeat until you cannot find any more assumptions. This step is where the conducting discipline is most concentrated, and where the most time is recovered downstream.

**Step 6: First Code Mode prompt.** Now you start the build. The prompt references the plan; the plan references the spec; the spec references AGENTS.md. The chain is complete.

<!-- → [TABLE: Six steps at a glance — three columns: step number / step name / artifact produced. Rows: interrogation → notes; formulation → three sentences; spec → half-page contract; Ask Mode plan → Codex draft; plan review → corrected plan; first Code Mode prompt → build begins. Quick reference for the sequence.] -->

---

## A Worked Planning Session

The best way to understand the sequence is to watch it run. Here is Seth's planning for a personal-finance tool — a script that reads his bank CSV exports and produces a monthly summary.

**Step 1: Ask Mode interrogation (10 minutes).**

Seth asks Codex about effective personal-finance summary design. The interrogation surfaces things he had not considered: categorization approaches (auto vs. manual), recurring-transaction detection, the difference between a cash-flow view and a net-worth view, multi-account handling. Seth had been thinking only about cash flow. The net-worth view is what he actually wants.

Ten minutes in Ask Mode changes the thing he is building. That is what the interrogation step is for.

**Step 2: One-sentence formulation (3 minutes).**

- *What does it do?* A script that reads bank CSV exports and produces a monthly net-worth summary with cash-flow detail.
- *What does it touch?* The CSV files (read), a local SQLite database for transaction history (read/write), a markdown output file (write).
- *What does it never touch?* Bank credentials. The cloud. Either one would make this tool a different kind of tool than Seth wants.

**Step 3: Minimum viable spec (15 minutes).**

```markdown
# Spec — Personal Finance Summary

## Problem
A local Python script that processes manually-downloaded bank CSV exports
into a monthly net-worth summary with cash-flow detail.

## Architecture
- Single Python script + a SQLite database.
- CSVs in ~/finance/inputs/; output to ~/finance/reports/YYYY-MM.md.
- One module per concern: parse, dedupe, categorize, summarize.

## User flows
1. I download new CSVs from each bank.
2. I run the script.
3. It reads new CSVs, deduplicates against existing SQLite, categorizes
   new transactions (auto if rules match; "uncategorized" otherwise).
4. It writes a summary markdown file for the most recent complete month.
5. I review the markdown; manually categorize anything "uncategorized";
   re-run.

## User needs
- Idempotent: running twice produces the same output.
- Dedup is reliable across CSV format differences between banks.
- Categorization rules are in a config file I can edit.
- Output is one markdown file per month, under 200 lines, readable on phone.

## Out of scope
- Automatic CSV downloading.
- Multi-currency.
- Tax calculation.
- Investment portfolio (separate tool).
```

The spec is one page. It took fifteen minutes. Notice what it does: it makes the out-of-scope decisions as explicit as the in-scope decisions. The "never touch bank credentials" from Step 2 becomes a section heading. The net-worth view Seth discovered in Step 1 becomes the problem statement. The spec is the formulation hardened into a contract.

**Step 4: Ask Mode plan (5 minutes).**

Seth asks Codex to propose an implementation plan given the spec. Codex returns seven steps with dependencies:

1. Set up SQLite schema; migration script.
2. CSV parser per bank (format detection on first run, then cached).
3. Deduplication logic against existing transactions.
4. Categorization rule engine.
5. Monthly summary generator.
6. Main script orchestration.
7. Tests for each module.

**Step 5: Plan review (5 minutes).**

Seth reads the plan. Three things are wrong.

First: Step 2 assumes a single CSV format. Seth has two banks with different formats. He corrects: "Step 2 needs to handle two distinct bank CSV formats. Format detection is per-file based on header row matching."

Second: Step 4 implies categorization rules are hardcoded. The spec says they should be in a config file. He corrects.

Third: Step 5 does not specify the markdown output format. The spec says under 200 lines, with sections for net worth, cash flow by category, and an uncategorized list. He adds it.

The plan is now in his words. He approves.

**Step 6: First Code Mode prompt.**

Seth writes a five-element specification for the SQLite schema (per Chapter 9) and starts the build. The build that follows is ninety minutes. Total planning: thirty-eight minutes. Total conducted build: two hours and eight minutes.

Compare that to the alternative. Past-Seth would have opened a new file and started typing after two minutes of thought. Past-Seth would have spent three hours stop-starting, discovered the two-bank CSV problem at hour two, found the hardcoded categorization at hour two and a half, realized at the end that the output format was undefined. Same build. Worse outcome. More time.

The planning did not add forty minutes. It removed ninety.

<!-- → [TABLE: Planning session timeline — six rows, one per step. Columns: step name, time spent, artifact produced. Shows the front-loaded investment and what each step generates.] -->

<!-- → [IMAGE: Annotated plan diff — left side: Codex's original seven-step plan; right side: Seth's corrected version, with three inline annotations marking the single-format assumption, the hardcoded-rules assumption, and the missing output-format spec. Caption: "The three corrections Seth made in Step 5. Each one was worth more than the five minutes the review took."] -->

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

<!-- → [INFOGRAPHIC: Two build timelines side by side — left: unplanned build (early start, multiple rollbacks, late discovery of assumptions, total time 3h). Right: conducted build (front-loaded planning, fewer rollbacks, earlier completion, total time 2h10m). Columns show where time goes in each.] -->

---

## What Planning Is Not

**Planning is not documentation.** The spec is not a requirements document. It is a contract for this build, written in thirty minutes, discarded after the build is done. If it helps document the project later, that is a side effect.

**A longer spec is not a better spec.** The minimum viable spec is the goal. Half a page. One paragraph per section. The test is whether the spec can govern the build, not whether it is comprehensive. A three-page spec for a ninety-minute build is almost always the wrong ratio.

**Planning as you go is not planning.** Plan-in-flight means formulating mid-execution, which mixes thinking work with generation work and degrades both. The front-loaded model exists because these are different kinds of work that do better when separated.

**Planning is not surrendering the discipline to Codex.** The Ask Mode plan is Codex's draft; your review is the discipline. The plan-review step is supervisory work — IJ and EI both fire during it. Codex proposes; you approve or correct. Both steps are required.

**The ratio changes with experience.** For a first conducted build, planning is thirty to forty percent of total build time. By the third build, it is closer to ten to fifteen percent — because the spec template is familiar, the interrogation is faster, and the plan-review eye is sharper. The overhead is front-loaded once, then amortized.

<!-- → [CHART: Planning time as percentage of total build time — line chart across builds 1 through 10. Shows the ratio declining from ~35% at build 1 to ~12% at build 10. Conveys that the overhead is front-loaded once, not permanent.] -->

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
