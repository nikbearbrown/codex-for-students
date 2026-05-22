# Research: Chapter 13 — Verification: How You Know It Works
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** The build is done when it passes the handoff conditions — not when Codex says it's done.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Liskov, Barbara H. and Jeannette M. Wing. "A Behavioral Notion of Subtyping." *ACM TOPLAS* 16 (1994): 1811–1841.** Behavioral contract.
- **Hoare, C.A.R. "An Axiomatic Basis for Computer Programming." *CACM* 12 (1969): 576–580.** Hoare triples.
- **Leveson, Nancy G. *Engineering a Safer World*. MIT Press, 2011.** STAMP. Verification of the controlled variable.
- **Argyris, Chris and Donald A. Schön. *Organizational Learning II*. Addison-Wesley, 1996.** Double-loop learning.
- **Beizer, Boris. *Software Testing Techniques*, 2nd ed. International Thomson, 1990.** Test quality vs. build quality.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC's Ch 13 quote: "I point Codex at low-coverage modules overnight and wake up to runnable unit-test PRs." Plus the implicit "test-quality check is still the human's job."

### Key empirical cases

- **Seth's "almost skipped the check" moment (TIKTOC opening).** Real instance required.
- **The "passing test, bad test" pattern.** Test passes because the test encoded the same misunderstanding as the code. The chapter's most important worked example. Author should produce a specific instance.
- **The Codex-generated test review pattern.** From OpenAI: Codex generates tests overnight; engineer reviews quality in the morning. The chapter's parallel: students using Codex for test generation must apply test-quality review.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Verification is layered.** Mechanical / scope / intent.
- **Tests passing ≠ correctness ≠ intent satisfaction.** Three distinct things.
- **Verification quality = specification quality.** A build can only be verified against criteria the student set.
- **Test quality is a separate concern from build quality.** Established in software engineering.

### What is disputed

- **Number of verification passes.** Two, three, four+. Book's three is defensible compact.
- **Whether intent verification can be partially automated.** Tests cover *specified* intent; unspecified intent is the dangerous middle. Manual intent verification remains.
- **Post-build document as ceremony.** For learning builds, the document is the learning artifact.

### What has changed recently (last 5 years)

- The 2025–2026 Codex generation supports "verification scripts" — Codex can run tests, linters, and type-checkers iteratively. Useful for mechanical and partial scope passes; intent remains human.
- Industry recognition (post-2023) that "the AI confirmed it worked" ≠ verification has produced explicit verification guidance from OpenAI and others.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 13 sits in the student build per TIKTOC.)

- **Functional pass for a login feature.** Tests pass; no errors. Mechanical OK.
- **Edge-case pass.** What happens with empty input? Wrong password format? Unicode in email? Race-condition between two simultaneous logins? Some specified by tests, some not. Manual verification supplies what tests don't.
- **Spec needs pass.** Does the result satisfy the formulation's intent? Does the user-facing flow make sense in the context of the existing app? Manual verification only — Codex cannot judge UX intent.

---

## 4. The Book's Thesis Connection

Ch 13 defines "done." If Ch 1's claim is the homework/quiz gap, Ch 13 is its positive operational answer: a build is done when it passes three passes and produces a post-build document.

The chapter's contribution:

1. **Three-pass discipline.** Operational; repeatable.
2. **Post-build document as learning artifact.** Capability proof.
3. **The student is the verifier.** Defended against "tests cover it."

Student-supplied capacity: intent. Intent-verification is the student's irreducible work.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Barbara Liskov (born 1939).** Excellent fit.

Candidates:
- **Liskov** — named. Behavioral subtyping. Woman, American. Excellent.
- **Nancy Leveson** (born 1948, USA, CS/safety). STAMP. Strong alternate.
- **Cliff Stoll** (born 1950, USA). *Cuckoo's Egg*. Sidebar material.

Recommendation: keep Liskov.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Full framework. Ch 13 verifies what Ch 1–12 built.

### Common misconceptions to disarm
- **"Tests pass = done."** No.
- **"If the build runs, it's done."** No.
- **"Intent verification is gut feeling."** No — comparison against the *written* formulation.
- **"Post-build is for big builds."** First builds especially benefit.

### Effective instructional sequences
- **Seth's near-miss as opener.** TIKTOC.
- **Three passes in sequence.** Cadence.
- **Post-build document template.** TIKTOC's five sections.
- **Reader's first post-build.** Apply.

### Known failure modes
- **Test-suite advocacy.** Verification ≠ just running tests.
- **Verification fatigue.** Mechanical and scope are fast; intent is slow.
- **Post-build as paperwork.** Frame as thinking tool.

### What separates understanding from memorization
A reader who *understands* Ch 13 can take a past build and produce a retrospective post-build document naming a real decision they would reverse. A reader who memorized Ch 13 can fill the template without self-criticism worth acting on.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The verification sequence — three passes.] -->`** Functional → edge case → spec needs. Binary at each. Resolution path on fail. Editorial style.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **"Almost skipped it" Seth moment.** Real case required.
- **Codex's verification-script integration.** Worth a paragraph on how Codex can run tests and report; how the student still owns intent.
- **Post-build document evolution across builds.** Shorter and sharper over time as framework internalizes.

---

## 9. Sourcing Notes

- **Liskov & Wing 1994** — ACM DL.
- **Hoare 1969** — open access *CACM*.
- **Leveson 2011** — MIT Press.
- **Argyris & Schön 1996** — Addison-Wesley.
- **Beizer 1990** — out of print but available.
- **OpenAI engineer quotes** — forum.openai.com Dec 2025.
