# Research: Chapter 11 — Planning Your First Conducted Build
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** Before Codex enters Code Mode, you know exactly what you are building, why, and which steps belong to you.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Alexander, Christopher. *Notes on the Synthesis of Form*. Harvard University Press, 1964.** Design begins with explicit "fit" between form and context.
- **Alexander, Christopher et al. *A Pattern Language*. Oxford, 1977.** Patterns as reusable solutions.
- **Brooks, Frederick P. *The Mythical Man-Month*, ch. 5. Addison-Wesley, 1995.** Second-system effect: planners with confidence over-engineer. First-build plans should be appropriately scoped.
- **Beck, Kent. *Extreme Programming Explained*, 2nd ed. Addison-Wesley, 2004.** Planning game: smallest viable units of work, ordered.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** Multiple Ch 11 quotes — engineers planning with Ask Mode before any Code Mode work; the task queue used to capture parallel ideas.

### Key empirical cases

- **Seth's full planning artifact (TIKTOC worked example).** One-sentence formulation, abbreviated spec, Ask Mode plan for Phase 1. Author should produce this.
- **The OpenAI Sora-Android planning case.** From the public OpenAI blog: 4 engineers, 18 days, planned the port via Ask Mode interrogation of both iOS and Android codebases simultaneously. Anchor case for the chapter's "planning at scale" principle.
- **Standish CHAOS reports (1994–present).** Decades of evidence linking planning quality to project success.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Plans reduce error rates.** Decades of evidence.
- **Planning quality is bimodal.** Some plan > no plan; the gap is what matters.
- **Planning is a learnable skill.** The chapter assumes this.

### What is disputed

- **Waterfall vs. iterative.** The book recommends minimum upfront plan + refinement. Practitioner consensus.
- **Plan as document vs. plan as conversation.** Book recommends written. Solo readers; plan also informs AGENTS.md.
- **Ask Mode plan vs. student plan.** Some delegate planning to Ask Mode entirely. The book argues: student plans first, then Ask Mode plans, then reconciles. Ask Mode plan is a check, not a replacement.

### What has changed recently (last 5 years)

- Codex Ask Mode formalizes plan-mode in the tool. The chapter teaches the discipline the tool now supports.
- Anthropic's 2025–2026 research suggests pre-plan-then-AI-engage produces best builds. Hybrid stance is empirically supported.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 11 sits in task tracker / student project.)

- **A task tracker first-build plan.** One-sentence: "A CLI task tracker for one user on macOS that stores tasks in JSON and supports add/list/done/remove." Spec sections: data model, user flows, error handling, success criteria. Ask Mode interrogation: "What would the file structure look like? What are the failure modes I'm not seeing?"
- **A homework dashboard.** One-sentence: "A static HTML page that summarizes my open assignments from a local markdown file." Spec sections: data input format, layout decisions, build process, deployment. Ask Mode plan: structure, dependencies, testing approach.
- **A study-flashcard tool.** One-sentence: "A simple flashcard app that picks the next card using spaced repetition based on my answers." Spec sections: algorithm, data persistence, UI minimal. Ask Mode plan: which spaced-repetition algorithm, how to handle "new card" vs "review card," error states.

---

## 4. The Book's Thesis Connection

Ch 11 shifts the reader from "framework learned" to "framework applied." Planning is the first chapter where the student leads.

The chapter's contribution:

1. **Plans operationalize the capacities.** Problem statement → PF. Scope → IJ. Order → EI. Failure-handling → PA. Tool choices → TO.
2. **Plans bridge formulation (Ch 7) and execution (Ch 12).** Formulation produced a sentence; planning expands to a sequence.
3. **First plans are practice.** Frame as skill that improves with repetition.

Student-supplied capacity: every plan decision requires knowledge the model lacks.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Christopher Alexander (1936–2022).** Strong fit. Keep.

Candidates:
- **Christopher Alexander** — named. Diversity: Austrian-born, multinational, white male.
- **Phyllis Pearsall** (1906–1996, UK, cartographer). *A–Z London Atlas* (1936). Less famous; moderate substantive fit.
- **Maria Telkes** (1900–1995, Hungary/USA, scientist). Solar-energy engineer. Moderate fit; better diversity.

Recommendation: keep Alexander. *Notes on the Synthesis of Form* is the cleanest fit.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Full Act Two framework. Ch 11 composes everything.

### Common misconceptions to disarm
- **"Planning is for big builds."** First builds especially benefit.
- **"My plan should be exhaustive."** Minimum viable. Detail in proportion to consequence.
- **"I'll plan as I go."** Counter directly. Planning during execution mixes work types.
- **"Ask Mode will plan it for me."** It can. The student plans first; Ask Mode checks.

### Effective instructional sequences
- **A full worked plan, in Seth's voice.** TIKTOC: Seth's complete planning artifact. The chapter's spine.
- **Minimum-viable-plan template.** Six elements: one-sentence task, success criterion, step list, handoff conditions per step, tool choices per step, failure recovery.
- **Ask Mode reconciliation.** Show Seth asking, then diffing his plan vs. Ask Mode's, then deciding.

### Known failure modes
- **The plan as ritual.** Decision-making over form-filling.
- **Over-planning a first build.** One page max.
- **Surrender to Ask Mode.** Defend student-plans-first.

### What separates understanding from memorization
A reader who *understands* Ch 11 can produce a one-page plan for a new task that names success criteria, scopes work, and identifies riskiest steps. A reader who memorized Ch 11 can fill the template without producing decisions worth executing.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: The planning sequence.] -->`** Phase gates: Ask Mode interrogation → problem formulation → spec → Ask Mode plan → review/approve → Code Mode. Editorial style.
- **`<!-- → [TABLE: Ask Mode plan evaluation.] -->`** Worked content:

  | Plan element | Strong looks like | Weak looks like | What to do when weak |
  |---|---|---|---|
  | Task list | Specific steps, ordered by dependency | Vague phases | Decompose to concrete tasks |
  | Per-step output | Each step names what done means | "Step N completes when previous done" | Write handoff condition for each |
  | Touched files | Explicit file paths | "Wherever needed" | Re-run Ask Mode with codebase context |
  | Risks | Named risks per step | None mentioned | Run Ask Mode "what could go wrong" pass |
  | Dependencies | Step ordering with rationale | Linear list | Identify which steps can parallelize, which must serialize |

---

## 8. Open Questions and Research Gaps

- **Seth's specific build (OQ-1).** Most acute at Ch 11. Author + Seth must lock.
- **Plan-mode interaction.** Codex's plan output: how well does it match a student-pre-written plan? Author should test before drafting Ch 11.
- **Empirical first-plan studies.** None published.

---

## 9. Sourcing Notes

- **Alexander 1964 / 1977** — Harvard / Oxford standard editions.
- **Brooks 1995** — anniversary ed.
- **Beck 2004** — Addison-Wesley.
- **OpenAI engineer-voice quotes** — forum.openai.com Dec 2025.
- **Standish CHAOS** — proprietary; cite recent summary.
