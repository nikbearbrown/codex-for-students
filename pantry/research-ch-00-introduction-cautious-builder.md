# Research: Chapter 0 — Introduction: The Cautious Builder
## Codex for Students: A Practitioner's Guide

**Chapter one-line:** Meet Seth. He noticed something his friends didn't.

**Research date:** 2026-05-21

---

## 1. Primary Sources

### Foundational papers and texts

- **Wiener, Norbert. *The Human Use of Human Beings: Cybernetics and Society*. Houghton Mifflin, 1950 (rev. 1954).** Wiener's question — what does a tool do to the human who uses it? — is the chapter's opening question. The book's intellectual lineage runs through this text. The 1954 second edition has a stronger ethical framing than the 1950 first edition; cite 1954.
- **Carr, Nicholas. *The Shallows: What the Internet Is Doing to Our Brains*. Norton, 2010.** Carr's argument that tools change cognition is the pre-LLM version of the chapter's claim. Useful for the "I felt it before I had vocabulary" beat.
- **Risko, Evan F. and Sam J. Gilbert. "Cognitive Offloading." *Trends in Cognitive Sciences* 20, no. 9 (2016): 676–688.** The canonical cognitive-science vocabulary for what Seth is observing. One technical sentence carries the chapter.
- **OpenAI. "Introducing Codex." (openai.com/index/introducing-codex/), 2025.** The vendor's own positioning. Useful for confirming what Codex *is* (agentic coding tool, not just chat completion) before the chapter assumes the reader knows.
- **OpenAI. "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor." (forum.openai.com), December 4, 2025.** The "internal use doc" TIKTOC references throughout. This is the public version. The chapter's voice can borrow from it without quoting heavily.

### Key empirical cases

- **The homework-quiz gap in the wild.** Documented across teacher reports in EdWeek and similar outlets, 2024–2026: students completing AI-assisted homework at 90%+ and failing unassisted exams at 50–60%. The Bastani RCT (PNAS 2025) measured this gap precisely; the chapter saves the numbers for Ch 1 and works in plain language here.
- **Seth's classroom observation.** The friend who aces the problem set and freezes on the quiz. This is Seth's testimony and the chapter's emotional anchor. Author should preserve a specific, reproducible instance from Seth's actual experience (which AP CS unit, what kind of problem, how visible the freeze was).
- **The "polished output, no soul" pattern (per Nicholas's observation, cited in Ch 3 and Ch 10).** Seth's friends produce assignments that look professional and reveal nothing. Nicholas is the second collaborator-observer whose voice appears in the book. Author should clarify Nicholas's role and contribution — credit and scope are open questions per TIKTOC's OQ-4.

---

## 2. The Core Concept — State of the Field

### What is settled

- **Cognitive offloading is real and measurable.** Established in cognitive science (Risko & Gilbert 2016) and now in LLM-assisted work specifically (Bastani PNAS 2025, Kosmyna arXiv 2025, Anthropic RCT 2026). The chapter's claim has empirical foundation.
- **The fluency trap is identifiable.** Generated text and code that is grammatically/syntactically clean but semantically derivative is the dominant failure mode for student AI use. Documented broadly in 2024–2026 education research.
- **Codex is an agentic system, not a chat completion endpoint.** OpenAI's documentation is explicit on this point. The book's framing must establish this in Ch 0 — students who think Codex is "just ChatGPT for code" will miss the chapter's pedagogical stakes.

### What is disputed

- **Whether the homework/quiz gap is permanent or reversible.** Bastani showed the gap *during* learning; no published RCT has measured durable persistence six months out. The book treats the gap as a present risk worth mitigating, which is defensible.
- **Whether AI literacy curriculum changes student behavior.** Largely untested. The book stakes its argument on *operational discipline*, not literacy — the correct empirical call.

### What has changed recently (last 5 years)

- Codex shifted from a code-completion model (2021) to a fully agentic coding tool with Ask/Code modes, AGENTS.md, and autonomous task execution (2024–2026). The book is written for the agentic generation; readers familiar with old Codex must update their mental model.
- The Bastani / Kosmyna / Anthropic empirical triplet (2025–2026) is the foundation. The book is the first practitioner book for students to operationalize these findings for Codex.

---

## 3. Application Domain Examples

(Domain coverage map: Ch 0 sits in AP Computer Science / homework.)

- **The Java method that "just works."** Student asks Codex to implement a `findMax` method on a linked list. Codex produces a correct implementation in five seconds. The student copies it, submits the homework, gets 100%. On the quiz, a slight variant is asked (find second-largest), and the student cannot reason about how to modify what they pasted.
- **The Python data-cleaning notebook.** Student needs to clean a CSV. Codex produces a working `pandas` pipeline. Student runs it. The output is correct. The student cannot explain why `groupby` was applied where it was, or what would change if the schema changed.
- **The CS project that compiles and runs.** Friend submits a class project that compiles, passes tests, looks professional. Cannot describe the data structure choices, the time complexity, or why one design was chosen over another. Visible to anyone who asks.

---

## 4. The Book's Thesis Connection

Ch 0 is the felt version of the thesis. The book argues that the student who *conducts* Codex builds capability; the student who *delegates* atrophies. Ch 0's job is to make the reader feel the cost of delegation through Seth's observation — before the framework is named, before the empirical numbers land.

What the chapter must establish:
- The gap between fluency-with-the-tool and fluency-in-the-domain is observable today, in actual classrooms.
- The reader has probably already noticed it. Validation precedes instruction.
- "Conducting" is the alternative to "delegating," and the book's job is to teach what conducting *operationally* means.

Student-supplied capacity (foreshadowed, not named): only the student can decide what the build is *for*. Codex optimizes within the frame given. The frame-setting work cannot be transferred — Ch 5 will name this as Problem Formulation.

---

## 5. The AI Wayback Machine — Candidate Figures

**TIKTOC names: Norbert Wiener (1894–1964).** Keep as primary.

Candidates:
- **Norbert Wiener** (1894–1964, USA, mathematician/cybernetician). Named. *The Human Use of Human Beings*. Substantive fit excellent. Diversity: white male American — does not help spread.
- **Mary Allen Wilkes** (born 1937, USA, computer scientist). First person to use a PC in their home (LINC, 1965). Diversity: woman. Substantive fit moderate (access, not cognition).
- **J. C. R. Licklider** (1915–1990, USA, psychologist/computer scientist). "Man-Computer Symbiosis" (1960). Substantive fit strong; same diversity profile as Wiener.

Recommendation: keep Wiener. If the diversity audit across the full set demands a swap here, Wilkes is a candidate, but Licklider's swap is better deferred to Ch 5 (Engelbart's chapter) which is where his ideas land hardest.

---

## 6. Pedagogical Delivery Research

### Prior knowledge required
The reader has used ChatGPT/Codex regularly. The chapter does not teach Codex basics — it assumes them. The reader is calibrated for Seth's observation to land.

### Common misconceptions to disarm
- **"My friends are cheating."** No. They are using tools the way the tools invite them to be used. The problem is structural, not moral. The chapter must not let the reader feel righteous.
- **"I can already tell the difference between borrowing and building."** Possibly. Possibly not. The chapter must invite self-audit (per the Ch 0 exercises) rather than asserting the reader is exempt.
- **"This is just a critique of AI."** No. The book is pro-Codex use, with discipline. Ch 0's tone must establish this.

### Effective instructional sequences
- **Pebble-in-the-pond.** TIKTOC's pattern. Seth's specific scene before any framework. Concrete-to-abstract.
- **Self-audit exercises.** TIKTOC's Ch 0 exercises ask the reader to inventory their own recent AI use. Strong move — the reader brings the data.
- **Calculator/arithmetic analogy.** TIKTOC mentions it in the Ch 0 exercise list. Good analogy: a calculator is fine *because* the user has arithmetic; if the user has no arithmetic, the calculator is a crutch. The chapter can use this once, briefly.

### Known failure modes
- **The "just give me the prompts" reader.** TIKTOC's Adoption Risk #1. Seth's voice must land in Ch 0 or the reader bounces. The chapter's emotional register matters at least as much as its content.
- **Lecture-mode Ch 0.** If the chapter reads as preface-as-warning, readers skim. Seth's voice is the antidote.
- **Over-naming.** Ch 0 should not name PA/PF/TO/IJ/EI. Those arrive in Ch 5. Ch 0 names the *experience* the framework will later decompose.

### What separates understanding from memorization
A reader who *understands* Ch 0 can describe, in their own life, a moment when they completed something with Codex and could not explain what they had submitted. A reader who memorized Ch 0 can repeat "homework/quiz gap" without producing an instance.

---

## 7. Representation and Display Research

TIKTOC specifies one figure:

- **`<!-- → [DIAGRAM: Seth's arc from observer to practitioner — simple two-point timeline.] -->`** Worked content:
  - Two points on a horizontal line.
  - Left: "Seth watches a friend ace homework and freeze on quiz."
  - Right: "Seth builds the conducting discipline (this book)."
  - Editorial style. Thin black rule. No color. No PA/PF/TO/IJ/EI labels — those arrive later.

No additional displays required.

---

## 8. Open Questions and Research Gaps

- **Seth's specific opening scene.** TIKTOC opens with Seth in AP CS watching a friend rip through a problem set and freeze on the quiz. Author should preserve the specific scene — what class, what problem, what visible reaction. Hypothetical scenes will not carry Ch 0.
- **Nicholas's role.** Per TIKTOC OQ-4, attribution structure for Nicholas is unresolved. Ch 0 doesn't yet need Nicholas, but the book must decide Nicholas's voice before Ch 3 and Ch 10.
- **Codex student access.** OQ-2 and OQ-6. As of 2026, students need ChatGPT Plus ($20/mo) or Pro ($100/mo) for full Codex access; ChatGPT Edu provides institutional access. $100 credit for verified US/Canada university students exists but doesn't extend to high schoolers. **Ch 0 should not require the reader to *have* Codex working yet** — Ch 1 handles install. But the access reality should be acknowledged in a footnote or in the preface.

Sources potentially aging within 3 years: Codex UI/feature references (Ask Mode, Code Mode button names). The cognitive-science citations age slowly.

---

## 9. Sourcing Notes

- **Wiener 1954** — standard edition; cite carefully (1950 vs. 1954).
- **OpenAI "Introducing Codex"** — vendor source, useful for positioning, not for empirical claims about learning.
- **OpenAI "How OpenAI Engineers use Codex"** — published Dec 4, 2025; the chapter and many later chapters quote this. The URL is forum.openai.com/public/blogs/how-openai-engineers-use-codex-to-tackle-big-projects-with-rigor-2025-12-04 — confirm at press.
- **OpenAI "Harness engineering"** essay (openai.com/index/harness-engineering/) is the companion to the above and contains additional engineer voice. Both are the practitioner-side anchor.
- **Risko & Gilbert 2016** — *Trends in Cognitive Sciences*. Standard, stable citation.
