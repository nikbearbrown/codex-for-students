# Research: Chapter 9 — Handoff Conditions and the Dangerous Middle
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** Not "looks good." A specific, testable condition that must be true before the next step begins.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Hopper, Grace. "The Education of a Computer." *ACM Symposium*, 1952.** "Done" must be defined before verified. Hopper's voice carries the chapter.
- **Meyer, Bertrand. "Applying 'Design by Contract.'" *IEEE Computer* 25, no. 10 (1992): 40–51.** Pre/postconditions/invariants. A handoff condition is a postcondition.
- **Hoare, C.A.R. "An Axiomatic Basis for Computer Programming." *CACM* 12, no. 10 (1969): 576–580.** Hoare triples — the formal foundation.
- **Leveson, Nancy G. *Engineering a Safer World*. MIT Press, 2011.** STAMP. Verification of the *controlled variable*, not just the action.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC's Ch 9 quotes: "Avoid excessive looping or repetition; if you find Codex re-reading or re-editing the same files without clear progress, stop and reframe." The OpenAI guidance on iteration limits anchors the chapter's "two-failed-corrections rule."

### Key empirical cases

- **Seth's "compiled, passed tests, six-days-later silent failure" (TIKTOC opening).** Real instance required. The chapter's emotional anchor.
- **Three handoff conditions analyzed (TIKTOC worked example).** One strong, one weak, one that missed the dangerous middle. Author should produce these from real Codex sessions.
- **The "while I'm here" scope creep.** Codex suggests an unrelated improvement during a build. The chapter's prescription: log it in the task queue, do not execute.
- **The semantically-wrong-but-tests-pass pattern.** Recurring failure: Codex's generated code passes Codex's generated tests. Both encode the same misunderstanding. The dangerous middle's defining shape.
- **Studies of software regressions despite passing tests.** Documented in software-engineering literature (Beizer's *Software Testing Techniques* 1990, modern empirical software-engineering work). Useful for the chapter's claim that tests are necessary but not sufficient.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Tests passing ≠ code correct.** Established in software engineering since the 1970s. The chapter operationalizes this for AI-assisted work.
- **Postconditions are not implied by syntactic success.** Established in formal methods (Meyer, Hoare).
- **The dangerous middle is real for AI-generated code.** Anthropic's RCT didn't name it, but the "iterative AI debugging" low-scoring pattern partially captures it. The chapter names the phenomenon directly.

### What is disputed

- **Whether the chapter's "revert and respecify, do not correct forward" rule is right.** Contrarian relative to typical practitioner habits (incremental fixes). The book's position: forward corrections pollute the context window, accumulating failed approaches that confuse Codex. Defensible but should be argued.
- **Two failed corrections as the threshold.** TIKTOC's rule. Some practitioners use three; some use one. The book's choice is operational, not theoretical.
- **Best-of-N as verification tool.** TIKTOC frames Best-of-N (generate multiple, evaluate, select) as Ch 9's verification tool. **Status check (OQ-6):** as of May 2026, Best-of-N is not a named user-facing Codex UI feature; the API supports parallel completions via `n` parameter. The chapter should reframe Best-of-N as a *technique* rather than a button — manually run the same prompt twice in separate sessions, or use the API with `n=3`. This matters; the chapter currently reads as if a Best-of-N button exists.

### What has changed recently (last 5 years)

- AI-generated code regressions are a documented industry issue (2024–2026). The chapter is teaching the discipline that the industry is naming.
- Codex's plan mode formalizes per-step approval — the gate built into the tool. The chapter's argument is that approval ≠ verification; the student still must check each step against intent.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 9 sits in software function / login system extended to dangerous-middle cases.)

- **The login function that "works."** Codex generates `login()`. Tests pass. Six days later: silent failure. The handoff condition the student wrote was "tests pass." The condition that wasn't checked: rate-limiting, lockout-after-failed-attempts, password-hash-format compatibility with existing users. All would have failed but none were in the tests.
- **The database migration that "succeeds."** Codex generates a migration adding a column. Migration runs. Exit 0. Days later: an unrelated query fails because the new column changed an index Codex didn't know existed. Handoff condition was "migration runs"; should have been "all existing queries that hit this table still execute in their previous time bound."
- **The refactor that "passes review."** Codex refactors a function. Tests pass. Code review approves. Production: the refactor changed observable timing in a way a downstream service depended on. Handoff condition was "tests pass + review approved"; should have included a load-test step that exercised the timing-dependent path.

---

## 4. The Book's Thesis Connection

Ch 9 is the empirical hard core of the thesis. If Ch 1's claim is "exit 0 ≠ correct" (terminal version), the editor/agent version is "tests pass + Code Mode says done ≠ correct." Ch 9 gives "correct" a positive operational definition.

The chapter's contribution:

1. **Names the dangerous middle for code.** The book's distinguishing concept relative to the gh-copilot book. The editor version is code that compiles, passes tests, satisfies the spec *literally*, and is still wrong because tests and spec encoded the same misunderstanding.
2. **Operationalizes verify.** Ch 4's gate ends with "verify." Ch 9 says *how*: with handoff conditions written before the prompt runs, checked after.
3. **Sets the threshold for acceptance.** The discipline is binary at the handoff condition. The student either knows what "done" means for this step or doesn't.

Student-supplied capacity: only the student knows what "correct" means for *their* task in *their* project.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Grace Hopper (1906–1992).** Strong fit.

Candidates:
- **Grace Hopper** (1906–1992, USA, computer scientist / US Navy Rear Admiral). Named. COBOL, A-0, the nanosecond. Diversity: woman, American, mid-20th-century. **Excellent.**
- **Margaret Hamilton** (born 1936, USA, software engineer). Apollo defensive programming. Already a candidate for Ch 6.
- **Tony Hoare** (born 1934, UK, computer scientist). Hoare logic. Substantive fit excellent (Hoare triples are the formal foundation). Less famous to high schoolers.

Recommendation: keep Hopper. Hoare nod in body text (one sentence) gives credit to the deeper lineage without changing the figure.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Full Act Two framework. Ch 9 is the synthesis.

### Common misconceptions to disarm
- **"If tests pass, code is correct."** Test passing means the code matches the tests. If the tests encode the same misunderstanding as the code, both pass and both are wrong.
- **"Code review catches the dangerous middle."** Sometimes. Often not — the reviewer reads the diff in isolation, not against the project's full state.
- **"Forward correction is fine."** Pollutes context window; accumulates failed attempts; Codex gets confused.
- **"The dangerous middle is rare."** It is the most common failure mode for AI-assisted work in the book's reader population.

### Effective instructional sequences
- **Lead with a case, not a definition.** Seth's six-days-later moment.
- **Strong vs. weak handoff conditions, side by side.** TIKTOC's table.
- **The two-failed-corrections rule.** Make memorable; make specific.
- **Revert-and-respecify drill.** Apply-level exercise: reader takes a real failed correction and rewrites the specification.

### Known failure modes
- **Case-study circus.** Three cases is plenty. One detailed case beats five thumbnails.
- **The dangerous middle as exotic.** Frame as the typical failure mode.
- **Best-of-N as button.** Need to reframe as technique (see Section 8). Don't promise a button that doesn't exist.

### What separates understanding from memorization
A reader who *understands* Ch 9 can take a command they intend to run and write a handoff condition that *isn't* "tests pass" — one that specifies what the result should look like in *their* environment, in advance. A reader who memorized Ch 9 can recite the dangerous-middle definition without producing a real handoff condition.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: The handoff condition as a gate between build steps.] -->`** Step N → [handoff condition check] → Step N+1. Failure branch: revert and respecify. Editorial style.
- **`<!-- → [TABLE: Strong vs. weak handoff conditions.] -->`** Five examples (mapped to TIKTOC's editor/code domain):

  | Task | Weak condition | Strong condition |
  |---|---|---|
  | Login function | Tests pass | Tests pass + rate-limiting active + existing users can still log in + lockout works |
  | Database migration | Migration runs | Migration runs + all existing queries return same data in same time bound |
  | Refactor | Tests pass + review approved | Tests pass + behavioral invariants verified + downstream consumers still get expected timing |
  | Bug fix | Bug not reproducible | Bug not reproducible + no new failures in the same module + regression test added |
  | New feature | Feature works in demo | Feature works in demo + works on real data + breaks no existing flow + observable from monitoring |

---

## 8. Open Questions and Research Gaps

- **Best-of-N status (OQ-6).** Critical. As of May 2026, no named Best-of-N button in Codex UI. The chapter currently reads as if there is one. Two fixes:
  - (a) Reframe Best-of-N as the *technique* (manually re-prompt, or use API `n` parameter). The conceptual content survives.
  - (b) Tie Best-of-N usage to API-access readers; sidebar for ChatGPT Plus / Edu readers explaining the manual version.
  Recommendation: (a), and consider whether the chapter even needs Best-of-N as a named concept — the discipline (generate-evaluate-select) can be taught without the marketing term.
- **A real Codex session for the chapter (TIKTOC Hard-Chapter requirement).** TIKTOC: "must produce genuine discomfort... requires a real Codex session, not a hypothetical." Author and Seth must produce one before drafting.
- **Test-quality vs. build-quality.** TIKTOC's Ch 13 has this; Ch 9 should foreshadow. The chapter should briefly distinguish "test passes because the test is bad" from "test passes because the code is right." Foreshadowing only — Ch 13 expands.

---

## 9. Sourcing Notes

- **Hopper 1952** — ACM DL.
- **Meyer 1992** — IEEE; preprints exist.
- **Hoare 1969** — open access *CACM*.
- **Leveson 2011** — MIT Press.
- **OpenAI engineer-voice quotes** — forum.openai.com Dec 2025.
- **Beizer 1990** *Software Testing Techniques* — out of print but widely available.
