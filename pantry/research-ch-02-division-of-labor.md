# Research: Chapter 2 — What You're Actually Good At (And What Codex Is Better At)
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** Pattern recognition is Codex's domain. Supervisory intelligence is yours. Knowing which is which is the whole game.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Polanyi, Michael. *The Tacit Dimension*. University of Chicago Press, 1966.** "We can know more than we can tell." The student knows their code's intent tacitly; Codex sees only the prompt. The labor split is Polanyi's split between articulable and tacit knowledge.
- **Dreyfus, Hubert L. *What Computers Still Can't Do*. MIT Press, 1992.** Human expertise is contextual and embodied. LLMs are pattern systems; situated judgment is structurally not what they do. The chapter's intellectual foundation.
- **Bommasani, Rishi et al. "On the Opportunities and Risks of Foundation Models." Stanford CRFM, 2021 (arXiv:2108.07258), §1.1.** The technical infrastructure: LLMs are correlation engines over very large corpora. Useful for one technical sentence about why Codex is superhuman at pattern completion.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." forum.openai.com, December 4, 2025.** TIKTOC quotes: "Codex excels at moving fast and covering ground" and "Codex works best with well-scoped tasks that would take you or a teammate about an hour to complete." Both quotes anchor the labor-split discussion in vendor practitioner authority.
- **Klein, Gary. *Sources of Power: How People Make Decisions*. MIT Press, 1998.** Recognition-primed decision. Plausibility auditing — hearing the wrong note before verification — is RPD applied to AI output.

### Key empirical cases

- **The near-miss with the silent failure (TIKTOC's Ch 2 opening).** Seth asks Codex for a function. Output compiles, passes tests, looks correct. Edge case slips through. Author should preserve a specific Seth instance.
- **The OpenAI Codex Sora-Android case.** "Sora Android App built in 18 days with 4 engineers" via Codex porting the iOS codebase — a documented case of pattern-completion at industrial scale. Cite as evidence of what Codex is *superhuman* at (replicating a known pattern across surfaces).
- **The Codex 1,500-PR project.** "Three engineers driving Codex, 3.5 PRs per engineer per day" from the OpenAI internal-use blog. Use selectively — the chapter's reader is not building a million-line codebase, but the case anchors Codex's capacity for pattern-saturated work.

---

## 2. The Core Concept — State of the Field

### What is settled

- **LLMs excel at pattern completion.** Across coding benchmarks (HumanEval, MBPP, SWE-bench), LLM code completion is now at or above mid-level human performance for well-specified tasks.
- **LLMs do not have a stable model of the user's project.** AGENTS.md helps; the model still cannot read the student's mind. Anything the model "knows" about the project must be in AGENTS.md or in the prompt.
- **Scope judgment requires knowledge the model doesn't have.** Structural property: the user's intent and tacit project context are not in the prompt unless made explicit.

### What is disputed

- **How well agentic Codex closes the gap.** Codex's Ask Mode reads files and proposes plans before execution — closing *some* of the context gap. But intent and consequence judgment remain the student's.
- **"Solve-verify asymmetry."** TIKTOC asserts that Codex solves faster than the human can verify, and this gap won't close. Defensible position but should be argued, not asserted; the chapter should acknowledge that *some* verification can be automated (tests, linters) while *intent verification* cannot.

### What has changed recently (last 5 years)

- Codex's agentic mode (Ask + Code) gives the model more context per session than the 2021 completion API. The chapter's labor-split argument is sharper in 2026 than it would have been in 2022 — the model is closer to the human's mental model, so the *remaining* gap (scope, intent, consequence) is more clearly identifiable.
- The 2024–2026 industry shift to "human-in-the-loop on every step" for agentic systems is institutional support for the chapter's argument.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 2 sits in software function / login system.)

- **The login function.** Codex produces `authenticate(username, password)` with correct password-hashing and session-creation patterns. The student must decide: which password rules apply to *this* app (length, complexity, history)? Which session duration is right? Should there be rate limiting? Codex's pattern is right; the project-specific decisions are not in the pattern.
- **The form validation.** Codex generates email-regex validation. Pattern is correct. The student must decide: should this app accept `+` in email addresses (some apps don't)? Should it validate against a domain allowlist? Codex doesn't know.
- **The error handler.** Codex generates a try/catch with logging. Pattern is correct. The student must decide: should this error propagate to the user, be swallowed silently, or trigger a retry? Codex defaults to one option; the right answer depends on the application.

---

## 4. The Book's Thesis Connection

Ch 2 operationalizes the thesis: pattern completion (Codex's strength) and supervisory intelligence (human's job) are different cognitive operations. The chapter must:

1. **Name pattern completion and scope judgment precisely.** Conflation is the chapter's enemy — readers will think pattern completion includes intent, or that they can "give Codex enough context" to do scope judgment. Neither is true.
2. **Establish the solve-verify asymmetry.** Codex generates faster than the student verifies; this gap is structural. Verification is the student's job.
3. **Foreshadow the five capacities.** Ch 5 names them (PA, PF, TO, IJ, EI). Ch 2 operates in plain English so the named decomposition lands harder.

Student-supplied capacity: every scope decision requires knowledge of the project, the user, the deadline, and the consequence of being wrong. None of these are in the prompt unless the student puts them there.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Frederick Winslow Taylor (1856–1915).** Acceptable but the same critique as the GitHub Copilot CLI book applies: Taylor's cultural legacy is fraught.

Candidates:
- **Frederick Winslow Taylor** (1856–1915, USA, mechanical engineer). Named. Strong intellectual fit; weaker cultural valence. Diversity: white male American.
- **Lillian Moller Gilbreth** (1878–1972, USA, industrial/organizational psychologist). Humane half of the Gilbreth-Taylor partnership; ergonomics. The "which tasks suit the human" framing is more Gilbreth than Taylor. **Strongest diverse alternate.** Woman, American, lesser-known.
- **Hubert Dreyfus** (1929–2017, USA, philosopher). *What Computers Still Can't Do*. Most direct intellectual ancestor; less famous to high schoolers. Same broad diversity profile as Taylor.

Recommendation: **swap Taylor for Lillian Gilbreth.** Same rationale as the GitHub Copilot CLI book — diversity improvement plus equal-or-better fit, without Taylor's cultural baggage. Author may have reasons to keep Taylor.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
Some Codex usage. The reader can run a prompt and read output. Ch 2 assumes this.

### Common misconceptions to disarm
- **"Codex is just bad at edge cases."** Codex is *good* at the common case for which it has patterns. Edge cases require knowledge that may not be in the patterns. The framing matters: Codex isn't broken; it's pattern-bound by design.
- **"Better prompts close the gap."** Better prompts narrow Codex's plausibility space; they do not transfer intent. The student still owns intent.
- **"Codex's tests catch the dangerous middle."** Sometimes. Often not — Codex's generated tests test the same patterns Codex generated the code from. Tests written by the student against intent are the irreducible verification.

### Effective instructional sequences
- **Two-column classification exercise.** TIKTOC's Apply-level exercise. Ten tasks; classify each as Codex / human / dangerous middle.
- **Same task, two outcomes.** TIKTOC's worked example. One run unattended, one with supervisory capacities exercised explicitly. Same Codex; different results.
- **Solve-verify diagram.** TIKTOC calls for it. Show Codex's solve-speed line rising; human verification capacity flat; gap widening over time.

### Known failure modes
- **The chapter as capacity-glossary.** Ch 5 names the capacities. Ch 2 must operate in plain English ("supervisory intelligence," "scope judgment") and earn Ch 5's decomposition.
- **Sounding like a manifesto.** "What you're actually good at" is intentionally cheeky; the chapter should not become a self-help essay.
- **Defeatism.** "Codex will always be better at code" — true for pattern work, false for intent. The chapter must keep the labor split clear so the reader doesn't conclude they have nothing to contribute.

### What separates understanding from memorization
A reader who *understands* Ch 2 can take a new build task and decompose it into pattern-completion work and scope-judgment work without prompting. A reader who memorized Ch 2 can repeat the labor-split lists without applying them.

---

## 7. Representation and Display Research

TIKTOC specifies two figures:

- **`<!-- → [TABLE: Division of labor — two columns: Codex does / Human does.] -->`** Worked content:

  | Codex does | Human does |
  |---|---|
  | Pattern completion | Plausibility auditing |
  | Code generation | Problem formulation |
  | Syntax resolution | Tool orchestration |
  | Test execution | Interpretive judgment |
  | Refactoring at scale | Executive integration |

- **`<!-- → [DIAGRAM: The solve-verify asymmetry — timeline.] -->`** Worked content:
  - Horizontal time axis.
  - Top line: Codex's solve speed — increasing slope.
  - Bottom line: human verification capacity — flat.
  - Widening gap labeled. Editorial style.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **The Lillian Gilbreth swap.** Recommended above; author's call.
- **Empirical separability of pattern vs. scope.** No published study cleanly measures the distinction the chapter draws. The labor split is a defensible synthesis from foundation models / cognitive science / human-factors literature. Acknowledge.
- **Codex Ask Mode's role in the labor split.** Ask Mode acquires more project context than a one-shot prompt — narrowing hallucination, not removing intent gap. The chapter should address this without conceding the supervisory-judgment claim.

---

## 9. Sourcing Notes

- **Polanyi 1966** — canonical; use sparingly.
- **Dreyfus 1992** — 2nd edition has the AI-aware preface. Cite this edition.
- **Bommasani 2021** — arXiv; cite a specific version (v3 is standard).
- **OpenAI engineer-voice quotes**: forum.openai.com Dec 4 2025 post. Attribute carefully.
- **Klein 1998** — trade book; academic foundation in Klein's RPD papers (1989, 1993).
