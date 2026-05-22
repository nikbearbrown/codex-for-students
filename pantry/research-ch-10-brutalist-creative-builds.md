# Research: Chapter 10 — Brutalist: When the Build Is Creative
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** The technical barrier is now low enough that any student can produce ambitious creative work. The question is whether the creative judgment stays theirs.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **LeWitt, Sol. "Paragraphs on Conceptual Art." *Artforum* 5 (1967) and "Sentences on Conceptual Art." *0–9* 5 (1969).** The intellectual core. The idea is the work; execution can be delegated. Author who holds the intent and writes the instruction is the author.
- **Bear Brown, Nik. *Brutalist Design System*. brutalist.art.** The chapter's central operational reference. brutalist.art is the author's own design system — the three-file approach (AGENTS.md + DESIGN.md + PROJECT.md) is articulated there. **Author should clarify in the chapter that brutalist.art is his own work** rather than citing it as third-party authority. (Per TIKTOC the link appears in Ch 10's "Links" section.)
- **DESIGN.md ecosystem references.** designmd.app (a library of 454 design systems formatted for AI agents), getdesign.md, github.com/VoltAgent/awesome-design-md. The 2025–2026 industry shift toward design-system-as-AI-readable-spec. The brutalist three-file system is one entrant in this ecosystem; the chapter should position it.
- **McCloud, Scott. *Understanding Comics*, ch. 7 ("The Six Steps"). HarperCollins, 1993.** Six-step model of creative work (idea/purpose → form → idiom → structure → craft → surface). Codex helps at craft and surface; the student holds idea, purpose, idiom.
- **Tufte, Edward R. *The Visual Display of Quantitative Information*. Graphics Press, 1983.** Visual choices carry meaning. No neutral display.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** Useful for the chapter's tone — vendor recognition that voice and craft matter.

### Key empirical cases

- **Nicholas's observation (TIKTOC).** Classmates let AI generate prose, graphics, music. Polished output, no soul. The chapter's emotional anchor. Author should preserve Nicholas's specific testimony.
- **The student data-visualization project (TIKTOC worked example).** Same chart built twice — once with Codex running unattended on aesthetic decisions, once with the Brutalist system. Same Codex; same technical output; different authorship. Author should produce both versions.
- **The "AI-generated essay" recognition pattern.** Documented widely 2024–2026 in education research: readers can identify generated text within 1–3 paragraphs with reasonable reliability. The chapter can reference the general phenomenon without claiming a specific number.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Generated text and code tend toward statistical centrality.** Most-plausible continuation = most generic.
- **Voice is identifiable.** Stylometric analysis (2024–2026) can reliably detect generated text in many contexts.
- **Aesthetic decisions are not separable from technical ones.** Tufte; design literature; growing 2025–2026 software-design literature.

### What is disputed

- **Whether AI can capture voice with sufficient examples.** Partial. The chapter's middle position is empirically defensible.
- **Whether students should care about voice in builds.** The chapter argues yes for any build whose output another human reads.
- **The three-file system specifically.** The Brutalist system is one of several (CLAUDE.md / AGENTS.md / SKILL.md / DESIGN.md ecosystems). The book teaches the Brutalist three-file as one operational choice; other choices exist.

### What has changed recently (last 5 years)

- The 2025–2026 ecosystem of design-system-as-AI-readable-spec (designmd.app, awesome-design-md, etc.) is institutional support for the chapter's argument. The book is contributing to this conversation, not inventing it from scratch.
- Codex's improved instruction-following in 2024–2026 makes the three-file system more effective than it would have been in 2022.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 10 sits in data visualization / creative work / Nicholas's domain.)

- **Data visualization with the Brutalist three-file system.** AGENTS.md: which chart library, build tool, file conventions. DESIGN.md: color palette, typography, the project's specific aesthetic principles (e.g., "always honest about uncertainty — show confidence intervals"). PROJECT.md: the dataset, the story to tell, the audience. Codex generates the chart code; the student holds the editorial judgment.
- **A student game.** AGENTS.md: framework (Unity, Godot, PixiJS), build process, file structure. DESIGN.md: visual style (palette, sprite conventions, UI patterns). PROJECT.md: the game's intent — what the player should feel, what makes this game *yours*.
- **A personal website.** AGENTS.md: technical stack, hosting, deployment. DESIGN.md: typography, color, layout principles, the brutalist-vs-minimal-vs-maximalist stance. PROJECT.md: what the site is *for*, what the visitor takes away.

---

## 4. The Book's Thesis Connection

Ch 10 extends the thesis from technical correctness to authorial integrity. The book's thesis is capability; Ch 10 makes capability include *voice*.

The chapter's contribution:

1. **Names the fluency trap for creative work.** Editor-AI books touch this concept; the Brutalist-three-file operationalization is fresh.
2. **Operationalizes voice preservation.** The three files are concrete artifacts. The discipline scales down to a single chart, scales up to a full project.
3. **Reframes "creative" for technically fluent readers.** Most readers don't think of themselves as creative writers. Ch 10's job is to show that any output a human reads is a creative artifact.

Student-supplied capacity: voice is irreducible. Only the student knows what their voice is.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Sol LeWitt (1928–2007).** Excellent fit.

Candidates:
- **Sol LeWitt** (1928–2007, USA, conceptual artist). Named. *Paragraphs* and *Sentences on Conceptual Art*. Wall-drawing instructions. The chapter's intellectual core. Diversity: white male American.
- **Yoko Ono** (born 1933, Japan/USA, artist). *Grapefruit* (1964). Instructional artworks. **Strongest diverse alternate.**
- **Ursula K. Le Guin** (1929–2018, USA, writer). *Carrier Bag Theory of Fiction*. Substantive fit moderate; less direct than LeWitt or Ono.

Recommendation: **consider Yoko Ono.** *Grapefruit* is the cleanest precedent for "instructions anyone can execute, remain the originator's work." Diversity contribution strong.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Ch 6 (AGENTS.md). The reader is comfortable with persistent context files.

### Common misconceptions to disarm
- **"Aesthetic isn't engineering."** It is. Format and voice are operational.
- **"AI can match my voice with examples."** Partial. Examples narrow; judgment of capture is the student's.
- **"This chapter is creative writing."** No. It's about judging aesthetic fit. The substrate is creative builds.
- **"Personal projects don't need a 'creative' anything."** They do if any human reads the output.

### Effective instructional sequences
- **Same output, three versions.** Generated default, generated with DESIGN.md, hand-written. Side by side.
- **Writing the three files.** Apply-level exercise: produce AGENTS.md + DESIGN.md + PROJECT.md skeleton for a current project.
- **Intent Layer check.** Have a classmate read PROJECT.md's intent. Can they tell what you're trying to make? If yes, it's clear; if no, rewrite.

### Known failure modes
- **The art-school detour.** Frame in concrete build terms.
- **Voice-as-mystique.** Operational: format, vocabulary, rhythm, stance.
- **Over-specified DESIGN.md.** 500-word DESIGN.md the student abandons after week one. Brevity wins.
- **Brutalist as brand-evangelism.** The chapter is teaching a *system*, not selling the brutalist.art domain. The link belongs in the appendix and footer; the chapter must teach the principles independent of the marketing.

### What separates understanding from memorization
A reader who *understands* Ch 10 can take their existing project and write a one-paragraph PROJECT.md Intent Layer that another reader recognizes as theirs. A reader who memorized Ch 10 can recite the three files without producing voiced output.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: The three-file system as three nested layers.] -->`** Worked content:
  - Outer ring: AGENTS.md (technical constitution — what Codex never improvises).
  - Middle ring: DESIGN.md (visual/aesthetic constitution — every decision specified or escalated).
  - Inner ring: PROJECT.md (project state; **Intent Layer is human, always**).
  - Editorial style. Concentric or stacked.

- **`<!-- → [TABLE: Labor separation in creative builds.] -->`** Worked content:

  | Codex handles | Human keeps |
  |---|---|
  | Generating chart code from spec | Choosing what story the chart tells |
  | Rendering text in the specified voice | Specifying *what* the voice is |
  | Producing variant designs from constraints | Selecting which variant is *yours* |
  | Building UI components from design tokens | Deciding which interactions matter |
  | Generating music in a defined style | Defining the style |

---

## 8. Open Questions and Research Gaps

- **brutalist.art as the book's reference.** Author owns the system. The chapter should be honest that it's teaching the author's own framework, not borrowing a third party's. Reader trust depends on this transparency.
- **DESIGN.md ecosystem positioning.** The chapter should briefly acknowledge the broader 2025–2026 design-system-as-AI-readable-spec ecosystem (designmd.app, awesome-design-md). The book's three-file system is one entrant; positioning it within the broader conversation matters.
- **Nicholas's voice and credit (OQ-4).** TIKTOC OQ-4 names Nicholas's attribution as unresolved. The chapter quotes Nicholas's observation; the book should resolve credit before publication.
- **Voice retention across Codex versions.** Empirically untested. The DESIGN.md is a living document.

---

## 9. Sourcing Notes

- **LeWitt 1967 / 1969** — open access via Artforum / 0–9 archives.
- **Ono 1964** — Wunternaum Press original; Simon & Schuster 2000 reprint is standard.
- **McCloud 1993** — trade.
- **Tufte 1983** — Graphics Press. Tufte is exacting about printing; quote sparingly.
- **brutalist.art** — author's own. Cite as such.
- **DESIGN.md ecosystem links** — current URLs at press; verify.
