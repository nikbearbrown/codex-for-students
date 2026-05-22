# Research: Chapter 6 — AGENTS.md: Your Coding Constitution
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** AGENTS.md is the file Codex reads at the start of every session. It is the difference between a Codex that knows your project and a Codex that guesses.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **OpenAI. "Custom instructions with AGENTS.md – Codex." developers.openai.com/codex/guides/agents-md.** The canonical reference. Documents the hierarchical discovery: `~/.codex/AGENTS.md` (or `CODEX_HOME`) → project root → subdirectories. Codex reads `AGENTS.override.md` first if present, then `AGENTS.md`, then fallback names. Cite this URL directly in the chapter.
- **OpenAI Codex repo. github.com/openai/codex/blob/main/AGENTS.md.** OpenAI's own AGENTS.md, used as a worked example by the broader community. Useful as the chapter's "production example" to contrast with student-scale versions.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC's AGENTS.md quote: "Maintain an AGENTS.md to help Codex operate more effectively in your repo across prompts." Vendor authority for the chapter's prescription.
- **Knuth, Donald E. "Literate Programming." *The Computer Journal* 27, no. 2 (1984): 97–111.** Intellectual lineage. AGENTS.md inverts the literate-programming relationship: human-facing context that makes machine output match intent.
- **Anthropic. CLAUDE.md documentation (claude.com).** Comparable system. Useful for the chapter's framing of AGENTS.md within a broader convention (the AI-context-file genre).
- **DESIGN.md ecosystem references.** designmd.app, getdesign.md, github.com/VoltAgent/awesome-design-md. The broader 2025–2026 ecosystem of per-project AI-context files. Useful for Ch 10's three-file system but the foundations land in Ch 6.

### Key empirical cases

- **Seth's first AGENTS.md (TIKTOC's worked example).** Five entries. Next session's Codex uses conventions without being told. Author should preserve the actual file.
- **OpenAI's own AGENTS.md.** Open repo. Useful as the "what production looks like" reference.
- **The "Codex makes the same mistake twice" pattern.** TIKTOC says: if Codex repeats a mistake, add the fix to AGENTS.md. Recurring failure mode and the discipline that addresses it.

---

## 2. The Core Concept — State of the Field

### What is settled

- **AGENTS.md is loaded automatically.** OpenAI's documentation is explicit: Codex reads the instruction chain at session start, once per run. The chapter's pedagogical opportunity is *different* from the gh-copilot book's CLI.md — here, automatic injection is the default, and the chapter must teach the student to *write* the file rather than (also) to remember to paste it.
- **Hierarchical discovery is the spec.** Global → project root → subdirectories. Each layer can override. Project-specific instructions take precedence.
- **Bloated AGENTS.md degrades Codex.** Established in OpenAI's own guidance. The chapter's "keep it tight" rule has vendor support.

### What is disputed

- **Whether AGENTS.md should be checked into version control.** Pro: project conventions become versioned. Con: lessons-learned may include sensitive details. Recommendation: yes, with a `[private]` section that is git-ignored.
- **The right granularity of subdirectory AGENTS.md files.** Some teams use one project-root file; some use per-component files. The book recommends one project-root file for student-scale work.
- **AGENTS.md vs. Codex Skills.** OpenAI introduced "Agent Skills" in 2025–2026 (developers.openai.com/codex/skills) as a more structured way to give Codex reusable capabilities. Skills and AGENTS.md are complementary: Skills are reusable across projects, AGENTS.md is project-specific. The chapter can mention Skills briefly without teaching them.

### What has changed recently (last 5 years)

- The 2024–2026 wave of AI-context files (CLAUDE.md, AGENTS.md, DESIGN.md, SKILL.md) reflects industry recognition that automatic context injection beats per-prompt context insertion. The chapter is in this lineage.
- OpenAI's hierarchical discovery (override files, global+project+subdirectory) is more sophisticated than earlier single-file approaches. The chapter should reflect this.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 6 isn't tied to one domain.)

- **A homework-project AGENTS.md.** Five entries: language (Python 3.11), runner (`python -m pytest`), code style (Black formatting, type hints required), file conventions (`src/` for code, `tests/` for tests), and the project's "never do" rules (don't add new dependencies without asking, don't modify the README).
- **A creative-coding project AGENTS.md.** Different five entries: framework (p5.js), file structure (one sketch per file), naming conventions (camelCase functions, PascalCase classes), build process (no build step — files run in browser), and the project's voice rules (comments are sparse, names are descriptive).
- **A subdirectory override.** A student working in the `tests/` directory benefits from a `tests/AGENTS.md` that overrides: "in this directory, Codex should never edit production code in `src/`; only add or modify test files."

---

## 4. The Book's Thesis Connection

Ch 6 is where the conducting discipline becomes durable. The gate (Ch 4) is per-session; the capacities (Ch 5) are per-decision; AGENTS.md persists across all sessions.

The chapter's contribution:

1. **The artifact is the discipline made visible.** A maintained AGENTS.md is evidence of supervisory work. Without it, the discipline lives only in the student's head and disappears between sessions.
2. **Automatic injection is the feature.** Unlike CLI.md (gh-copilot book), AGENTS.md is auto-loaded. The pedagogical opportunity is different: the chapter teaches *what to write*, not *when to paste*. The discipline is upstream (good content) rather than per-invocation (remember to include).
3. **Lessons-learned accumulation.** A `## Lessons learned` section that grows over time is the mechanism by which one mistake becomes durable knowledge.

Student-supplied capacity: the student is the only person who knows their project's conventions, exclusions, and prior mistakes. AGENTS.md is the form that knowledge takes.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Donald Knuth (born 1938).** Strong fit.

Candidates:
- **Donald Knuth** (born 1938, USA, computer scientist). Named. Literate programming. Famous-tier. Substantive fit excellent. Diversity: white male American.
- **Margaret Hamilton** (born 1936, USA, software engineer). Apollo flight-software documentation discipline. **Strongest diverse alternate.** Woman, American, mid-20th-century.
- **Ward Cunningham** (born 1949, USA, programmer). Invented the wiki. Substantive fit moderate.

Recommendation: **consider swapping to Margaret Hamilton.** Hamilton's documentation-as-discipline at Apollo is at least as strong a fit as Knuth's literate programming, and helps the diversity spread. Knuth remains excellent; Hamilton is excellent and more under-cited.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The reader has done a few Codex sessions and felt the cost of re-establishing context.

### Common misconceptions to disarm
- **"AGENTS.md is documentation for Codex to read."** True — but the writing is *for the student* (clarifies their own intent). The student writes it, then both student and Codex benefit.
- **"More content is better."** No. The five-element format is the recommendation. A 50-line AGENTS.md the student maintains beats a 500-line file abandoned after week one.
- **"Start AGENTS.md when the project is bigger."** No. Start at minute one.
- **"AGENTS.md is project-only — global is overkill."** No. Many students benefit from `~/.codex/AGENTS.md` with personal preferences (their coding style, preferred test framework, "always use type hints").

### Effective instructional sequences
- **Write AGENTS.md, then use it, in one session.** TIKTOC's central exercise. Concrete and immediate.
- **OpenAI's own AGENTS.md as worked example.** Show the production version; show the student-scale version. The contrast teaches scale-down.
- **Lessons-learned by example.** Three entries; each is one date, one mistake, one fix.

### Known failure modes
- **AGENTS.md as homework.** Frame as upstream investment that pays off every subsequent session.
- **Over-documented AGENTS.md.** Brevity wins. Show short examples.
- **Never-updated AGENTS.md.** Update cadence matters. End-of-session two-minute review.

### What separates understanding from memorization
A reader who *understands* Ch 6 can write an AGENTS.md for a new project, explain why each entry is there, and identify two entries that would become candidates for moving to global (`~/.codex/AGENTS.md`). A reader who memorized Ch 6 can recite the five-element format without producing entries that match their actual project.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [DIAGRAM: AGENTS.md in the session context.] -->`** Worked content:
  - Top: session start.
  - Below: AGENTS.md loaded automatically (hierarchical: global → project → subdir).
  - Below: every prompt operates with AGENTS.md in context.
  - Bottom contrast row: without AGENTS.md (Codex guesses) vs. with AGENTS.md (Codex knows).

- **`<!-- → [TABLE: AGENTS.md include/exclude.] -->`** Worked content:

  | Include | Exclude |
  |---|---|
  | Bash commands Codex can't guess | What Codex can figure out from code |
  | Code style deviations from the norm | Standard conventions |
  | Test runners and how to invoke them | Constantly changing state |
  | Architectural decisions | Personal notes unrelated to coding |
  | Environment quirks and lessons learned | Secrets, API keys, credentials |

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **AGENTS.md vs. AGENTS.override.md positioning.** TIKTOC's Out-of-Scope list excludes "Advanced agent pipelines / AGENTS.override.md." But the override mechanism is documented and useful — students who need to temporarily change rules for a specific task should know it exists. Recommendation: brief sidebar mention with a pointer to OpenAI docs, not a teaching treatment.
- **Codex Skills.** OpenAI's Skills feature (developers.openai.com/codex/skills) is the reusable-capability complement to project-specific AGENTS.md. Author should decide whether to mention Skills in Ch 6 (sidebar) or defer entirely. Recommendation: one-paragraph sidebar; full treatment would expand the book.
- **AGENTS.md across editions.** As Codex evolves, AGENTS.md syntax may extend. Author should plan for an Appendix-A update strategy.
- **Hamilton vs. Knuth swap.** Decision before drafting.

---

## 9. Sourcing Notes

- **OpenAI Codex docs** — developers.openai.com/codex/guides/agents-md. URL stable as of May 2026; reverify at press.
- **OpenAI Codex repo** — github.com/openai/codex/blob/main/AGENTS.md. Stable.
- **Knuth 1984** — open access via *Computer Journal* archive.
- **Hamilton sources**: NASA Apollo archive (ntrs.nasa.gov); secondary biographies (Robert McMillan's *Wired* piece on her, 2015) are accessible to high schoolers.
- **DESIGN.md ecosystem links**: designmd.app, getdesign.md, github.com/VoltAgent/awesome-design-md — useful for Ch 10 mostly; brief reference here is fine.
