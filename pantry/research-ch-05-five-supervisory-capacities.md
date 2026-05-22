# Research: Chapter 5 — The Five Supervisory Capacities
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** These are the five things you do that Codex cannot. Name them. Practice them. Never delegate them.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Engelbart, Douglas C. "Augmenting Human Intellect: A Conceptual Framework." Stanford Research Institute, 1962.** The conceptual ancestor. Open access: dougengelbart.org.
- **Norman, Donald A. *The Design of Everyday Things*, rev. ed. Basic Books, 2013.** Gulf of execution and gulf of evaluation. PF closes the gulf of execution; PA + IJ close the gulf of evaluation. The chapter's framework maps cleanly onto Norman's.
- **Klein, Gary. *Sources of Power: How People Make Decisions*. MIT Press, 1998.** Recognition-primed decision. PA is RPD on AI output.
- **Anthropic RCT (Ch 1).** High-scoring patterns are operations the chapter names: follow-up question = PA surfacing as a question; code+explanation = IJ in action; conceptual-only AI = TO with right scope.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, Dec 2025.** TIKTOC's PF quote: "When I'm in meetings all day, Codex works in the background — but I give it the direction first." The engineer's direction-setting is PF. The OpenAI source provides the practitioner anchor for PF specifically.

### Key empirical cases

- **The "compiles, tests pass, still wrong" Seth case.** TIKTOC's Ch 5 opening. PA catches what verification doesn't. Author should preserve a specific instance.
- **The five-capacity post-mortem of a Codex failure.** Author should produce one: a build that went wrong, decomposed by which capacity was absent at which step. Teaches by example.
- **OpenAI engineers' "well-scoped tasks would take an hour" rule.** TIKTOC quotes this for PF. The scoping decision is a five-capacity decision: PF names the task, TO chooses Ask vs. Code, IJ judges whether the scope is right, PA monitors for off-pattern signals, EI holds the project goal across multiple such tasks.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Human-machine task division has 60+ years of literature.** Fitts (1951), Sheridan (1992), and current human-factors taxonomies. The five-capacity framework is in the lineage.
- **Pattern recognition and judgment are different operations.** Established in cognitive science (Klein, Kahneman).
- **Naming improves deployment.** Vocabulary studies in expert pedagogy show that named distinctions are more reliably noticed and applied.

### What is disputed

- **Whether five is the right number.** Same as the GitHub Copilot CLI book — defensible operational choice, not a theoretical claim.
- **Stability under tool improvement.** TIKTOC's Contested Claims table acknowledges: "Currently requires human judgment." Hold lightly.
- **Distinguishability in practice.** Some readers will conflate PA with IJ. The chapter's exercises must surface and address this.

### What has changed recently (last 5 years)

- Anthropic's 2026 RCT provides empirical validation for the engagement-pattern framework. The five capacities are a finer-grained version of Anthropic's three high-scoring patterns.
- Agentic Codex with Ask/Code modes changes *where* capacities fire, not which capacities are needed. Ask Mode is where PF and PA do the most work; Code Mode is where TO, IJ, and EI dominate.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 5 sits in Git automation domain per TIKTOC for the gh-copilot book; Codex book is software function / login domain.)

For each capacity, a software-build example:

- **[PA] Plausibility Auditing.** Codex generates a `getUser(id)` function that returns the user. Tests pass. PA fires: this function doesn't check authorization — the caller could request any user's data. The output is technically correct; PA hears the wrong note.
- **[PF] Problem Formulation.** Before any prompt: am I building authentication or authorization? Login or session-management? Those are different systems. PF decides.
- **[TO] Tool Orchestration.** For "explore how auth currently works in this codebase," Ask Mode is the right tool. For "implement the change," Code Mode. For "compare three approaches," Best-of-N (where available) or manual multi-prompt. TO chooses.
- **[IJ] Interpretive Judgment.** Codex's output is correct for the general auth pattern. The student knows this codebase uses an older session model that doesn't compose with the modern pattern. IJ supplies meaning the explanation cannot.
- **[EI] Executive Integration.** Three prompts ago we established: this build is supposed to keep the existing auth API stable. This new suggestion changes the API. EI stops the build.

---

## 4. The Book's Thesis Connection

Ch 5 makes supervisory work nameable. The thesis depends on the capacities being identifiable. If the reader cannot name what they are doing when they conduct, they cannot tell whether they are doing it.

The chapter's contribution:
1. **Decomposition.** "Judgment" is too vague. Five capacities are five practicable operations.
2. **Diagnostic vocabulary.** When a build goes wrong, the reader can ask: which capacity was absent?
3. **Stability.** Codex changes; capacities are tool-agnostic.

Student-supplied capacity: all five require knowledge the model doesn't have. None transfers.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Douglas Engelbart (1925–2013).** Perfect fit. Keep.

Candidates:
- **Douglas Engelbart** (1925–2013, USA, computer engineer). Named. *Augmenting Human Intellect*. Famous for the mouse and Mother of All Demos, lesser-known for the conceptual framework. Substantive fit overwhelming. Diversity: white male American.
- **Lucy Suchman** (born 1951, USA, anthropologist of work and technology). *Plans and Situated Actions* (1987). Argument that work is situated — context-dependent in ways plans don't capture. **Strongest diverse alternate.** Woman, American, ongoing scholar.
- **Vannevar Bush** (1890–1974, USA, engineer). "As We May Think" (1945). Augmentation, Memex. Substantive fit moderate.

Recommendation: keep Engelbart. Suchman is the best swap-target if diversity rebalancing across the full set demands it here.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 4's gate. The reader is already running Ask → review → Code in some form.

### Common misconceptions to disarm
- **"Five things on every prompt."** No. Each capacity fires at a different moment. PF dominates at problem-setting; PA fires at output review; TO at tool choice; IJ during plan-reading; EI throughout.
- **"Conscious performance every time."** They fuse into single conducting practice. Early on, naming helps; later, names recede.
- **"PA = paranoia."** PA is hearing the wrong note. Vigilance, not anxiety. Distinguish.

### Effective instructional sequences
- **Define by example.** TIKTOC's structure — short definition then specific moment. Follow this.
- **Trace through a build.** Apply-level exercise — label each step's primary capacity.
- **Diagnostic exercise.** Given a build that went wrong, name which capacity was absent.

### Known failure modes
- **The chapter as glossary.** Lead with action.
- **Over-specification.** Capacities are usefully fuzzy. Don't give brittle decision rules.
- **PA-as-paranoia.** Already addressed.

### What separates understanding from memorization
A reader who *understands* Ch 5 can read an unfamiliar Codex transcript and label which capacity is exercised at each step. A reader who memorized Ch 5 can recite five names without applying them.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: Five supervisory capacities as a five-column layout.] -->`** Five columns, each: abbrev / plain name / one-sentence terminal-specific definition. Already specified.
- **`<!-- → [TABLE: Five supervisory capacities — label, name, what it catches, example failure when absent.] -->`** Worked content:

  | Label | Plain name | What it catches | Failure mode when absent |
  |---|---|---|---|
  | PA | Plausibility Auditing | Output that's technically correct but feels wrong | Ship code that compiles, passes tests, fails in production |
  | PF | Problem Formulation | Building the wrong thing | Hours into a build, realize the task was misframed |
  | TO | Tool Orchestration | Wrong tool for the moment | Use Code Mode when Ask Mode should have run first |
  | IJ | Interpretive Judgment | Generic output missing project-specific meaning | Accept Codex's pattern that doesn't fit this codebase |
  | EI | Executive Integration | Drift across multiple prompts | Final build undoes a constraint set three prompts ago |

---

## 8. Open Questions and Research Gaps

- **Empirical separability.** Same as gh-copilot book: synthesis, not measurement. Acknowledge.
- **Agentic-mode capacity firing.** As Codex acquires more autonomy (autopilot modes), some capacity exercise shifts to *plan-approval* rather than per-step. Address in the chapter.
- **Capacity-by-moment grid.** A 5×5 grid (capacity by build moment) would be useful for a sidebar.

---

## 9. Sourcing Notes

- **Engelbart 1962** — dougengelbart.org. Open access.
- **Norman 2013** — rev. ed. standard.
- **Klein 1998** — trade; academic papers (1989, 1993) for depth.
- **Anthropic RCT** — cited Ch 1.
- **OpenAI engineer quotes** — forum.openai.com Dec 2025.
