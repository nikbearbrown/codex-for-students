# Chapter 14 — Verification: How You Know It Works

*The build is done when it passes the handoff conditions — not when Codex says it's done.*

---

The per-step handoffs had all passed.

Seth's Haunt & Harvest asset-budget script had finished its last step. It processed his v0.4 build export log. The summary markdown rendered: total budget, asset count by folder, audio file counts, four uncategorized assets flagged for manual review. Codex's status report said the build was complete. Every handoff condition had returned a pass.

He was about to declare it done. Then he ran the verification pass — the one he almost skipped.

He opened the markdown file on his phone. It rendered. The asset-count-by-folder section used a Markdown table with seven columns — folder, file count, total size MB, delta count, delta size, largest file, largest size. On a phone screen in portrait mode, the table did not fit. The delta columns at the right edge were cut off. You could scroll horizontally to see them. He wouldn't, on the couch, between scenes.

The build had passed every mechanical check. The User need from the spec — *output is readable on phone* — was technically met in the sense that the markdown file could be opened on a phone. It was not met in the sense that mattered. The table format was wrong for the device.

Seth fixed it. The table became a list. Folder name as a heading, the seven fields as a bullet list beneath each. The output fit. The build was now genuinely done.

This is the distinction the chapter is about. The per-step handoffs are necessary. They are not sufficient. There is a verification pass at the level of the *whole build* — against the spec's User needs, not against the individual steps' conditions — and it catches what nothing else catches.

---

## What the handoffs cannot see

Before explaining the three passes, it is worth being precise about what the per-step handoff conditions actually check — because understanding their limits is understanding why the verification pass exists.

A handoff condition is specific, testable, and binary. It checks whether a particular step produced the particular output that step was supposed to produce. *The new function exists at `auth/login.py`. It imports only from the standard library. It passes the three named test cases. It modifies no file outside `auth/`.*

That condition tells you the step is done. It does not tell you the *build* is done. The build is the integration of all the steps. The integration can produce a failure that no single step's handoff would have caught — a table that is technically correct markdown and practically unreadable on the device the spec said it would be read on.

The phone-readability failure is not a handoff failure. Every step that produced the output passed its condition. It is a *spec-needs* failure: the final state of the system does not satisfy a User need that was never encoded as a per-step check. This failure lives above the individual steps. It is visible only when you compare the complete output against the written spec.

This is not a criticism of the handoff discipline. The handoffs protect against the wrong thing being built at each step. The verification pass protects against the right things being built at each step that combine into the wrong whole. Both are necessary. They catch different failures.

| Layer | What it checks | Example failure it catches | Example failure it misses |
|---|---|---|---|
| Per-step handoff condition | Did *this step* produce the artifact and behavior the spec required? | Categorization rules hardcoded instead of loaded from a config file — caught at Step 4 | The auto-incremented schema key — technically correct, makes a later step harder |
| Pass 1–2 (functional + edge-case) verification | Does the whole script run, do tests pass, do edge cases — empty log, single-asset log, special characters in paths — behave sanely? | Categorization rule that crashes on a path with an ampersand or unicode character | A markdown summary table that's technically valid and unreadable on a phone |
| Pass 3 (spec-needs) verification | Does the final build meet the User needs as written — *not* what the script does, but what it *delivers* to the user? | "Readable on phone" — seven-column markdown table cut off in portrait mode | A failure mode the spec never names — if the User need wasn't written down, Pass 3 can't check it |

---

## The three passes

The verification step has three layers, run in order.

**Pass 1: functional verification.** Does the script run? Do the tests pass? Are the obvious failure modes handled? This is the pass most people stop at. It is necessary and not sufficient. A build that passes every test and violates its own spec at the user-need level passes Pass 1 completely.

**Pass 2: edge-case verification.** Does the script handle the cases the spec defined as within scope but unusual? Empty inputs. Invalid inputs. Large inputs. The specific boundary conditions named during planning.

For Seth's build: an empty export log (does the script handle a build with no new assets?); a log with one asset; a duplicate asset within a single log (does dedup catch it?); an asset path with special characters from a Windows export (does it parse?). All passed.

Pass 2 catches failures that Pass 1 misses because the average case works and the edge case does not. A categorization rule that handles every normal folder path and fails on a path with an ampersand or unicode character passes every test written for normal paths. Pass 2 catches it — if you thought to test it.

**Pass 3: spec-needs verification.** Does the final state of the build meet the User needs from the spec? Not what the spec said the script should *do* at the technical level — that is Pass 1. Not what edge cases it handles — that is Pass 2. What the spec said the script should *deliver* at the user-need level.

This is where the phone-readability failure lived. "Output is readable on phone" is a User need, not a technical specification. No per-step handoff was written against it. No test verified it. Pass 3 is the only pass that checks it, and Pass 3 checks it by doing the thing: opening the output on a phone and reading it.

Pass 3 verification is manual, specific, and tied to the spec's written User needs. You go down the list. You check each one against the actual system state. You mark pass or fail. You fix the fails.

![Three stacked pass cards connected by downward arrows. Pass 1 — Functional: does it run? Pass 2 — Edge case: does it do what was asked? Pass 3 — Spec needs (highlighted in red): does it survive real use? Each card has side annotations listing what the pass catches and what it cannot see.](images/14-verification-fig-01.png)

*Figure 14.1 — The verification sequence. Three passes — execution, specification, intent. Pass 3 is highlighted because it is the only pass that checks against the spec's User needs.*

---

## Why stopping at Pass 1 is the common case

Most builds stop at Pass 1. The tests pass; the build is declared done. This is not negligence. It is the natural stopping point when the tests were written to verify technical correctness and the User needs were never translated into verification steps.

The translation is the work. "Readable on phone" needs to become "open the output on an iPhone-preset Chrome devtools viewport, portrait mode, default font size, and verify no horizontal scroll is required to read the asset-count-by-folder section." Once the translation is done, Pass 3 is fast — you are following a checklist, not making judgment calls.

The problem is that the translation is never done as a deliberate step. It is supposed to happen naturally, as part of writing the spec well. Seth's spec said "readable on phone." The translation into "no horizontal scroll in portrait mode" did not happen. The verification pass surfaced the gap.

The lesson Seth encoded in his AGENTS.md: *"For any output that will be read on phone, verify column count / line width before declaring done."* The next spec will say "readable on phone in portrait mode at default font size; no horizontal scroll; tested on iPhone-preset Chrome devtools before declaring done." The phrasing makes the Pass 3 verification structural — you have the check listed — rather than incidental, where you catch it only if you happen to think of it.

This is the pattern. The first time a User need is vague, Pass 3 surfaces the gap. The gap gets translated into a more precise formulation. The more precise formulation becomes either an AGENTS.md entry or a template for the next spec. The verification pass is how the discipline tightens over time.

---

## The three passes are cumulative

A point worth stating explicitly.

Pass 2 does not replace Pass 1. Pass 3 does not replace Pass 2. They are cumulative — each pass catches things the previous passes cannot, and a failure at any pass means the build is not done.

A build that passes Pass 1 and fails Pass 2 has a working happy path and a broken edge case. Fixing the edge case may not require revisiting Pass 1, but Pass 1 needs to remain green after the fix. A build that passes Pass 1 and Pass 2 but fails Pass 3 has correct technical behavior and unmet user needs. Fixing the user-need failure may require changing the technical behavior, which means re-running Pass 1 and Pass 2 after the fix.

The sequence is: Pass 1, then Pass 2, then Pass 3. If any pass fails, fix and restart the sequence for the affected area. The sequence ends when all three passes are green for the complete build.

The phone-readability fix — changing the table to a list — required Seth to re-run Pass 1 (does the list render correctly in markdown?) and Pass 2 (does the list handle the edge cases the table did: categories with long names, categories with zero transactions?) before running Pass 3 again (does the list fit on the phone screen without horizontal scroll?). Fixing a Pass 3 failure introduced potential Pass 1 and Pass 2 regressions. He checked both. Both were fine. Pass 3 re-ran. The build was done.

![A descending step chart. X-axis is verification iteration with stops labeled start, P1, P2, P3, done; y-axis is defects remaining. The red step line drops sharply at each pass — largest drop at Pass 1 (mechanical), smaller at Pass 2 (edge cases), smallest at Pass 3 (spec needs, highlighted). A small dashed re-run-on-fix arc connects the Pass-3 region back into earlier passes; a residual marker shows what no pass catches.](images/14-verification-fig-02.png)

*Figure 14.2 — Verification loop flow. Each pass drops the defect count by what only that pass can catch. Fixing a later-pass failure requires re-running earlier passes — the loop, not the line, is the structure.*

---

## The post-build learning document

The verification pass produces *what happened*. The post-build learning document is the artifact that records *what you learned from what happened*.

Five sections:

**What I built.** One paragraph, plain language. The kind of description you would give a friend who asked what you spent the afternoon on.

**What I delegated to Codex and why.** The specific work Codex did, with the reasoning.

**What I kept for myself and why.** The mirror. The work that was irreducibly yours — the decisions that required your judgment or your specific knowledge of the project.

**What I learned that I didn't know before.** The discoveries. Features of the language or framework you understand better. Codex behaviors you learned to anticipate. AGENTS.md entries you added.

**What I would do differently.** The honest section. A specific decision you would reverse. Something you would change in the spec, the plan, the AGENTS.md, or the execution.

One page. Thirty minutes. The document is the artifact that converts the experience of building into the capacity to build the next thing better.

Write it now. Memory degrades fast. The specific details that make the "What I would do differently" section useful — the exact formulation failure, the precise Codex behavior you were surprised by — are clearest in the hour after the build is done. Written later, they become general lessons. General lessons are less useful than specific ones.

---

## What Seth's post-build document said

Here is Seth's document for the asset-budget build, reproduced in full:

> **What I built.** A Python script that processes Godot export logs from Haunt & Harvest into a per-build asset-budget summary with deltas against the previous build, written to a markdown file. The script is local-only, idempotent, and produces output I can read on my phone between play sessions.
>
> **What I delegated to Codex and why.** The SQLite schema migration, the export-log parsing logic, the dedup logic, the categorization rule engine, the markdown rendering. These are pattern-completion tasks where Codex is faster than I am and where my verification — run the script, check the output — is straightforward.
>
> **What I kept for myself and why.** The Intent Layer of the spec — what this script is for, and what it does not do (it does not collapse same-path same-size audio assets automatically; it flags them for me). The schema design choice: content-hash key versus auto-incremented integer. The choice mattered for idempotency, which was a User need. The categorization rules in the config file — these encode my judgment about which folders count as "AI," "inventory," "audio," etc., and are irreducibly mine. The phone-readable formatting decision, caught only in Pass 3.
>
> **What I learned that I didn't know before.** Codex's default schema design biases toward auto-incremented integer keys. When idempotency is a requirement, content-hash keys are usually right, and Codex will not propose them unless you ask. Pass 3 verification catches things Pass 1 and Pass 2 cannot — specifically, the phone-readability issue that no per-step handoff would have caught. AGENTS.md got an entry: "for any output that will be read on phone, verify column count and line width before declaring done."
>
> **What I would do differently.** I would write the User needs section of the spec more concretely. "Readable on phone" was true at the level of the markdown file being a markdown file, and false at the level of the layout actually fitting a phone screen. The next spec will say: "readable on phone in portrait mode at default font size; no horizontal scroll; tested on iPhone-preset Chrome devtools before declaring done."

280 words. The AGENTS.md has been updated. The next build starts from a better place. The document is not a record of failure — it is the record of what the verification pass was designed to produce: specific, actionable lessons that would not have surfaced without the discipline.

---

## What the verification pass cannot catch

A few things the three passes do not promise.

**Failures whose downstream effects have not yet surfaced.** A build that introduces a subtle behavior change affecting a scenario you have not tested will pass all three passes today and fail tomorrow. The verification covers what you know to check.

**Failures in dependencies.** Codex may generate code that calls an external library whose behavior has changed since the model's training data. Pass 2 can catch this if you check the *meaning* of the output rather than just its presence. It does not catch it automatically.

**Failures of formulation.** If the formulation was wrong — you wanted X and asked for Y — the verification will confirm Y was delivered correctly. The mismatch surfaces only when you notice the system does not do X. Upstream formulation work (Chapter 8) prevents these. The verification pass confirms that what was built is what was specified. It cannot confirm that what was specified is what was needed.

The verification pass is the last line of defense. It is not the only line.

---

## What would change my mind

The chapter's strong operational claim: the three-pass verification catches a meaningful share of failures that single-pass mechanical verification misses, specifically failures whose handoff conditions were under-specified.

What would soften that claim: a controlled comparison — same class of builds, with single-pass and three-pass verification — that found no measurable difference in post-deployment failure rates. I expect the difference to be substantial because Pass 3 is the only pass that catches spec-needs failures, and spec-needs failures are among the most expensive: they pass all technical checks and manifest only in use.

What remains open: when the three passes are worth automating. The chapter describes them as manual. For builds that run repeatedly, automating Pass 2 and parts of Pass 3 is worth the upstream effort. The threshold for automation is not addressed here.

---

## What is still puzzling

**Whether the post-build document should be shared.** Honest post-build documents contain self-criticism. Sharing creates incentives to perform rather than reflect. The book's working answer: candid and private, redacted and public when the specific lesson would help someone else. The specific phone-readability lesson — "for outputs read on phone, verify column count before declaring done" — is worth sharing. The specific decision Seth would reverse about the schema is worth keeping private until the lesson is sufficiently general to be useful without the embarrassing detail.

**The relationship between manual verification and CI/CD.** Industry continuous integration is the institutional version of the three passes — automated, running on every commit, catching regressions as they are introduced rather than at the end of the build. Whether students should learn the manual version first and the automated version later, or start automated, is open. The book's working answer: manual first, because the discipline of writing the verification criteria explicitly is the foundation for writing the automated tests correctly. Students who skip manual verification and go straight to test-writing often write tests that pass without being informative.

**Whether three passes is the right number.** The chapter's three-pass structure maps onto three concerns: technical correctness, edge-case coverage, and user-need satisfaction. These are the three concerns that appear consistently across the book's worked examples. A fourth pass — checking against external constraints like accessibility standards or security requirements — is defensible for some builds. The chapter does not prescribe it; the principle would extend naturally.

---

## AI Wayback Machine

🕰️ **Barbara Liskov** (born 1939) — computer scientist whose work on behavioral subtyping and formal specification formalized the principle that *"correct" must be defined before it can be verified*.[^1] The Liskov Substitution Principle, articulated with Jeannette Wing in 1994, made the connection precise: a program's correctness is a property of its *specification*, not just of its execution. You cannot verify correctness against a standard that has not been written down.

![Barbara Liskov](../images/barbara-liskov-1vp.png)

*Puppet Art by [Nik Bear Brown](https://www.nikbearbrown.com/).*

The three-pass verification is Liskov applied to AI-assisted builds. Pass 1 checks execution — does the code run? Pass 2 checks against the spec's defined boundaries — does it handle what the spec said it would handle? Pass 3 checks against the spec's intent — does the output deliver what the spec said the user would receive?

The hierarchy — execution, then specification, then intent — is the hierarchy Liskov's framework formalizes. The failure Seth caught at Pass 3 would not have been catchable at Pass 1 or Pass 2 precisely because no specification had been written for it at the step level. The spec existed at the build level (in the User needs section). Liskov's insight is that the level of the specification determines the level at which correctness can be verified. Seth's Pass 3 was checking against the build-level specification. Nothing in the per-step handoffs could have substituted for it.

The post-build learning document, in turn, is the practitioner's record of where the specification was tight enough to verify against, and where it was not — which is the cognitive event that makes the next build's specification tighter.

---

## Bridge

You have the discipline. The next chapter hands you the build.

---

## Exercises

**Warm-up**

1. *(Tests: what each pass checks)* For each of the following failures, name which pass would catch it and explain why the earlier passes would not: (a) the script crashes when the input CSV is empty; (b) the output file renders in a browser but the font size falls below 16px on mobile; (c) a merchant name containing an ampersand causes a silent parse error that drops the transaction; (d) the script runs correctly but produces a different result on the second run than the first.

2. *(Tests: the handoff / verification distinction)* A student says: "My per-step handoff conditions all passed, so my build is done." What is the specific failure mode this reasoning misses? Name the type of failure and describe a concrete example from a build in this book or one of your own.

3. *(Tests: translating User needs into Pass 3 checks)* Translate the following vague User needs into testable Pass 3 verification steps. For each, state exactly what you would do, what tool or viewport you would use, and what result would count as a pass: (a) "the output should be readable"; (b) "setup should be quick"; (c) "errors should be handled gracefully."

**Application**

4. *(Tests: running the three passes)* Run a three-pass verification on a build you have completed in this course. Document each pass: what you checked, what passed, what failed. For any failure: describe the fix, then state which earlier passes you re-ran after the fix and what the results were.

5. *(Tests: the cumulative structure)* You fix a Pass 3 failure by changing the output format from a table to a list. Identify every Pass 1 and Pass 2 check that could plausibly be affected by this change. For each, write the specific re-verification step you would run.

6. *(Tests: writing a post-build learning document)* Write a post-build learning document for a build you have completed. All five sections. The "What I would do differently" section must name a specific decision — not a general lesson — and explain exactly what you would change in the spec, the AGENTS.md, or the execution. A post-build document that contains only general lessons does not meet the standard; specific ones do.

**Synthesis**

7. *(Tests: verification + formulation together)* Seth's Pass 3 failure — the phone-readability table issue — was ultimately a formulation failure: the User need "readable on phone" was not precise enough to encode as a per-step handoff condition. Trace the failure all the way back: where in the formulation workflow (Chapter 8) would the more precise phrasing have been written? What Ask Mode interrogation question might have surfaced "portrait mode, no horizontal scroll" as a constraint before Code Mode began? What would Seth's spec have said if the formulation had been done correctly?

8. *(Tests: what verification cannot catch)* The chapter names three classes of failure the three passes cannot catch: downstream failures, dependency failures, and formulation failures. For each class, identify which earlier discipline in the book is the primary defense — and explain why the verification pass cannot substitute for it.

**Challenge**

9. *(Open-ended)* The chapter's three-pass structure maps onto three concerns: technical correctness, edge-case coverage, and user-need satisfaction. Some builds have a fourth concern not covered by the three passes — accessibility requirements, security constraints, or compliance with external standards. Design a Pass 4 for one of these concerns. Your Pass 4 must: name the concern precisely, describe the checks that constitute the pass, explain what a failure looks like, and identify the spec section (from Chapter 8's minimum viable spec) that would need to exist for Pass 4 to be checkable. Explain why the concern belongs in a separate pass rather than in Pass 3.

---

[^1]: Liskov, B. and Wing, J. M. "A Behavioral Notion of Subtyping." *ACM Transactions on Programming Languages and Systems* 16, no. 6 (1994): 1811–1841.

---
