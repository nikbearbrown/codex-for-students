# Research: Chapter 12 — Running the Build: Codex Tasks and Human Tasks
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** The plan is approved. Now you execute it in Code Mode — one step at a time, with explicit handoff conditions between every step.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Deming, W. Edwards. *Out of the Crisis*. MIT Press, 1986.** PDCA cycle. The chapter's operational model.
- **Spear, Steven J. and H. Kent Bowen. "Decoding the DNA of the Toyota Production System." *HBR*, Sep 1999.** Andon-cord discipline. The chapter's "revert and respecify" is andon applied to AI-assisted code.
- **Allspaw, John and Jesse Robbins, eds. *Web Operations*. O'Reilly, 2010.** Particularly incident-response chapters.
- **Beyer, Betsy et al. *Site Reliability Engineering* (Google SRE book). O'Reilly, 2016.** Ch 13 ("Emergency Response") and Ch 14 ("Managing Incidents"). Open access at sre.google.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC quotes throughout: feedback-mechanism guidance, iteration limits, task queue use. Vendor authority.

### Key empirical cases

- **Seth's full execution transcript (TIKTOC worked example).** Seth's actual prompts, Codex's actual outputs, his handoff evaluations, one rejection and revision, the final accepted output. Author + Seth must produce.
- **The "scope creep" prompt.** Codex suggests an unrelated improvement. The book's rule: log in task queue, decline now. Documented widely in software-engineering culture.
- **The "Codex iterates against tests" pattern.** From OpenAI: when Codex has a feedback mechanism, results are "dramatically better." Show a worked example with `pytest` invocation in the specification, and Codex iterating until tests pass.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Stop-on-failure reduces compounding error.** 70-year lineage from Toyota.
- **Scope creep during execution increases cost and error rates.** Standard finding.
- **Post-execution review improves future performance.** Established in human-factors and operations.
- **Feedback-mechanism in prompts produces better results.** OpenAI's own guidance.

### What is disputed

- **"Revert and respecify" appropriateness for student work.** Some say overkill. Book: learn the discipline at low stakes.
- **Two failed corrections as threshold.** Defensible; some use three.
- **Autopilot mode.** Codex's more-autonomous modes handle Ch 12's per-step gate as a built-in feature. Book: autopilot is a tool; active review remains the student's discipline.

### What has changed recently (last 5 years)

- The 2026 generation of agentic Codex chains operations more autonomously. This *raises* stakes for active review.
- "Approval-required" patterns for destructive ops are now standard in enterprise 2024–2026. The book teaches the discipline industry mandates.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 12 sits in the student build — Seth's actual project.)

The chapter's worked example must be Seth's real build, executed step by step. Suggested structure:

- **Step 1:** Specification for the first build step, including a feedback mechanism (a test command Codex can run). Codex executes in Code Mode. Handoff condition: tests pass + outputs match expected shape from spec.
- **Step 2:** Next step. One handoff failure: Codex's output passes tests but the file structure doesn't match the AGENTS.md convention. Revert and respecify with explicit file-path constraint. Revision passes.
- **Step 3:** Scope-creep moment. Codex suggests "while I'm here, want me to refactor X?" Logged to task queue. Declined for now.
- **Step 4:** A plausibility-auditing moment. Something passes but feels wrong. Seth investigates. Finds a subtle issue. Respecifies.

---

## 4. The Book's Thesis Connection

Ch 12 is where the thesis becomes lived experience. Every framework piece appears in execution.

The chapter's contribution:

1. **Executing the discipline.** Framework in motion.
2. **Operational rules for doubt moments.** Revert-and-respecify, two-failed-corrections, scope-creep-to-task-queue.
3. **The post-build moment.** Ch 13's territory.

Student-supplied capacity: at every step, the student decides whether to proceed, revert, respecify, or stop. None transfer.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: W. Edwards Deming (1900–1993).** Strong fit.

Candidates:
- **Deming** — named. PDCA. Famous in operations.
- **Taiichi Ohno** (1912–1990, Japan). Andon-cord. **Strongest diverse alternate.**
- **Walter Shewhart** (1891–1967, USA). PDCA before Deming. More academic.

Recommendation: **consider Ohno.** Andon-cord is the chapter's exact stance; Ohno helps diversity. Deming remains excellent.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Full Act Two + Ch 11 planning.

### Common misconceptions to disarm
- **"Good plan = automatic execution."** No. Execution tests the plan.
- **"Revert is failure."** Revert is the discipline working.
- **"Scope creep is fine if the addition is good."** No. Log it; do it later.
- **"Autopilot handles this."** No. Autopilot shows what *Codex* would do; review is what *student* must do.

### Effective instructional sequences
- **Seth's full transcript annotated.** Every step labeled with capacity.
- **The three pivotal moments.** Handoff failure, scope creep, plausibility audit. Labeled segments.
- **Reader runs alongside Seth.** Optional but powerful.

### Known failure modes
- **Transcript dump.** Narrative + transcript.
- **Romanticizing difficulty.** First builds going wrong twice is normal.
- **Over-promising rules.** Guidelines, not laws.

### What separates understanding from memorization
A reader who *understands* Ch 12 can predict where scope-creep risk lives in a build they have not yet attempted. A reader who memorized Ch 12 can recite the rules without applying them.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: The build loop.] -->`** Specification → Code Mode execute → handoff check → pass: next step / fail: revert + respecify. Supervisory capacity label at check step. Editorial style.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **Seth's build (OQ-1).** Most acute here.
- **Plan-mode + Code-mode integration.** Author should run Seth's actual build through the 2026 Codex UI and document the experience for the chapter.
- **The "while I'm here" rule.** As Codex becomes more agentic, the rule fires more often. Author may want to specify aggressiveness (every suggestion? every file-touching one? only destructive?).

---

## 9. Sourcing Notes

- **Deming 1986** — MIT Press, 2nd ed. (2018) recent reprint.
- **Spear & Bowen 1999** — HBR archive.
- **Allspaw & Robbins 2010** — O'Reilly.
- **Beyer et al. 2016** — open access at sre.google.
- **Ohno 1988** — Productivity Press.
- **OpenAI engineer-voice quotes** — forum.openai.com Dec 2025.
