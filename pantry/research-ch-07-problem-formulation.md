# Research: Chapter 7 — Problem Formulation: The Mission Before the Build
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** The most expensive mistake in an AI-assisted build happens before the first prompt. Formulate the problem first.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Brooks, Frederick P. "No Silver Bullet: Essence and Accidents of Software Engineering." *IEEE Computer* 20, no. 4 (1987): 10–19.** Essential difficulty (what to build) vs. accidental difficulty (how). Codex reduces accidental difficulty. Problem formulation IS essential difficulty.
- **Brooks, Frederick P. *The Design of Design: Essays from a Computer Scientist*. Addison-Wesley, 2010.** Design as formulation.
- **Polya, George. *How to Solve It*. Princeton University Press, 1945.** Four-stage model: understand → plan → execute → look back. Problem formulation is stage 1.
- **Simon, Herbert A. *The Sciences of the Artificial*, ch. 5 ("The Science of Design"). MIT Press, 1996.** Design as problem-formulation under uncertainty.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC's Ch 7 quote: "When kicking off a new feature, engineers use Codex to scaffold boilerplate — but first they specify what they want." Vendor authority for the "specify first" prescription.
- **OpenAI. "Best practices – Codex." developers.openai.com/codex/learn/best-practices.** OpenAI's own published guidance on prompt structure and problem formulation. Useful reference for the chapter's authoritative anchor.

### Key empirical cases

- **Seth's "three hours in, realized I was building the wrong system" moment (TIKTOC's Ch 7 opening).** Real instance required. The chapter's emotional anchor.
- **Mars Climate Orbiter loss (1999).** Formulation failure at NASA scale. Different teams used different units; software ran correctly in both; the *problem* of unit-consistency was not explicitly formulated. NASA's MCO Mishap Investigation Board report is open and public.
- **The "feature creep before code" pattern.** Recurring failure: a build that starts as a small change and accretes scope before specification is written. Documented in software-engineering literature (Boehm, Standish CHAOS reports).

---

## 2. The Core Concept — State of the Field

### What is settled

- **Specification work is the largest source of cost overruns.** Decades of software-engineering data.
- **Codex cannot transfer intent.** Structural property.
- **Ask Mode interrogation improves output quality.** OpenAI's own guidance and Anthropic's RCT both support this.

### What is disputed

- **Pre-formulate completely vs. formulate-as-you-go.** The book's hybrid stance — one-sentence formulation before any prompt, refined during — is the practitioner consensus.
- **Detail level for first-build formulation.** TIKTOC's "minimum viable spec" with five sections (problem, architecture principles, user flows, user needs, etc.) is more elaborate than some practitioners use. The book's choice is defensible but more verbose than a one-sentence formulation alone.
- **Ask Mode as formulation tool.** The book teaches Ask Mode for interrogation. Some practitioners do this alone, on paper. Both work; Ask Mode is faster.

### What has changed recently (last 5 years)

- Codex's Ask Mode formalizes what experienced engineers were doing manually (write a plan first). The chapter teaches the discipline the tool now supports.
- The 2025–2026 generation of Codex with plan-mode output makes Ask-Mode formulation more reliable than 2023 prompt-engineering attempts.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 7 sits in task tracker / student project.)

- **"Build me a task tracker" — formulated badly.** What kind? CLI tool, web app, mobile? Who uses it (just the student, a team, friends)? What's the data model (flat list, hierarchical, tagged)? Persistence (file, SQLite, cloud)? The student who runs `gh copilot suggest "build a task tracker"` (or its Codex equivalent) gets a generic answer. The student who formulates first gets a tool tailored to their actual workflow.
- **"Add a feature to my project" — formulated badly.** Which feature? What's the interaction with existing features? What's the migration path? Ask Mode interrogation surfaces these.
- **"Fix the bug" — formulated badly.** Which bug? Reproduce-steps? Acceptable fix scope (point fix or refactor)? Formulation forces these.

---

## 4. The Book's Thesis Connection

Ch 7 is where the supervisory discipline shifts from per-command to per-build. Most silent failures are formulation failures, not execution failures.

The chapter's contribution:

1. **Most silent failures are formulation failures.** A weak prompt produces code matching the weak prompt — which compiles, runs, does the wrong thing. The fix is upstream.
2. **Formulation is irreducibly the student's work.** Ask Mode can probe; intent is the student's.
3. **The cost of bad formulation compounds.** A weak formulation produces a weak prompt → produces wrong code → student debugs the *code* when the *formulation* was wrong.

Student-supplied capacity: intent. Only the student knows what they actually want.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Frederick Brooks (1931–2022).** Excellent fit. Keep.

Candidates:
- **Frederick P. Brooks Jr.** (1931–2022, USA, computer scientist). Named. *Mythical Man-Month*, "No Silver Bullet." Substantive fit overwhelming. Diversity: white male American.
- **Christopher Strachey** (1916–1975, UK, CS). Early specification-language work. Less famous. Diversity: white male British.
- **Adele Goldberg** (born 1945, USA, CS). Co-developer of Smalltalk. Specification as design. **Strongest diverse alternate.** Woman, American.

Recommendation: keep Brooks. Goldberg is fine if rebalancing demands.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 6's AGENTS.md. The reader is writing down project context.

### Common misconceptions to disarm
- **"I'll figure it out as I go."** Works for tiny tasks. Fails for anything multi-step.
- **"My prompt was clear; Codex got it wrong."** Usually the prompt was clear to the student, ambiguous to the model. Formulation converts.
- **"Ask Mode will plan it for me."** Ask Mode probes; the student decides.

### Effective instructional sequences
- **Bad → better → best.** TIKTOC's pattern. Same task, three formulations.
- **The Ask Mode interrogation transcript.** Show one in full — questions, model's clarifications, student's revisions.
- **One-sentence test.** TIKTOC names it: what does this script do, what does it touch, what must it never touch. Apply to reader-supplied task.

### Known failure modes
- **Formulation as bureaucracy.** Frame as time-saver. Seth's "three hours in" moment is the persuader.
- **Over-formulation.** Detail in proportion to consequence.
- **Formulation theater.** Writing the form without making the decisions.

### What separates understanding from memorization
A reader who *understands* Ch 7 can use Ask Mode to surface two or three decisions they had not consciously made. A reader who memorized Ch 7 can write a one-sentence formulation that follows the template without making the decisions it encodes.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: The problem formulation gate.] -->`** Vertical gate. Below: prompts allowed. Above: nothing. At the gate: one-sentence formulation. Editorial style.
- **`<!-- → [TABLE: Weak vs. strong problem statements.] -->`** Five examples:

  | Weak | Strong |
  |---|---|
  | "Improve the app" | "Reduce task-tracker landing-page load time below 200ms on mobile, without changing the data model" |
  | "Fix the bug" | "When a user submits an empty form, show a validation message instead of a 500 error; do not change submission flow" |
  | "Add tests" | "Add unit tests for `parseDate()` covering the four edge cases listed in `docs/edge-cases.md`" |
  | "Refactor login" | "Extract password-hashing from `authenticate()` into a standalone module `hash.ts` with the existing API surface unchanged" |
  | "Make it better" | "Reduce the number of duplicate API calls on `GET /users/{id}` so a single page-load fires at most one call per user" |

---

## 8. Open Questions and Research Gaps

- **Ask Mode post-2026.** Stable as of writing; subject to renaming. Discipline survives.
- **Minimum-viable spec format.** TIKTOC specifies five sections; some practitioners use fewer. Author may want to test the spec format with student readers before finalizing.
- **Empirical formulation-quality studies for Codex.** None published. The chapter's claim consistent with prompt-engineering general findings; not directly measured.

---

## 9. Sourcing Notes

- **Brooks 1987 / 2010** — IEEE / Addison-Wesley.
- **Polya 1945** — Princeton classic.
- **Simon 1996** — 3rd ed.
- **OpenAI engineer-voice quotes** — forum.openai.com Dec 2025.
- **OpenAI best practices** — developers.openai.com/codex/learn/best-practices.
- **NASA MCO report** — ntrs.nasa.gov.
