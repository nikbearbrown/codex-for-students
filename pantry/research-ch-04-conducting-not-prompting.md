# Research: Chapter 4 — Conducting, Not Prompting: The Core Idea
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** Programming as conducting. Codex does what it's superhuman at. You do what only you can.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Simon, Herbert A. *The Sciences of the Artificial*, 3rd ed. MIT Press, 1996.** Bounded rationality: real decisions made within real cognitive limits by designing systems that extend those limits. The Ask Mode / Code Mode gate is bounded rationality applied to AI-assisted coding.
- **Reason, James. *Human Error*. Cambridge University Press, 1990.** Swiss-cheese model: accidents happen when defenses align. The Ask Mode plan-review step is a defense layer between specification and execution.
- **Endsley, Mica R. "Toward a Theory of Situation Awareness in Dynamic Systems." *Human Factors* 37 (1995): 32–64.** Perception → comprehension → projection. Ask Mode builds perception and comprehension before Code Mode executes. Without the gate, the student perceives the output but does not comprehend it.
- **OpenAI. "Custom instructions with AGENTS.md – Codex." developers.openai.com/codex/guides/agents-md.** Official documentation for Ask vs. Code modes (the "Ask" and "Code" buttons in the Codex ChatGPT sidebar). The chapter cites this for tool semantics.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC quotes: "For large changes, start by prompting Codex for an implementation plan using Ask Mode." The vendor's own prescription matches the chapter's argument.
- **Anthropic RCT (Ch 1).** High-scoring patterns — follow-up questions, code+explanation, conceptual-only AI. The Ask Mode → Code Mode gate is the operational version of these patterns.

### Key empirical cases

- **Same task, two outcomes (TIKTOC's worked example).** "Write me a login function" directly in Code Mode vs. Ask Mode plan review then Code Mode specification. Same Codex. Different results. The chapter's spine.
- **The "Codex re-reads the same files without progress" pattern.** TIKTOC quotes from OpenAI: "Avoid excessive looping or repetition; if you find Codex re-reading or re-editing the same files without clear progress, stop and reframe." Recurring failure mode worth a specific worked example.
- **OpenAI engineer's "background work" pattern.** TIKTOC quotes: "When I'm in meetings all day, Codex works in the background — but I give it the direction first." Useful counterpoint: the engineer is conducting at scale; the discipline is the same.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Verification layers reduce error rates in high-stakes systems.** Established in human-factors literature (Reason, Endsley) and operational practice. The Ask Mode / Code Mode gate is defense-in-depth applied to AI-assisted coding.
- **Plan-then-execute is industry standard.** The Codex Ask/Code split is OpenAI's institutionalization of what experienced developers were already doing manually. The chapter is teaching the discipline the tool now formalizes.
- **Best-of-N sampling improves output quality for ambiguous tasks.** Established in LLM literature. Whether Best-of-N is a *user-facing feature* in Codex as of 2026 needs verification — see Section 8.

### What is disputed

- **Whether the Ask/Code gate is sustainable at developer pace.** Some practitioners argue the gate adds friction. Anthropic's RCT data suggests engagement patterns produce comparable speed with better skill retention. The book takes the long-run position; defensible.
- **Whether autopilot-style execution is acceptable for student work.** Codex supports more autonomous execution modes. The book argues against pure autopilot for student builds because the discipline that builds capability is exactly the per-step review. Defensible.

### What has changed recently (last 5 years)

- The Ask/Code split is a 2024–2026 vendor formalization. Pre-2024, the discipline was manual ("write a plan first, then code"). The chapter teaches the discipline that the tool now supports natively.
- Industry adoption of plan-mode-by-default in destructive operations (2024–2026) is institutional support for the chapter's argument.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 4 sits in software function / login system.)

- **Login function done twice.** TIKTOC's worked example. First: "write me a login function" in Code Mode. Codex produces generic auth code. Second: Ask Mode interrogates the codebase ("what auth library is already in use, what are the session conventions, where do passwords get stored"), produces a plan; Code Mode executes against the plan; output matches the project.
- **Database migration.** First: "add a `last_login_at` column to users." Codex's generic migration may or may not match the project's migration framework. Second: Ask Mode finds the migration tool, names the convention, generates the migration in that convention.
- **API endpoint addition.** First: "add a GET /users/{id} endpoint." Codex's pattern may not match the project's auth, routing, or serialization conventions. Second: Ask Mode plan first; Code Mode generates against project conventions.

---

## 4. The Book's Thesis Connection

Ch 4 is where the thesis becomes operational. The chapter must:

1. **State the gate clearly.** Ask Mode → review → Code Mode. Three operations, every time, for any non-trivial build step. The chapter's value depends on this being memorable.
2. **Frame conducting.** The orchestra metaphor. The orchestra (Codex) plays exactly what they understood you to mean. The gap between meaning and understanding is where things break.
3. **Defend the gate against the speed objection.** Anthropic RCT is the empirical answer; Seth's voice is the persuasive answer. "I tried skipping the Ask Mode step on a small change. The change broke three other things I didn't know it touched."

The five capacities get named in Ch 5; the gate is where they first operate together.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Herbert Simon (1916–2001).** Strong fit. Keep as primary.

Candidates:
- **Herbert Simon** (1916–2001, USA, polymath). Named. Nobel laureate. Bounded rationality. Substantive fit excellent. Diversity: white male American.
- **J. C. R. Licklider** (1915–1990, USA, psychologist/CS). "Man-Computer Symbiosis" (1960). The gate is exactly Licklider's symbiosis. Same diversity profile.
- **Donald Schön** (1930–1997, USA, philosopher/planner). *The Reflective Practitioner*. Reflection-in-action between Ask and Code. Less famous; same diversity profile.

Recommendation: keep Simon. Bounded rationality is the most precise fit. If diversity rebalancing demands a swap, Schön works but creates redundancy with Ch 7 (Brooks).

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 1–3 problem fully felt. The reader must want the alternative.

### Common misconceptions to disarm
- **"Ask Mode is for newbies."** No. Senior OpenAI engineers prescribe Ask-Mode-first for large changes. The discipline is for everyone.
- **"I can keep Ask and Code in my head."** Sometimes for trivial tasks. For anything non-trivial, the gate's *artifact* (a reviewed plan) is the point. In your head ≠ in writing.
- **"Best-of-N is the gate."** No. Best-of-N (where available) is a verification tool. The Ask/Code gate is a structural discipline that operates regardless of Best-of-N availability.

### Effective instructional sequences
- **Same task, two paths.** TIKTOC's central pattern. The chapter must execute this crisply.
- **Decompose the gate.** Ask Mode is one operation. Plan review is another. Code Mode is a third. Each independently mastered before composition.
- **Show the rejected output.** TIKTOC's worked example should include a moment where the student rejects an output and respecifies. Concrete; learnable.

### Known failure modes
- **The gate as ceremony.** If the gate becomes ritual without comprehension, the discipline collapses. Plan review is *read carefully*, not skimmed.
- **The "but the OpenAI engineers do it autopilot" objection.** The blog post mentions autopilot for well-scoped tasks. The chapter must distinguish: well-scoped means the engineer has already done the supervisory work upstream. Student builds typically don't have that upstream specification, so autopilot is more risky.
- **The orchestra metaphor over-extended.** One reference. Don't write the chapter in conducting vocabulary.

### What separates understanding from memorization
A reader who *understands* Ch 4 can describe a recent Codex prompt and walk through what Ask Mode would have surfaced before Code Mode ran. A reader who memorized Ch 4 can list "Ask, review, Code" without producing an application.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The Ask Mode / Code Mode gate.] -->`** Worked content:

  Vertical flow:
  - Human in Ask Mode: interrogate, plan, formulate.
  - Gate: plan reviewed and approved (visible affordance).
  - Human in Code Mode: execute against plan, verify output.
  - Constraint band: "No Code Mode execution until plan reviewed."

  Editorial style. Print-bookmarkable.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **Best-of-N feature status.** TIKTOC's OQ-6: "Best-of-N feature availability on student-accessible plans? Confirm before Chapter 9." As of May 2026, Best-of-N is not a named user-facing button in Codex; the API supports `n` parameter for parallel completions. The book may need to reframe Best-of-N as a *technique* (generate-multiple-and-select, possibly via the API or manual re-prompting) rather than a UI feature. Ch 4 only foreshadows; Ch 9 owns the resolution.
- **Codex feature stability.** TIKTOC's high-risk-aging item. The Ask/Code split is OpenAI's current UX; could be renamed. The discipline survives any renaming. Author should freeze the chapter's language at press time and plan a 2-year edition cadence.
- **Student access to Codex.** OQ-2. ChatGPT Plus ($20/mo) and ChatGPT Edu provide Codex access. Free tier has limited Codex. Author may want to add a sidebar in Ch 4 about which plan is needed to follow along.

---

## 9. Sourcing Notes

- **Simon 1996** — 3rd edition.
- **Reason 1990** — canonical.
- **Endsley 1995** — paywalled in some archives; preprints exist.
- **OpenAI Codex docs** — developers.openai.com/codex; cite the URL current at press.
- **OpenAI engineer-voice quotes** — forum.openai.com Dec 4 2025; verify URL.
