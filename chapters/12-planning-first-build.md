# Chapter 11 — Planning Your First Conducted Build

> Before Codex sees a single prompt, you know exactly what you are building, why, and which steps belong to you.

---

## Learning outcomes

1. **(Apply)** Complete a minimum viable spec for a student-scale project using Ask Mode interrogation.
2. **(Apply)** Generate an execution plan using Ask Mode and review it before switching to Code Mode.
3. **(Analyze)** Identify the three steps in the build most likely to hit the dangerous middle.

---

## Opening

This is Seth.

The first fully conducted build I did, I sat at my desk for forty minutes without typing a single prompt to Code Mode.

I had AGENTS.md open in one window. I had a scratch file open in another. I had Codex in Ask Mode. And I was *thinking* — formulating, interrogating, planning. Past-me would have started typing two minutes in. The current me knew that the typing was downstream, and that the typing would go badly if the thinking was not done first.

The thinking took forty minutes. The build, when it started, took ninety. The total was two hours and ten minutes for something that — without the planning — would have taken three hours of stop-start-rewind work and would have produced something I would not have been proud of.

This is the chapter. Forty minutes of planning. Ninety minutes of building. Two hours and ten minutes of conducted work, in place of three hours of unguided work, with a better outcome at the end.

<!-- → [DIAGRAM: The planning sequence — Ask Mode interrogation → Problem formulation → Spec → Ask Mode plan → Review and approve → Code Mode execution. Phase gates labeled. Editorial style.] -->

---

## The planning sequence

Six steps. Each takes a few minutes. Together they produce the artifacts the build will execute against.

**1. Ask Mode interrogation** (Chapter 7). Use Ask Mode to investigate the problem space before committing to a frame. Ask Codex what considerations matter. Note the ones you had not thought of.

**2. One-sentence problem formulation** (Chapter 7). Three questions, one sentence each: what does this build do? what does it touch? what does it never touch? If you cannot answer in one sentence each, the formulation is not finished.

**3. Minimum viable spec** (Chapter 7). The five-section short spec: problem, architecture, user flows, user needs, out of scope. Half a page. 10–15 minutes.

**4. Ask Mode plan**. Ask Codex to propose an implementation plan given the spec. Codex returns a sequence of steps with dependencies.

**5. Plan review**. Read the plan. Find at least one assumption Codex made that you have not verified. Correct it. Repeat until you cannot find any more assumptions.

**6. First Code Mode prompt**. Now you start the build. The prompt references the plan; the plan references the spec; the spec references AGENTS.md.

The sequence is *front-loaded* discipline. Six steps before you write a single Code Mode prompt. The investment is repaid by every Code Mode prompt that does not need to be rolled back because the planning caught what would have made it wrong.

---

## A worked planning session

Seth's planning for a personal-finance tool — a script that reads his bank CSV exports and produces a monthly summary.

**Step 1: Ask Mode interrogation (10 minutes).**

Seth asks Codex about effective personal-finance summary design. Codex surfaces: categorization (auto vs. manual), recurring-transaction detection, the difference between cash-flow view and net-worth view, how to handle multi-account scenarios. Seth notes that he had been thinking only about the cash-flow view; the net-worth view is what he actually wanted.

**Step 2: One-sentence formulation (3 minutes).**

- *What does it do?* A script that reads bank CSV exports and produces a monthly net-worth summary with cash-flow detail.
- *What does it touch?* The CSV files (read), a local SQLite database for transaction history (read/write), a markdown output file (write).
- *What does it never touch?* Bank credentials (none stored; CSVs are downloaded manually). The cloud (entirely local).

**Step 3: Minimum viable spec (15 minutes).**

```markdown
# Spec — Personal Finance Summary

## Problem
A local Python script that processes manually-downloaded bank CSV exports
into a monthly net-worth summary with cash-flow detail.

## Architecture
- Single Python script + a SQLite database.
- CSVs in `~/finance/inputs/`; output to `~/finance/reports/YYYY-MM.md`.
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

**Step 4: Ask Mode plan (5 minutes).**

Seth asks Codex to propose an implementation plan given the spec. Codex returns:

1. Set up SQLite schema; migration script.
2. CSV parser per bank (format detection on first run, then cached).
3. Deduplication logic against existing transactions.
4. Categorization rule engine.
5. Monthly summary generator.
6. Main script orchestration.
7. Tests for each module.

**Step 5: Plan review (5 minutes).**

Seth reads. He notices Codex's plan assumes a single CSV format. His situation is two banks with different formats. He corrects: "Step 2 needs to handle two distinct bank CSV formats. Format detection is per-file based on header row matching."

He also notices Step 4 implies the categorization rules are hardcoded. The spec says they should be in a config file. He corrects.

He notices Codex did not include the markdown output specification. He adds it explicitly: "Step 5 produces markdown matching the User needs section; specifically: under 200 lines, sections for net worth (one number), cash flow (income/expenses by category), and uncategorized list."

The plan is now in his words. He approves.

**Step 6: First Code Mode prompt.**

Seth starts the build. The first prompt is for the SQLite schema, with a five-element specification per Chapter 8. The build proceeds.

Total planning time: 38 minutes. The build that follows is the chapter's worked example for Chapter 12.

---

## The planning gate

The discipline:

> **No Code Mode prompt runs until the spec exists, the Ask Mode plan has been reviewed, the formulation has passed the one-sentence test, and at least one assumption in the plan has been corrected.**

The planning gate is the chapter's central rule. Like the Ask Mode → Code Mode gate from Chapter 4, the planning gate is non-negotiable for builds long enough to need a plan. For one-shot prompts (a small refactor, a one-function addition), the gate is overhead. For multi-step builds, the gate is the protection.

The threshold for "long enough to need a plan": any build whose execution would take more than 30 minutes from first prompt to last verification. Below the threshold, the formulation-and-gate from Chapter 4 is enough. Above the threshold, you need the plan.

---

## Reading the plan critically

The plan-review step (Step 5 above) is where the conducting discipline is most concentrated.

When Codex returns a plan in Ask Mode, the temptation is to skim. The plan looks reasonable. You want to get to the building. You approve.

This is the move that produces the dangerous middle later. The plan-review moment is when assumptions are cheap to catch. Once Codex is in Code Mode, the assumptions are baked into the output, and undoing them is expensive.

Two practical patterns help.

**Pattern 1: edit the plan in your text editor.** In the Codex CLI, `Ctrl+G` opens the plan in your text editor. Edit it. Remove steps Codex should not take. Add steps Codex missed. Save and return. The plan is now what *you* approved, not what Codex proposed.

**Pattern 2: find at least one assumption Codex got wrong.** Every plan contains at least one. Sometimes it is small (Codex assumes a file path you would have written differently). Sometimes it is larger (Codex assumes a single CSV format when your situation has two, as in the worked example). Finding the one assumption is the review goal. If you cannot find one, look harder. If you genuinely cannot find one, approve — but be primed for the build to surface the assumption later.

---

## Common misconceptions

**"Planning is for big builds."** Builds that take more than 30 minutes benefit. Most non-trivial student builds cross the threshold.

**"My plan should be exhaustive."** No. The minimum viable plan is the goal. Half a page; one paragraph per step; one handoff condition per step. Detail in proportion to consequence.

**"I'll plan as I go."** The cost of planning-in-flight is consistently higher than planning-upstream. Plan-in-flight means you are formulating mid-execution, which mixes work types and produces worse outcomes.

**"Ask Mode planning surrenders the discipline."** No. The Ask Mode plan is *Codex's draft*; your review is the discipline. Both are required.

**"40 minutes of planning is too much for a 90-minute build."** The ratio (planning ≈ 30–40% of build time for first conducted builds) drops sharply with experience. By the third conducted build, planning is 10–15% of build time and the build is faster overall.

---

## Exercises

1. **(Apply)** Produce an Ask Mode interrogation, one-sentence formulation, minimum viable spec, and Ask Mode plan for the project you will build in Chapter 12. Fits on one page.

2. **(Analyze)** Review your plan. Identify the three steps most likely to hit the dangerous middle. For each, write a strong handoff condition.

3. **(Evaluate)** Is your spec ready to govern a build? What is the weakest section? Fix it before Chapter 12.

---

## What would change my mind

The chapter's strong operational claim is that **the planning gate produces materially better build outcomes** for multi-step projects than starting from Code Mode. If a controlled comparison found no measurable difference, the gate becomes optional. The chapter would still teach the planning sequence as a supervisory practice; the case for "every multi-step build" weakens.

I expect the difference to be substantial because planning catches dangerous-middle conditions before they cost real time, and because the post-build documentation (Chapter 14) is dramatically easier when the plan exists.

---

## Still puzzling

- **The exact threshold above which planning is worth the overhead.** "30-minute build" is a working heuristic, not a measured threshold.

- **Whether the spec and the plan should be separate files.** Some practitioners keep them separate; some inline the plan as the spec's "implementation" section. Either works.

- **How the plan evolves during the build.** The book's working answer: update the plan at the end of the build to reflect what actually happened. Use the updated plan as the basis for the post-build document.

---

## AI Wayback Machine

🕰️ **Christopher Alexander** (1936–2022) — architect and design theorist whose *Notes on the Synthesis of Form* (1964) argued that **good design begins with a clear statement of the problem** before any solution is attempted.[^1] Alexander's project, refined in *A Pattern Language* (1977) and *The Timeless Way of Building* (1979), was that the practitioner's first work is *understanding the problem the design must solve* — and that solutions developed without first understanding produce things that are technically correct and that do not serve the people who inhabit them. The planning gate is Alexander applied to AI-assisted builds. The formulation, the spec, the plan, the review — all of these are the first work, before the building. Alexander was writing about buildings; the discipline is the same.

---

## Bridge

The plan is complete. The spec is approved. Chapter 12 executes the plan.

---

[^1]: Alexander, C. *Notes on the Synthesis of Form*. Harvard University Press, 1964. See also *A Pattern Language* (Oxford, 1977).
