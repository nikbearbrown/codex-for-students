# Chapter 12 — Running the Build: Codex Tasks and Human Tasks

> The plan is approved. Now you execute it — one step at a time, with the suggest → explain → verify gate applied at every step.

---

## Learning outcomes

1. **(Apply)** Execute a conducted build sequence in Code Mode, completing Codex tasks and human tasks in dependency order.
2. **(Apply)** Apply each of the five supervisory capacities at the steps requiring them.
3. **(Analyze)** Identify when a build is going off-script and stop before it breaks.

---

## Opening

Seth's personal-finance build, Phase 1. The plan from Chapter 11 is open on one side. Codex is open on the other. The first step is the SQLite schema.

He pastes the relevant AGENTS.md sections plus the spec's Architecture section into a Code Mode prompt. He references the plan: *"Step 1 from the plan: SQLite schema for the transactions table. Per the spec, the database lives at `~/finance/finance.db`."* He writes the full five-element specification for this step.

The CLI returns a schema migration script. Seth reads it. The script creates a `transactions` table with the columns the spec implied. Seth runs `gh copilot explain` mentally against the script — *what does each column do, what are the constraints, what is the primary key?* The CLI's explanation matches Seth's prediction.

He runs the migration. The database is created.

Then he pauses.

Plausibility audit fires. The schema uses an `INTEGER` primary key auto-incremented. But the spec said idempotency was a user need — running the script twice should produce the same output. With auto-incremented integer keys, importing the same CSV twice would create duplicate transactions with different keys. The dedup logic (Step 3) would have to detect duplicates by content, not by key. That is fine in principle; the spec did not specify a different key strategy.

Seth pauses on the choice. Auto-incremented integer key (current) vs. a content-hash key (would make dedup trivial but adds complexity to the schema). He decides: content-hash key, because the dedup logic is cleaner and the SQL inserts become idempotent automatically with an `INSERT OR IGNORE`.

He reverts the migration. He updates the plan to reflect the schema change. He re-prompts with the revised specification. The new schema is created.

The first step took 15 minutes instead of the 5 he had planned, because the plausibility audit caught a design choice that would have made Step 3 harder. The total build will be faster for the catch.

This is the chapter. The per-step discipline. The pivotal moments where the framework catches what would have broken later.

---

## The build loop

Each step in the plan goes through the same loop.

1. **Reference the plan.** *"Per Step N: do X with Y conditions."*
2. **Paste relevant AGENTS.md sections** into the prompt context.
3. **Compose the specification** (the five-element format from Chapter 8).
4. **Submit the Code Mode prompt.**
5. **Read the output.** Predict what it will look like before reading.
6. **Verify against handoff condition** (Chapter 9).
7. **Plausibility audit on the result.** Even if the handoff passed, does the result make sense?
8. **Update AGENTS.md** if a lesson surfaced.

The loop is one or two minutes per step on smooth runs. When the discipline catches something — like the schema choice in the opening — the loop pauses and the catch is handled. The catch is the build's value.

<!-- → [DIAGRAM: The build loop. Specification → Codex executes in Code Mode → Handoff condition check → Pass: next step / Fail: revert and respecify. Supervisory capacity label at the check step.] -->

---

## The three pivotal moments

Every multi-step build produces at least three moments where the discipline matters. Recognize them as they arrive.

### Moment 1: handoff failure → revert, do not correct forward

A step's output fails its handoff condition. The temptation is to compose a follow-up prompt to fix the result. The discipline (Chapter 9) is to revert and respecify.

The schema example above is a soft version — the handoff condition technically passed (the migration ran) but plausibility audit caught a design issue. The hard version is when the handoff condition actually fails. In Seth's build, this happened at Step 4 (the categorization rule engine). The first Code Mode prompt produced an engine that hardcoded the rules — directly contradicting the spec. Seth used Codex's `/rewind` to restore to before the failed step. He revised the prompt to make the config-file requirement explicit as a negative constraint. The next prompt produced what the spec required.

After two failed corrections on the same step, use `/clear` or open a new session. The accumulated context from failed attempts is noise. Fresh start with a tighter spec.

### Moment 2: scope creep → log to spec or task queue, decline now

Codex generates an output that does the requested step *and* offers to do something adjacent. The adjacent thing might be useful. It is not the current step.

In Seth's build, this happened at Step 5 (the summary generator). Codex offered to *also* generate a year-over-year comparison view. Useful future feature. Not in the spec. Seth logged it in the spec's "Out of scope (later)" section and declined now. The current step's focus was preserved. The suggestion survives in writing.

### Moment 3: plausibility audit → trust the feeling

The current step's output passes the handoff condition. Something feels off.

The schema choice in the chapter opening is this moment. The migration worked. The handoff condition (schema exists, table has the right columns) passed. The plausibility audit fired on the *design choice itself* — the auto-incremented key was technically correct but would make later steps harder.

The discipline is to investigate the feeling. Do not dismiss it because the handoff passed. The condition you wrote was the condition you thought to write; the feeling is the condition you did not think to write.

---

## What human tasks look like in the build

Not every step in the build is a Codex prompt. Some are *human tasks* — work that should not be delegated.

For Seth's finance build, human tasks included:

- The schema choice from the opening (a design judgment).
- Manually categorizing the first few "uncategorized" transactions to seed the rule engine (the categorization is yours, not Codex's).
- Reviewing the summary markdown for the first month — is it the *summary* you wanted, or has it drifted to a more generic shape?
- Deciding what to do with edge cases the dedup logic surfaced (an exact-amount-same-day duplicate that turned out to be two real transactions, not a duplicate).

The plan labels which steps are Codex tasks and which are human. The build log labels which capacity (PA, PF, TO, IJ, EI) was exercised at each human step. Together, the labels make the supervisory work visible.

---

## After two failed corrections

The discipline's hardest moment is recognizing when to give up on a step and start it fresh.

The signal: you have tried two corrections on the same step. Codex has had two chances to produce a working result. The result is still wrong, and your session context now contains both failed attempts plus your corrections plus Codex's responses.

The next attempt has a polluted context. Codex is reasoning against the failures as if they are part of the desired result. The third attempt is less likely to work than the first.

The discipline:

1. **Stop.** Do not write a third correction.
2. **`/clear` or `/rewind`.** The context is now clean.
3. **Revisit the specification.** Why did the first two attempts fail? Almost always: under-specification or wrong frame. The fix is upstream, not in the next prompt.
4. **Rewrite the specification** with the failed conditions as explicit negative constraints.
5. **First attempt with the new spec.** Usually works.

The cost of starting fresh feels high. It is consistently lower than the cost of continuing with polluted context.

---

## Give Codex a way to verify its own work

A move from the practitioner literature, repeated here because it is the operational form of the in-prompt verification mentioned in Chapter 8.

If your specification includes a test command or a verification script that Codex can run, Codex will iterate against it. From the OpenAI engineering retrospective: *"When Codex has some way to check its work — like run tests or screenshot the UI — it can iterate and get dramatically better results."*[^1]

For Seth's finance build, the dedup logic specification included: *"After implementation, run `python -m pytest tests/test_dedup.py` and fix any failures before reporting done."* Codex iterated against the test suite. The first attempt failed two tests; Codex revised; the second attempt passed. Seth read the final code; verified it manually; accepted.

The in-prompt verification is the per-step extension of the handoff condition. It does not replace your verification (you still read the code and run the plausibility audit), but it dramatically reduces the number of cases where the obvious failures surface in your review.

---

## Common misconceptions

**"The plan should run smoothly once it's written."** A good plan reduces but does not eliminate failures. The dangerous middle is the failure mode plans cannot fully eliminate. The build's value is partly in the plan's correctness and partly in the discipline of catching the cases the plan did not anticipate.

**"If I update AGENTS.md mid-build I'll lose my place."** Update AGENTS.md in a scratch file during the build; commit the updates at the end. The cost of mid-build edits is real; the cost of *not* updating is larger because the lessons are lost.

**"Plausibility audit is just being paranoid."** PA is the named capacity that catches dangerous-middle failures. It is not paranoia; it is the exercised supervisory capacity. The chapter is the discipline of *attending* to PA when it fires.

**"My build went smoothly; the discipline must be overkill."** Survivor bias. Builds that go smoothly are builds where nothing was hiding. Builds where something *was* hiding — and the discipline caught it — feel less smooth in retrospect but produce better outcomes.

**"I can document the build later."** You will not. The build log written during the build is real; the build log reconstructed after is partial. Write during.

---

## Exercises

1. **(Apply)** Execute Phase 1 of your conducted build (from Chapter 11). Document each handoff evaluation and each capacity exercised.

2. **(Analyze)** At least one Codex output will require revision. Document the failure: what was the handoff condition; what passed it; what was wrong despite the pass.

3. **(Evaluate)** At the end of Phase 1: what did you learn? What would you change in the spec or AGENTS.md?

---

## What would change my mind

The chapter's strong claim is that **the per-step loop (paste, prompt, read, verify, plausibility audit, log) materially reduces silent failures**. If a controlled comparison found no measurable difference between disciplined and undisciplined execution of the same plan, the loop becomes optional. The book would still teach it for the supervisory-practice benefit; the urgency softens.

I expect the difference to be substantial because the loop's catches address the specific failure modes the data shows produce skill atrophy.

---

## Still puzzling

- **The exact number of steps above which the loop is worth the overhead.** Builds with two or three steps can proceed with lighter discipline. The book's working answer: builds with five or more steps benefit from the full loop.

- **Whether the build log should be public or private.** Public build logs serve other students. Private build logs allow more candid plausibility-audit notes. Probably both — candid private, redacted public.

- **How the build loop changes when Codex has more agentic autonomy.** The 2026 Codex app supports modes where Codex chains steps with less human intervention. The book argues against using those for student work — the discipline is the point.

---

## AI Wayback Machine

🕰️ **W. Edwards Deming** (1900–1993) — statistician whose **Plan-Do-Check-Act** cycle became the foundation of modern quality management. Deming argued that quality is built into a process through verification at *every step*, not inspected in at the end. The PDCA cycle iterates: plan the change, do the change at small scale, check the result against the plan, act on what the check revealed.[^2]

The per-step loop in this chapter is PDCA at prompt granularity. Plan: the specification. Do: the Code Mode prompt and execution. Check: the handoff condition and the plausibility audit. Act: revert and respecify (or proceed) based on what the check revealed. Deming wrote about manufacturing in the 1950s; the cycle scales. The Code Mode prompt is the unit of work; the loop is the unit of quality. Deming's insight — that quality is built in, not inspected in — is the chapter's insight applied to AI-assisted coding.

---

## Bridge

The build is done when it passes the handoff conditions. Chapter 13 defines what "done" actually means at the level of the whole build.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Deming, W. E. *Out of the Crisis*. MIT Press, 1986; reissued 2018. PDCA is articulated across his work; the 1986 book is the standard reference.
