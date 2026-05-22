# Research: Chapter 8 — Writing Codex Prompts That Are Specifications
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** "Write me a login function" is not a prompt. A prompt names the thing, the invariants, the output format, and what not to touch.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Lovelace, Ada. *Notes on the Analytical Engine* (1843), Note G.** First computer program: explicit operations in dependency order. Conceptual ancestor of the five-element specification format.
- **Liskov, Barbara H. and Jeannette M. Wing. "A Behavioral Notion of Subtyping." *ACM TOPLAS* 16 (1994): 1811–1841.** Behavioral contract. A specification prompt is a contract.
- **Wirth, Niklaus. "Program Development by Stepwise Refinement." *CACM* 14 (1971): 221–227.** Specifications as successive refinement.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC quote: "Structure your prompt as if you are writing a GitHub issue — include file paths, component names, diffs, and doc snippets when relevant." Vendor authority.
- **OpenAI. "Codex Prompting Guide." developers.openai.com/cookbook/examples/gpt-5/codex_prompting_guide.** Official prompt-engineering guidance for Codex. The chapter's directly-cited primary practitioner source.

### Key empirical cases

- **The login function as prompt vs. specification.** TIKTOC's opening. Side by side. Codex's output for each. The difference is precision, not Codex.
- **The "give Codex a way to verify its own work" pattern.** TIKTOC: include tests or bash commands Codex can run. Codex iterating against feedback "produces significantly better results than single-pass generation." The chapter should show a specification that includes such a feedback mechanism.
- **The "GitHub issue" framing.** Anchor case: an actual well-written GitHub issue (a real one, from a public repo with permissive license) compared to the same task as a poorly-written prompt. Concrete and verifiable.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Specification clarity correlates with output quality.** Broad consensus in prompt-engineering literature.
- **The model has no memory between sessions.** AGENTS.md helps; the prompt is still the prompt. Specifications must be self-contained.
- **Negative constraints matter.** "Do not touch X" is not implied. Explicit prohibition is needed.
- **Feedback mechanisms in prompts dramatically improve results.** Established in OpenAI's own guidance. Specifications that include tests or verification commands trigger iterative refinement.

### What is disputed

- **The right level of formality.** XML-tagged structured prompts vs. natural-language paragraphs vs. hybrid. The book recommends a simple five-element format on memorability grounds.
- **Whether examples in the prompt help.** Useful for complex tasks; overhead for routine. The book recommends examples for non-obvious patterns only.
- **Step-by-step reasoning prompts.** Mixed evidence for current models. The book sidesteps by focusing on structure, not reasoning elicitation.

### What has changed recently (last 5 years)

- The 2024–2026 generation of Codex treats structured prompts more reliably than 2023. The five-element format ages well.
- OpenAI's published Codex prompting guide is direct vendor authority — the book is in alignment with the platform's own recommendations.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 8 sits in software function / login system.)

For a login function, the five elements:

1. **Operation:** Implement (not "design", not "explore").
2. **Invariants:** The existing `User` model stays unchanged; the session-management API stays unchanged.
3. **Context:** `AGENTS.md` Auth-conventions section; the existing `auth/session.ts` file as reference.
4. **Output format:** A new file `auth/login.ts` with a `login(email, password): Promise<Session | AuthError>` exported function. Tests in `tests/auth/login.test.ts`.
5. **Negative constraint:** Do not modify any file outside `auth/`. Do not add new dependencies. Do not change the `User` model.

Same task as one-line prompt ("write me a login function") produces generic auth code. Specification produces code that fits the project.

For a database migration:

1. **Operation:** Add a column (not "modify schema").
2. **Invariants:** No existing rows are dropped; the migration is reversible.
3. **Context:** The project uses `db/migrations/` with Knex; latest migration is `20260520-add-tags.js`.
4. **Output format:** A new migration file with up and down functions; idempotency check at the top.
5. **Negative constraint:** Do not modify existing migrations. Do not change the migration runner config.

---

## 4. The Book's Thesis Connection

Ch 8 bridges formulation (Ch 7) and execution (Ch 4's gate, applied in Ch 12). The formulation produces *what the student wants*; the specification produces *what the prompt must contain to elicit code that does what the student wants*.

The chapter's contribution:

1. **Specification is the bridge between intent and code.** A weak prompt cannot be saved by Ask Mode review or explain. The dangerous middle (Ch 9) lives in the gap between under-specified prompts and seemingly-correct output.
2. **The five elements operationalize the supervisory capacities.** Operation = PF. Invariants = EI. Context = TO. Output format = IJ. Negative constraint = PA + EI.
3. **Specifications include feedback mechanisms.** This is the chapter's distinctive operational claim — a specification that gives Codex a way to verify itself produces dramatically better results.

Student-supplied capacity: each element is information the model cannot supply. Specification is giving the model the context it lacks.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Ada Lovelace (1815–1852).** Excellent fit. Keep.

Candidates:
- **Ada Lovelace** (1815–1852, UK, mathematician). Named. Note G. **Substantive and diversity fit are both strong.**
- **Grace Hopper** (1906–1992, USA, computer scientist). Better fit in Ch 9.
- **Frances Allen** (1932–2020, USA, computer scientist). Compiler optimization. Diverse alternate.

Recommendation: keep Lovelace.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 7's formulation. The reader has decided what they want.

### Common misconceptions to disarm
- **"Specifications are for big projects."** No — any operation that can fail silently deserves the format.
- **"More words = better specification."** No. Precision per element, not verbosity.
- **"I can just edit the generated code."** Sometimes. The dangerous middle is when the code is not visibly wrong. Specification prevents; correction-after-the-fact doesn't.

### Effective instructional sequences
- **Before/after pairs.** TIKTOC's pattern. Five to seven pairs, each annotated with the missing element.
- **Reader rewrites their own prompt.** Apply-level exercise.
- **Negative-constraint emphasis.** The most under-used element. Devote disproportionate attention.
- **Feedback-mechanism examples.** Show specifications that include `pytest` invocation, type-checker commands, etc.

### Known failure modes
- **Specification as boilerplate.** Each element must be substantively decided.
- **Over-specification of trivial tasks.** Specify in proportion to consequence.
- **Confusion between formulation and specification.** Formulation = decide what to do. Specification = convert decision to prompt.

### What separates understanding from memorization
A reader who *understands* Ch 8 can take a recent prompt and identify which of the five elements was implicit, explicit, or absent. A reader who memorized Ch 8 can recite the elements without spotting which their prompts miss.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [TABLE: Prompt vs. specification.] -->`** Worked content:

  | Element | Weak prompt | Specification |
  |---|---|---|
  | Operation | "Write me a login function" | "Implement `login(email, password)` in `auth/login.ts`" |
  | Invariants | (implicit) | "Existing `User` model and session-management API unchanged" |
  | Context | (implicit) | "See AGENTS.md Auth-conventions; existing `auth/session.ts`" |
  | Output format | (implicit) | "New file `auth/login.ts`; tests in `tests/auth/login.test.ts`" |
  | Negative constraint | (implicit) | "No changes outside `auth/`; no new dependencies" |

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **Specification templates as a reference card.** Author may want to produce a printable card.
- **Feedback-mechanism examples for student-scale work.** What testing infrastructure can a high school student reliably invoke? `pytest`, `jest`, `cargo test`, simple bash commands. The chapter should give a few worked examples that don't assume professional CI/CD.
- **Empirical specification-quality studies.** No published study cleanly measures the chapter's format against output quality. The claim is consistent with prompt-engineering general findings.

---

## 9. Sourcing Notes

- **Lovelace 1843** — multiple scholarly editions; cite Note G.
- **Liskov & Wing 1994** — ACM DL.
- **Wirth 1971** — open access *CACM*.
- **OpenAI Codex prompting guide** — developers.openai.com.
- **OpenAI engineer-voice quotes** — forum.openai.com Dec 2025.
