# Chapter 13 — Verification: How You Know It Works

> The build is done when it passes the handoff conditions — not when Codex says it's done.

---

## Learning outcomes

1. **(Apply)** Run a structured verification pass on a completed build using explicit criteria from the spec.
2. **(Analyze)** Distinguish build failures from test quality gaps.
3. **(Evaluate)** Produce a post-build assessment.

---

## Opening

Seth's personal-finance build had finished its last step. The script processed his February CSV exports. The summary markdown rendered: net worth, cash flow by category, four uncategorized transactions for manual review. Codex's report said the build was done. The handoff conditions per step had all passed.

Seth was about to declare it done.

Then he ran the verification pass — the one he almost skipped because the per-step handoffs had been clean. He checked the output against the User needs from the spec:

- *Idempotent: running twice produces the same output.* He ran the script a second time. The output was identical. **PASS.**
- *Dedup is reliable across CSV format differences.* He inspected the SQLite database. The transaction count matched the unique transactions from both CSV files. **PASS.**
- *Categorization rules are in a config file.* The rules were in `~/finance/config/categories.yaml`. **PASS.**
- *Output is one markdown file per month, under 200 lines.* The file was 156 lines. **PASS.**

So far so good. But the spec also said: *"Output is readable on phone."* This was a User need but it had not been a per-step handoff condition. Seth opened the markdown on his phone. It rendered. But the cash-flow-by-category section, which used a Markdown table with seven columns, did not fit on the phone screen. It scrolled horizontally; the categories at the right edge were cut off.

The build had passed every per-step handoff. The User need from the spec was technically met (the output *is* readable, in the sense that you can read it by scrolling). The User need was *not* met in the way that mattered — the table format was wrong for the device.

Seth fixed it. The table became a list of categories with cash-flow amounts, each on its own row. The output fit on the phone. The build is now genuinely done.

This is the chapter. The verification pass at the level of the *whole build*, against the spec's User needs, catches what the per-step handoffs cannot.

<!-- → [DIAGRAM: The verification sequence — three passes. Pass 1: functional verification. Pass 2: edge case verification. Pass 3: SDD/spec needs verification. Each pass has a binary result and a path to resolution.] -->

---

## The three verification passes

The verification step has three layers, run in order.

**Pass 1: functional verification.** Does the script run? Do the tests pass? Are the obvious failure modes handled? This is the pass most students stop at. It is the necessary first check; it is not sufficient.

**Pass 2: edge-case verification.** Does the script handle the cases the spec defines as out-of-bounds? Empty inputs, invalid inputs, very large inputs, the unusual cases the spec named or that you anticipated during planning? This pass catches the cases where the implementation works for the average case and fails for the edge.

For Seth's build, Pass 2 included: an empty CSV (does the script handle it?); a CSV with one transaction (does it handle a single-row case?); a duplicate within a single CSV (does dedup catch it?); a transaction with special characters in the description (does it parse?). All passed. Pass 2 cleared.

**Pass 3: spec needs verification.** Does the final state of the script meet the User needs from the spec? Not what the spec said the script should *do* at the technical level (Pass 1), not what edge cases it should handle (Pass 2), but what the spec said the script should *deliver* at the user-need level.

This is the pass that catches what Seth caught. The User need was "readable on phone." The Pass 1 verification (does the script produce a markdown file?) passed. The Pass 2 verification (does the script handle edge cases?) passed. The Pass 3 verification (does the markdown deliver the user need?) failed on the table-width issue. Pass 3 is the protection against builds that are technically correct and practically wrong.

The three passes are *cumulative*. Pass 2 catches things Pass 1 missed. Pass 3 catches things Pass 2 missed. Stop at Pass 1 and you ship the build that passes tests and does not serve the user need.

---

## The post-build learning document

The verification pass produces *what happened*. The post-build learning document is the artifact that records *what you learned from what happened*.

Five sections:

1. **What I built.** One paragraph, plain language. The kind of description you would give a friend who asked what you spent the afternoon on.
2. **What I delegated to Codex and why.** The specific work Codex did, with the why.
3. **What I kept for myself and why.** The mirror. The work that was irreducibly yours.
4. **What I learned that I didn't know before.** The discoveries. The features of the language or framework you understand better. The Codex behaviors you learned to anticipate. The AGENTS.md entries you added.
5. **What I would do differently.** The honest section. A specific decision you would reverse. Something you would change in the spec, the plan, the AGENTS.md, or the build's execution.

The document is one page. It takes thirty minutes to write. It is the artifact that converts the experience of building into the capacity to do the next build better than this one.

The discipline is to write it *now*, while the lessons are fresh, not later.

---

## Worked example: Seth's post-build document

> **What I built.** A Python script that processes bank CSV exports into a monthly net-worth and cash-flow summary, written to a markdown file. The script is local-only, idempotent, and produces output I can read on my phone.
>
> **What I delegated to Codex and why.** The SQLite schema migration, the CSV parsing logic, the dedup logic, the categorization rule engine, the markdown rendering. These are pattern-completion tasks where Codex is faster than I am and where my verification (run the script; check the output) is straightforward.
>
> **What I kept for myself and why.** The Intent Layer of the spec — what this script is for and what kinds of decisions it does NOT make (it does not categorize ambiguous transactions automatically; it flags them for me). The schema design choice (content-hash key vs. auto-incremented — the choice mattered for idempotency, which was a User need). The categorization rules in the config file — these encode my own classification of my transactions and are irreducibly mine. The phone-readable formatting decision (table → list) caught only in Pass 3.
>
> **What I learned that I didn't know before.** That Codex's default schema design biases toward auto-incremented integer keys; when idempotency is a requirement, content-hash keys are usually the right choice and Codex will not propose them unless you ask. That Pass 3 verification (against User needs from the spec) catches things Pass 1 and Pass 2 do not — specifically, the phone-readability issue that no per-step handoff would have caught. AGENTS.md got an entry: "for any output that will be read on phone, verify column count / line width before declaring done."
>
> **What I would do differently.** I would write the User needs section of the spec more concretely. "Readable on phone" was true at the level of the markdown file being a markdown file, and false at the level of the layout actually fitting on a phone screen. The next spec will say "readable on phone in portrait mode at default font size; no horizontal scroll; tested on iPhone-preset Chrome devtools before declaring done." That phrasing would have made the Pass 3 verification structural rather than incidental.

Roughly 280 words. The document is the artifact Seth will reference the next time he builds something like this. The AGENTS.md has been updated with the phone-readability lesson. The next build starts from a better place.

---

## What the verification pass cannot catch

A few things the discipline does not promise.

**Failures whose downstream effects have not yet surfaced.** A build that introduces a subtle behavior change that only matters in a scenario you have not yet tested will pass all three passes today and break tomorrow. The verification covers what you know to check.

**Failures in dependencies.** Codex may generate code that calls an external library whose behavior has changed since the model's training. The verification can catch this if you check the *meaning* of the output (Pass 3) and not just its presence (Pass 2).

**Failures of formulation.** If the formulation itself was wrong (you wanted X and asked for Y), the verification will confirm Y happened. The mismatch surfaces only when you notice the system does not do X. The upstream formulation work (Chapter 7) prevents these.

The verification pass is necessary, not sufficient. It is the last line of defense, not the only line.

---

## Common misconceptions

**"If the tests pass, the build is done."** Tests are Pass 1. Two more passes follow.

**"My script's report says it worked."** The script reports what it was asked to report on. The verification checks what the script may not have been asked to report on.

**"Intent verification is just gut feeling."** No. Intent verification is comparison against the *written* User needs in the spec. The spec exists in writing; the verification compares the system state to the writing.

**"I'll write the post-build document later."** You will not. Write during. Memory degrades fast.

**"Post-build is bureaucratic."** Five sections, thirty minutes, one page. The artifact pays back the next time you build something similar.

---

## Exercises

1. **(Apply)** Run a three-pass verification on your Chapter 12 build. Document each pass's result. For any pass that catches a failure, fix it and re-verify.

2. **(Analyze)** A test in your build passes but you are not sure it is testing the right thing. Diagnose and rewrite the test to check what you intended.

3. **(Create)** Write a post-build learning document for your Chapter 12 build. Five sections. Be especially honest in the "What I would do differently" section.

---

## What would change my mind

The chapter's strong operational claim is that **the three-pass verification catches a meaningful share of failures** that single-pass mechanical verification misses. If a controlled comparison found no measurable difference in the rate of post-deployment failures between one-pass and three-pass verification, the second and third passes become optional. The chapter would still recommend them; the urgency softens.

I expect the difference to be substantial because Pass 3 is the only pass that catches failures whose handoff conditions were under-specified, and these failures are the most expensive ones.

---

## Still puzzling

- **When verification becomes worth automating.** The three passes are manual in the chapter. For builds run repeatedly, automating Pass 2 and parts of Pass 3 is worth the upstream effort.

- **Whether the post-build document should be shared.** Honest post-build documents contain self-criticism; sharing produces incentives to perform. The book's working answer: candid private; redacted public when useful.

- **The relationship between this chapter's verification and CI/CD.** Industry CI/CD is the institutional version of the three passes. Whether students should learn the manual version first and the automated version later, or start automated, is open.

---

## AI Wayback Machine

🕰️ **Barbara Liskov** (born 1939) — computer scientist whose work on behavioral subtyping and formal specification formalized the principle that *"correct" must be defined before it can be verified*. The Liskov Substitution Principle, articulated with Jeannette Wing in 1994, made the connection precise: a program's correctness is a property of its *specification*, not just of its execution.[^1] The chapter's three-pass verification is Liskov applied to AI-assisted builds. Pass 1 (mechanical) checks execution. Pass 2 (edge cases) checks against the spec's defined boundaries. Pass 3 (User needs) checks against the spec's intent. The hierarchy — execution → specification → intent — is the hierarchy Liskov's framework formalizes. The post-build learning document, in turn, is the practitioner's record of where the hierarchy held and where it did not, which is the cognitive event that makes the next build's hierarchy tighter.

---

## Bridge

You have the discipline. Chapter 14 hands you the build.

---

[^1]: Liskov, B. and Wing, J. M. "A Behavioral Notion of Subtyping." *ACM Transactions on Programming Languages and Systems* 16, no. 6 (1994): 1811–1841.
