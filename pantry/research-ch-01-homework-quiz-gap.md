# Research: Chapter 1 — The Homework/Quiz Gap: What's Actually Happening
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** Students who use AI freely during practice score dramatically lower on unassisted tests — and feel like they learned more, not less.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Bastani, Hamsa; Bastani, Osbert; Sungu, Alp; Ge, Haosen; Kabakcı, Özge; Mariman, Rei. "Generative AI without guardrails can harm learning: Evidence from high school mathematics." *PNAS* 122 (2025).** ~1,000 Turkish high school students; three arms (control, GPT Base, GPT Tutor with guardrails). **GPT Tutor group performed 127% better in AI-assisted practice but scored the same as control on the unaided exam. GPT Base performed *worse* than control on the unaided exam — the precise quantum is the "17 percentage point" figure TIKTOC quotes.** Open access: <https://www.pnas.org/doi/10.1073/pnas.2422633122>. Cite Cohen's d and p-values from the paper directly — TIKTOC requires them in the chapter.
- **Kosmyna, Nataliya et al. "Your Brain on ChatGPT: Accumulation of Cognitive Debt when Using an AI Assistant for Essay Writing Task." arXiv:2506.08872, MIT Media Lab, June 2025.** 54 participants, three groups (LLM/Search/Brain-only), EEG. LLM users showed weakest brain-network connectivity (TIKTOC's "up to 55% reduction" figure traces to the paper's specific band-power comparisons — verify the 55% number against the paper's reporting before using it as cited). Crossover session: LLM users reassigned to brain-only showed weaker recall of their own essays.
- **"How AI assistance impacts the formation of coding skills." Anthropic, 2026 (arXiv:2601.20245).** RCT, 52 mostly junior engineers learning Python's Trio asynchronous library. AI-assisted group averaged 50% on 14-question conceptual quiz; hand-coding group averaged 67%. **17-percentage-point gap, Cohen's d = 0.738, p = 0.01** — this is the TIKTOC-quoted figure. Three low-scoring patterns (below 40%): complete AI delegation, progressive AI reliance, iterative AI debugging. Three high-scoring patterns (65%+): follow-up questions, code-with-explanation, AI for conceptual-only.
- **Sweller, John. "Cognitive Load During Problem Solving." *Cognitive Science* 12 (1988): 257–285.** Cognitive load theory — schemas form through effortful practice; when the load is removed, the schema does not form. The mechanism under the three foundational studies.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC quotes this for the "Start with Ask Mode questions before building" guidance. The engineer-voice paragraph anchors the chapter's practitioner-side advice in the same authority Bastani provides empirically.

### Key empirical cases

- **Two students, same final grade (TIKTOC's worked example).** One builds a database schema by working through it. One asks Codex for the schema and reads it. Six weeks later: one can design a database, one can describe what a database looks like. Concrete worked example; the chapter's emotional core.
- **The debugging-gap finding (Anthropic RCT).** The largest performance gap in Anthropic's data was on diagnostic questions — students who delegated could not reason about *why* code was broken. This finding is more terrifying than the headline 17-point gap because debugging is the irreducible capability of a working engineer. The chapter should make this finding's specific severity visible.
- **Seth's own homework/quiz audit.** Author and Seth should produce, before drafting, a specific instance from Seth's AP CS class: a problem set Seth aced with Codex's help, the matching quiz, what happened. The chapter's spine is Seth's testimony.

---

## 2. The Core Concept — State of the Field

### What is settled

- **AI-assisted task completion ≠ AI-assisted learning.** All three foundational studies converge via independent measurement modalities (exam scores, EEG, conceptual quiz). The empirical foundation is solid.
- **The mechanism is identifiable.** Anthropic's RCT named it most precisely: skill formation depends on *which interaction patterns* the user employs. Delegation patterns produce poor formation; engagement patterns produce comparable-to-control formation.
- **Codex's agentic mode amplifies the risk.** Pre-2024 Codex was completion-only. Post-2024 Codex executes multi-step tasks autonomously. The more autonomous the tool, the more the student must consciously decide what *not* to delegate.

### What is disputed

- **Are the effects permanent?** Bastani measured at one time point; Kosmyna's crossover sessions suggest at least short-term persistence; no long-run RCT.
- **Does AI literacy instruction help?** Bastani's guardrail arm shows that *interface design* helps. Whether *student education* helps is largely untested.
- **Domain transfer.** Anthropic's RCT measured Python developers; Bastani measured math students; Kosmyna measured essay writers. None measured AP CS students using Codex specifically. The book uses the closest available analogues; the transfer is reasonable but not direct.

### What has changed recently (last 5 years)

- The empirical literature on LLM-and-learning effectively did not exist in 2021. The 2025–2026 triplet is the foundation.
- Codex itself shifted from completion API to agentic system between 2021 and 2024. The book is the first practitioner book to address the agentic version for student readers.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 1 sits in AP CS / homework.)

- **AP CS A linked-list problem.** Student asks Codex for a method to detect a cycle in a singly-linked list. Codex produces Floyd's algorithm correctly. Student submits, scores well. Quiz asks for the same algorithm with one constraint changed (no extra memory). Student cannot derive the modification because they never built the mental model for *why* the two-pointer approach works.
- **AP CS Principles algorithm trace.** Student asks Codex to write a sort. Codex produces a quicksort with random pivot. Student submits, scores well. The quiz asks the student to trace the algorithm by hand on a specific input. The student cannot — they never simulated the algorithm in their head.
- **The Python project.** Student builds a "real" project for a class — a small game, a data dashboard. With Codex, completion time drops from weeks to days. The project demos beautifully. The student is asked to extend it during the demo. They freeze.

---

## 4. The Book's Thesis Connection

Ch 1 is where the thesis stops being implicit and becomes explicit. The chapter's job:

1. **Name the mechanism.** The homework/quiz gap is not a moral failing; it is a structural consequence of which cognitive operations get exercised. The chapter must make the mechanism legible.
2. **Locate the student-supplied capacity.** The studies measure what gets *lost* when delegation happens. The chapter must name what was lost: schema formation, debugging intuition, mental models, the ability to operate the domain unassisted.
3. **Foreshadow the gate.** The Anthropic RCT's high-scoring patterns *are* the conducting discipline. The chapter does not need to name Ask Mode / Code Mode (Ch 4) — it should describe Anthropic's high-scoring patterns and let Ch 4 produce the operationalization.

Student-supplied capacity: the struggle that builds the schema. There is no way to outsource the schema to Codex; the schema is the thing the chapter is trying to protect.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: William James (1842–1910).** Keep as primary.

Candidates:
- **William James** (1842–1910, USA, psychologist/philosopher). Named. *Principles of Psychology* (1890), chapter on Habit. The mechanism of consolidation James named is the mechanism the homework/quiz gap breaks. Famous-tier; substantive fit excellent.
- **K. Anders Ericsson** (1947–2020, Sweden/USA, psychologist). Deliberate practice. The chapter's claim that struggle builds capability is Ericsson's framework. Less famous; same broad diversity profile.
- **Lev Vygotsky** (1896–1934, Russia/Soviet, psychologist). Zone of proximal development. Strong pedagogical fit; less direct fit to the empirical claim. Different national/political diversity.

Recommendation: keep James. Habit consolidation is the precise mechanism the homework/quiz gap breaks. Ericsson is a fine alternate.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
None beyond Ch 0. The chapter is the empirical hard-stop for Act One.

### Common misconceptions to disarm
- **"The numbers don't apply to me — I'm a strong student."** Bastani's strong students lost more, not less. Strong students had more to lose.
- **"Codex is better than what those studies measured."** The studies measured GPT-4 and current-generation models. Codex's underlying model is current-generation. The studies apply.
- **"I would notice if I was forgetting."** Kosmyna's data say no — students who delegated did not recognize the gap when tested unassisted.
- **"If I just use Ask Mode I'm safe."** Ask Mode is necessary, not sufficient. Ch 4 onward unpacks why.

### Effective instructional sequences
- **Specific-to-general with empirical anchoring.** Open with Seth's friend. Then the Bastani number. Then the mechanism. Then the worked example. Then the audit exercise.
- **The Bastani table** (TIKTOC calls for it). Hard numbers, side by side. Cohen's d in the table is non-negotiable; the chapter is the book's empirical foundation.
- **Two-path diagram** (TIKTOC calls for it). The fluency trap. Path A: struggle → consolidation → capability. Path B: delegate → output → no consolidation → atrophy.

### Known failure modes
- **Statistic dump.** Open with Seth, not with Bastani. The numbers land harder after the anecdote.
- **Therefore-don't-use-AI.** The book is pro-Codex with discipline. The chapter must not slide into Luddism. The reader needs the alternative (Ch 4), not just the diagnosis.
- **Confusing correlation and causation.** Bastani is an RCT; the causation is established for the study population. Don't overclaim transfer.

### What separates understanding from memorization
A reader who *understands* Ch 1 can identify, in their own recent AI use, which of Anthropic's low-scoring patterns they have engaged in and name what specifically was at risk of atrophy. A reader who memorized Ch 1 can quote the 17-point gap without applying it to themselves.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [TABLE: Bastani RCT results — two columns: AI-Assisted vs. Hand-Coding group.] -->`** Worked content:

  | Metric | AI-Assisted | Hand-Coding | Gap |
  |---|---|---|---|
  | Practice score | +127% vs. control (GPT Tutor); +48% vs. control (GPT Base) | baseline | — |
  | Unassisted exam score | GPT Base: −17pp vs. control; GPT Tutor: tied with control | baseline | up to 17pp |
  | Anthropic conceptual quiz | 50% | 67% | 17pp |
  | Cohen's d (Anthropic) | — | — | 0.738 |
  | p-value (Anthropic) | — | — | 0.01 |

  Note: TIKTOC's "48% higher" and "17 percentage points lower" figures need to be verified against Bastani's exact methodology — the figures may conflate GPT Base and GPT Tutor arms. Author should verify before press.

- **`<!-- → [DIAGRAM: The fluency trap — two-path diagram.] -->`** Worked content:
  - Path A (top): `struggle → consolidation → durable capability`.
  - Path B (bottom): `delegate → fluent output → no consolidation → atrophy`.
  - Shared starting point: same task.
  - Divergent endpoints: same final grade, very different durable capacity.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **Bastani's exact numbers as quoted.** TIKTOC's "48% higher practice / 17 points lower exam" is approximately right but the arm-mapping (Tutor vs. Base) should be verified against the paper before quotation. The risk is unintentional misquotation in the book's empirical foundation.
- **Kosmyna's "up to 55%."** TIKTOC quotes this for brain-connectivity reduction. The paper's specific reporting (alpha band, theta band, network-level connectivity) should be cited precisely; "up to 55%" is a summary number that may not match a single measurement.
- **Terminal-AI applicability.** None of the three foundational studies measured AI-assisted coding in editor environments specifically. The book uses Anthropic's RCT as the closest analogue. The transfer is reasonable; acknowledge it.
- **Codex feature-version risk.** Specific Codex syntax (Ask Mode, Code Mode, Best-of-N) is in Appendix A per TIKTOC's aging-risk mitigation. Ch 1 doesn't teach these — it foreshadows them.

---

## 9. Sourcing Notes

- **Bastani PNAS 2025** — open access; cite directly. Verify the exact arm-vs-arm comparisons against the paper.
- **Kosmyna 2025** — arXiv preprint at time of writing. Verify peer-reviewed status at press. The "55%" figure needs sourcing precision.
- **Anthropic RCT 2026** — blog + arXiv. The Cohen's d and p-value come from the arXiv version, not the blog.
- **Sweller 1988** — standard cognitive-load citation, stable.
- **OpenAI "How OpenAI Engineers use Codex"** — December 4, 2025; check URL at press.
