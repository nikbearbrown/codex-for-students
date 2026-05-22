# Chapter 13 — Running the Build: Codex Tasks and Human Tasks
*Quality is built in at every step, not inspected in at the end — and the step is one prompt wide.*

> The plan is approved. Now you execute it — one step at a time, with the suggest → explain → verify gate applied at every step.

---

Seth's Haunt & Harvest asset-budget build, Phase 1. The plan from Chapter 12 is open on one side. Codex is open on the other. The first step is the SQLite schema.

He pastes the relevant AGENTS.md sections plus the spec's Architecture section into a Code Mode prompt. He references the plan: *"Step 1 from the plan: SQLite schema for the build_assets table. Per the spec, the database lives at `~/HauntAndHarvest/budget.db`."* He writes the full five-element specification for this step. The CLI returns a schema migration script. Seth reads it. The script creates a `build_assets` table with the columns the spec implied. He runs `gh copilot explain` mentally against the script — *what does each column do, what are the constraints, what is the primary key?* The explanation matches his prediction.

He runs the migration. The database is created.

Then he pauses.

The schema uses an `INTEGER` primary key auto-incremented. The spec said idempotency was a user need — running the script twice should produce the same output. With auto-incremented integer keys, importing the same export log twice would create duplicate asset rows with different keys. The dedup logic at Step 3 would have to detect duplicates by content, not by key. That is fine in principle; the spec did not specify a different key strategy. But the dedup logic would be simpler with a content-hash key (over build version + asset path + size), and the SQL inserts would become idempotent automatically with an `INSERT OR IGNORE`.

Seth reverts the migration. He updates the plan to reflect the schema change. He re-prompts with the revised specification. The new schema is created.

The first step took 15 minutes instead of 5. It caught a design choice that would have made Step 3 harder. The total build will be faster for the catch. That is the discipline in action: the plan was good, the spec was clean, the handoff condition passed, and the plausibility audit fired anyway — on the design choice behind the correct output, not on the output itself.

This is the chapter. The per-step loop. The pivotal moments where the framework catches what would have broken later.

---

## The build loop

Each step in the plan goes through the same sequence.

Reference the plan: *"Per Step N: do X with Y conditions."* Paste the relevant AGENTS.md sections into the prompt context. Compose the five-element specification from Chapter 9. Submit the Code Mode prompt. Read the output — and predict what it will look like before reading, so that the prediction creates a target to check against. Verify against the handoff condition. Run the plausibility audit on the result, even if the handoff passed. Update AGENTS.md if a lesson surfaced.

That is the loop. One or two minutes per step on smooth runs. When the discipline catches something, the loop pauses and the catch is handled. The catch is where the build's quality is built.

![A five-node cycle — Read plan, Pick step, Execute, Verify, Next — arranged around a central hub labeled ONE PROMPT WIDE. The Verify node is highlighted in red. A dashed red branch leaves Execute on failure to a Stop — Revert node.](images/13-running-the-build-fig-01.png)

*Figure 13.1 — The build loop. Verify is highlighted because quality is built in at the check, not inspected in at the end. The stop branch fires after two failed corrections on the same step.*

The loop is not bureaucratic overhead. It is the shape of the supervisory capacities in practice. The five-element specification is Problem Formulation. Reading the output against a prediction is Plausibility Auditing. Verifying against the handoff condition is Plausibility Auditing made explicit. Updating AGENTS.md is Executive Integration applied to the project's persistent context. The loop is the operational form of everything Chapter 6 named.

---

## The three pivotal moments

Every multi-step build produces at least three moments where the discipline matters. The discipline is recognizing them as they arrive.

**Handoff failure: revert, do not correct forward.**

A step's output fails its handoff condition. The temptation is to write a follow-up prompt to fix the result. The discipline is to revert and respecify.

The schema example above is a soft version — the handoff condition technically passed but the plausibility audit caught a design issue. The hard version is when the condition actually fails. In Seth's build, this happened at Step 4, the asset categorization rule engine. The first Code Mode prompt produced an engine that hardcoded the folder-to-category mapping — directly contradicting the spec, which required rules loaded from a config file. Seth used `/rewind` to restore to before the failed step. He revised the prompt to make the config-file requirement explicit as a negative constraint. The next prompt produced what the spec required.

After two failed corrections on the same step, the session context is too polluted to recover cleanly. The accumulated failures are now noise that Codex is reasoning against. Use `/clear` or open a new session. Fresh start with a tighter specification. The cost feels high in the moment; it is consistently lower than the cost of continuing.

**Scope creep: log it, decline now.**

Codex generates an output that does the requested step and offers to do something adjacent. The adjacent thing might be useful. It is not the current step.

In Seth's build, this happened at Step 5, the per-build summary generator. Codex offered to also generate a release-over-release trend chart across the last ten builds. Useful future feature. Not in the spec. Seth logged it in the spec's "Out of scope (later)" section and declined. The current step's focus was preserved. The suggestion survives in writing.

The discipline is not to dismiss the suggestion — it may be genuinely useful — but to hold the boundary. The adjacent thing goes in the log. The current step gets finished first.

**Plausibility audit: trust the feeling.**

The current step's output passes the handoff condition. Something feels off.

The schema choice is this moment. The migration worked. The handoff condition passed. The plausibility audit fired on the design choice itself — the auto-incremented key was technically correct but would make later steps harder. The feeling was: *this is going to cause a problem I haven't specified yet.*

The discipline is to investigate that feeling. Do not dismiss it because the handoff passed. The handoff condition you wrote was the condition you thought to write. The feeling is the condition you did not think to write. That is the dangerous middle from Chapter 10, arriving again, in the shape of a correct output that does not fully serve the situation.

![Three labeled boxes side by side. Plan deviation: do this — /rewind, respecify with the failure as a negative constraint. Test failure (highlighted in red): do this — investigate the feeling; the condition you wrote was the condition you thought to write. Scope drift: do this — log it, decline now.](images/13-running-the-build-fig-02.png)

*Figure 13.2 — Three pivotal moments. Distinct triggers, distinct responses. The middle moment is the hardest — the discipline is to investigate a feeling rather than dismiss it because the tests passed.*

---

## What human tasks look like in the build

Not every step in the build is a Codex prompt. Some are human tasks — work that should not be delegated.

For Seth's asset-budget build, human tasks included the schema choice in the opening (a design judgment). Manually categorizing the first few "uncategorized" assets to seed the rule engine — the categorization is Seth's judgment, not a pattern Codex can supply without being told what `scenes/ai/` means versus `scenes/inventory/`. Reviewing the summary markdown for the first build: is it the summary Seth wanted, or has it drifted to a more generic shape? Deciding what to do with the edge case the dedup logic surfaced — two assets with the same path and identical byte size across builds that turned out to be two genuinely different audio cues, not a duplicate, and therefore not something the dedup rule should collapse.

The plan labels which steps are Codex tasks and which are human. The build log labels which supervisory capacity — PA, PF, TO, IJ, EI — was exercised at each human step. Together, the labels make the supervisory work visible: at the end of the build, Seth can look at his build log and see exactly which steps required his judgment and which capacity each one exercised.

That visibility is not for the teacher. It is for Seth. The build log is the record of the conducting discipline applied to a real project. It is the artifact that converts the experience of building into the capacity to teach building — the Feynman test, applied to the whole build.

| # | Task | Codex / Human | Capacity | Handoff condition | Result |
|---|---|---|---|---|---|
| 1 | Generate SQLite schema migration for `build_assets` table | Codex | — | "Migration script exists, runs cleanly against an empty DB, creates the columns the spec implies" | Pass mechanically, **revise** on PA — auto-increment key replaced with content-hash to make Step 3 idempotent |
| 2 | Decide the key strategy and rewrite the schema spec | Human | **IJ + PA** | "Schema uses `(build_version, asset_path, size)` hash as primary key; `INSERT OR IGNORE` is idempotent" | Pass |
| 3 | Implement dedup logic against existing build history | Codex | — | "Running the importer twice on the same log produces zero new rows on the second run; `pytest tests/test_dedup.py` green" | Pass |
| 4 | Seed initial folder→category rules by hand for the first three categories | Human | **IJ** | "`config/categories.yaml` contains `scenes/ai/`, `scenes/inventory/`, `audio/sfx/` mapped to their categories; file is valid YAML" | Pass |
| 5 | Implement per-build markdown summary generator with deltas | Codex | — | "Generator writes `~/HauntAndHarvest/budgets/v{build}.md` under 200 lines; sections for total budget, by-folder counts, audio counts, uncategorized list" | **Revert** on scope creep — Codex offered an extra release-over-release trend chart; logged to Out-of-scope, re-prompted without it, then pass |

---

## Give Codex a way to verify its own work

There is a move from the practitioner literature that reduces the number of obvious failures that surface in your review: include a verification command in the specification that Codex can run after execution.

If your specification includes a test command or a verification script, Codex will iterate against it. From the OpenAI engineering retrospective: *"When Codex has some way to check its work — like run tests or screenshot the UI — it can iterate and get dramatically better results."*[^1]

For Seth's build, the dedup-logic specification included: *"After implementation, run `python -m pytest tests/test_dedup.py` and fix any failures before reporting done."* Codex ran the test suite, iterated twice against failures, and reported done with all tests passing. Seth then read the final code and ran the plausibility audit. The in-prompt verification had already eliminated the mechanical failures; Seth's review was on the design and domain correctness — specifically, whether the dedup rule respected the case where two same-sized audio files at the same path were in fact different cues.

This is the division of labor the loop assumes. Codex is responsible for mechanical correctness — the tests you gave it pass. You are responsible for design correctness and domain fitness — the tests you gave it were the right tests, the code does what the situation actually requires, the behavior in the non-tested cases is sane. Mechanical correctness is verifiable by machine. Design correctness and domain fitness require the supervisory capacities.

---

## The Deming connection

There is a principle behind the per-step loop that predates Codex by seventy years and is worth naming because it clarifies what the loop is doing.

W. Edwards Deming — statistician, engineer, architect of Japan's post-war industrial quality revolution — argued throughout his career that quality is built into a process at every step, not inspected in at the end. His Plan-Do-Check-Act cycle makes this operational: plan the change, do it at small scale, check the result against the plan, act on what the check revealed. The cycle repeats. Quality emerges from the iteration, not from a final inspection.

The distinction Deming pressed hardest: *inspection at the end finds defects that have already been made.* The cost of the defect is already embedded in the product. Building quality in at every step prevents the defect from being made, which is cheaper by an order of magnitude.

The per-step build loop is PDCA at prompt granularity. The specification is the Plan. The Code Mode prompt and execution is the Do. The handoff condition and plausibility audit are the Check. The decision to proceed or revert is the Act. The loop repeats at every step of the build. The dangerous-middle failure is exactly the defect that builds up when Check is weak — the auto-incremented key, the hardcoded rule engine, the edge case the test suite did not exercise.

Deming wrote about manufacturing. The principle scales. The build step is the unit of work; the loop is the unit of quality. Quality is built in, not inspected in. That is what the discipline is.

---

## After two failed corrections

The build's hardest moment is recognizing when to stop and start fresh.

The signal is the second failed correction on the same step. The session context now contains two failed attempts, the corrections, and Codex's responses to those corrections. Codex is reasoning against all of this. The next attempt is less likely to work than the first would have been with a better specification.

Stop. Do not write a third correction. Use `/clear` or `/rewind`. The context is clean.

Then ask: why did the first two attempts fail? The answer is almost always upstream — under-specification, wrong frame, a negative constraint that should have excluded the failure mode but was absent from the spec. The fix is to rewrite the specification with the failure as an explicit constraint. The first attempt with the revised spec usually works.

The two-corrections rule is the operational form of the principle. It sounds arbitrary until you have violated it twice and seen what happens to the third attempt. Then it is not arbitrary; it is the point past which the cost of continuing exceeds the cost of starting over.

---

## What would change my mind

The chapter's central claim is that the per-step loop materially reduces silent failures. If a controlled comparison — same plan, same build, disciplined loop vs. no loop — found no measurable difference in build quality or in the number of downstream failures traced to decisions made without audit, the loop's overhead becomes unjustified. The book would still teach the supervisory capacities; the explicit loop form softens.

I expect the difference to be substantial. The loop's catches address the specific failure modes the data from Chapters 2 and 3 shows produce skill atrophy and silent errors. The loop is the mechanism that makes the supervisory capacities operational rather than theoretical.

---

## Still puzzling

The exact step count above which the full loop is worth the overhead. Builds with two or three steps can proceed with lighter discipline. The book's working answer is five or more steps — but that threshold is a practitioner estimate, not a measured result.

Whether the build log should be public or private. Public logs serve other students who can learn from the pattern of catches. Private logs allow more candid plausibility-audit notes. Probably both: candid private log during the build, redacted public version afterward.

How the loop changes when Codex operates with more agentic autonomy. The 2026 Codex app supports modes where Codex chains steps with less human intervention between them. The book argues against using those modes for student work — the human step in the loop is the discipline. A future edition may address those modes directly.

---

## AI Wayback Machine

🕰️ **W. Edwards Deming** (1900–1993) — statistician whose **Plan-Do-Check-Act** cycle became the foundation of modern quality management. Deming argued that quality is built into a process through verification at *every step*, not inspected in at the end. The PDCA cycle iterates: plan the change, do it at small scale, check the result against the plan, act on what the check revealed.[^2] The per-step loop in this chapter is PDCA at prompt granularity. Plan: the specification. Do: the Code Mode prompt and execution. Check: the handoff condition and the plausibility audit. Act: revert and respecify, or proceed. Deming's insight — that quality is built in, not inspected in — is the chapter's insight applied to AI-assisted coding. The dangerous-middle failure is the defect that inspection at the end finds too late. The loop is the mechanism that prevents the defect from being built in the first place.

---

## Bridge

The build is done when it passes the handoff conditions. Chapter 14 defines what "done" actually means at the level of the whole build — and asks you to conduct it yourself, end to end.

---

## Exercises

**Warm-up**

1. *(Targets: the build loop)* Write out the eight steps of the build loop from memory, then check them against the chapter. Which steps did you omit or reorder? The omissions are the ones most likely to get skipped in a real session under time pressure.

2. *(Targets: Codex vs. human tasks)* Take the last three things you asked Codex to do in a coding session. Classify each as: (a) a Codex task that the loop covers, (b) a human task that should not have been delegated, or (c) a judgment call that could go either way. For any (b), name which supervisory capacity the delegation bypassed.

**Application**

3. *(Targets: three pivotal moments)* In your next Codex session, watch for the three pivotal moments — handoff failure, scope creep, plausibility-audit fire. When one occurs, document it: what triggered it, what the temptation was, what the discipline says to do, and what you actually did. One paragraph per moment encountered.

4. *(Targets: in-prompt verification)* Add a verification command to the specification for your next Codex task — a test to run, a file to check, a count to verify. After the session: did Codex use it? Did it catch anything that your review would have caught anyway, or something your review might have missed?

**Synthesis**

5. *(Targets: build loop + supervisory capacities)* Run a five-step build sequence using the full loop. For each step, label the supervisory capacity that fired at the check stage. At the end: which capacity fired most often? Which capacity did you have to exercise hardest? What does that tell you about where your domain depth is thinnest?

**Challenge**

6. *(Targets: two-corrections rule + Deming's principle)* The chapter claims the two-corrections rule is the operational form of Deming's "build quality in, don't inspect it in." Write a paragraph explaining the connection in your own words — not using Seth's examples, using your own. Then: identify one step in your current project where you have already violated the rule (corrected forward more than twice). What would a proper rewind-and-respecify look like for that step now?

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Deming, W. E. *Out of the Crisis*. MIT Press, 1986; reissued 2018. PDCA is articulated across his work; the 1986 book is the standard reference.

---

## Prompts

Use these prompts with Claude to generate interactive D3 v7 versions of the figures in this chapter. Each produces a standalone HTML file you can open in a browser and modify freely.

**Prerequisites:** Load `brutalist/CLAUDE.md` and `brutalist/DESIGN.md` into your Claude project context before using these prompts. They define the stack, naming conventions, color system, and typography the figures use.

---

### Figure 13.1 — The build loop

Build a five-node clockwise cycle in D3 v7 inside a `--color-fill` plot rectangle. Five rectangular cards with a `--color-fill` header strip and a monospace ALL CAPS code (`READ PLAN`, `PICK STEP`, `EXECUTE`, `VERIFY`, `NEXT`), arranged in a clock-face pattern around a central hub. Connect the five nodes with curved single-headed arrows that share one `<defs>` arrowhead marker. The `VERIFY` node is highlighted in `--color-red` — header text, code label, and card stroke all switch to red. From `EXECUTE`, draw a dashed red branch to a sixth card (`STOP — REVERT`, `/rewind → respecify`) using a second red-filled arrowhead marker; label the branch `fail` in red italic. At the centre of the cycle, place a monospace ALL CAPS label `ONE PROMPT / WIDE` over an italic secondary line `PDCA at prompt granularity`. Hovering any card shows a tooltip with the longer step description. Dashed footer rule plus a caption naming why Verify is highlighted.

> Reference implementation: `d3/13-running-the-build-fig-01.html`

---

### Figure 13.2 — Three pivotal moments

Build a three-column box layout in D3 v7. Each column is a tall rectangular card with a `--color-fill` header strip containing a monospace ALL CAPS code (`PLAN DEVIATION`, `TEST FAILURE`, `SCOPE DRIFT`). Inside each box, three labeled sub-sections: a bold `Trigger` heading with two `--color-secondary` body lines; a bold `Temptation` heading with two italic `--color-secondary` lines; a bold red `Do this` heading with a monospace red action line (`/rewind`, `investigate the feeling`, `log it, decline now`) followed by three `--color-ink` instruction lines. The middle card (`TEST FAILURE`) is the highlighted one — header text and card stroke switch to `--color-red`. Hovering any card shows a tooltip explaining the trigger and the discipline. Dashed footer rule plus a two-line caption naming the middle moment as the hardest — the discipline is investigating a feeling rather than dismissing it because the tests passed.

> Reference implementation: `d3/13-running-the-build-fig-02.html`
