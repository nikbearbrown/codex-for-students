# codex-for-students-a-practitioners-guide
## Full TOC Draft — All Phase Outputs Compiled

**Working title:** Codex for Students: A Practitioner's Guide
**Series:** Practitioner Guides for the AI Classroom · Bear Brown & Company
**Author:** Nik Bear Brown · bear@bearbrown.co · Bear Brown, LLC
**Co-author:** Seth Brown
**Document:** Full TOC Draft — compiled from all phase outputs
**Version:** 1.0
**Status:** Pre-production — ready for chapter drafting

---

## Document structure

1. Book Concept and Thesis
2. Learner Profile
3. Book Type and Deployment Specification
4. Field Positioning
5. Three-Act Learning Arc
6. Prerequisite Map
7. Build Arc and Terminal Deliverables
8. Chapter-by-Chapter TOC
9. Chapter Anatomy Template
10. Case Study Strategy
11. Hard Topics, Contested Claims, Aging Risk
12. Market Positioning
13. Feature List
14. Out of Scope
15. Adoption Risk Register
16. Open Questions

---

# PART 1 — BOOK CONCEPT AND THESIS

## Book concept summary

> This book teaches **technically fluent high school students how to
> use Codex as a Conductor** — directing the machine through real
> builds while protecting the cognitive struggle that builds their
> own capability — by showing exactly where to draw the line between
> what Codex executes and what only the student can do, through the
> story of a student who learned the discipline by practicing it.
> It fills the gap left by generic AI literacy courses (which treat
> students as problems to manage) and prompt engineering guides
> (which optimize for output, not for learning). It succeeds when
> the reader can **run a complex build with Codex and finish it
> knowing more than when they started, not less**.

**One-sentence logline:**
The student who learns to conduct Codex builds faster and thinks
better; the student who lets it run unattended builds a convincing
artifact and an atrophying mind.

## Central thesis

"This book argues that Codex is not a shortcut but an instrument —
that the student who draws an explicit line between what Codex
executes and what only the human can do will build real capability,
while the student who delegates without that line will produce
polished output while their own thinking atrophies, and this matters
because the homework/quiz gap is already visible in every classroom
where AI tools are used without a conducting discipline."

## Thesis test

The TOC reflects the thesis at every act:

- ACT ONE: Seth watches his friends ace homework and freeze on
  quizzes. The reader feels the cost before they need the solution. ✓
- ACT TWO: The conducting framework, the five supervisory
  capacities, AGENTS.md, Ask Mode → Code Mode workflow, the
  Brutalist three-file system — the line is drawn operationally,
  tool by tool. ✓
- ACT THREE: The reader runs their own first fully conducted build,
  from problem formulation through verified output. The discipline
  is practiced, not described. ✓

**Thesis test: PASS**

---

# PART 2 — LEARNER PROFILE

## Primary reader

A technically fluent high school student, 2026. Already uses
Codex or ChatGPT daily. Ahead of teachers on tooling. Watching
friends borrow capability they haven't built. Technically fluent,
domain-shallow, honest enough about the gap to want a discipline
that closes it.

**Specific person:** Seth Brown — the Cautious Builder. AP
Computer Science student who noticed that his friends were acing
homework with AI and freezing on unassisted quizzes. He reached out
not for a lecture on plagiarism but for a concrete operational
discipline that would let him build real software with Codex without
hollowing out his own thinking. He is now co-authoring this book
by practicing the discipline as he writes it.

## Prior knowledge assumed

- Basic prompting with ChatGPT or Codex
- Some coding experience (AP CS level or equivalent)
- Familiarity with browser-based or terminal-based AI tools
- Can run a terminal command if given exact syntax

## Prior knowledge NOT assumed

- Codex CLI or API
- AGENTS.md
- Ask Mode vs. Code Mode distinction
- The five supervisory capacities by name
- The conducting framework
- Software Design Documents
- Any formal software engineering methodology

## Prior misconceptions

1. "If Codex writes correct code, I learned something" — the
   Bastani RCT shows the opposite: 48% higher practice scores,
   17 percentage points lower on the unassisted exam
2. "Using AI more is always better" — the Kosmyna EEG study
   shows up to 55% reduction in brain connectivity during
   AI-assisted writing vs. brain-only writing
3. "I'm ahead of my teachers on AI, so I know enough" — technical
   fluency without domain depth is the specific danger zone
4. "Codex is just a better chatbot" — it is an agentic system that
   plans multi-step tasks, executes shell commands, reads and
   writes files, and iterates autonomously; the autonomy is the
   feature and the risk simultaneously

## Motivation type

**Primary:** Intellectual — Seth and his reader have already
noticed the problem and want the repair kit.
**Secondary:** Professional — they want to build real things, not
just homework assignments.
**Not primary:** Academic (course requirement) — this book is
found independently, not assigned.

## Prerequisite map

| Prerequisite | Safe to assume? | If not: where addressed |
|---|---|---|
| Basic AI prompting | Yes | — |
| Some coding experience | Yes | — |
| Terminal familiarity | Probably | Ch 1 first session |
| Codex CLI installed | No | Ch 1 installation |
| AGENTS.md concept | No | Ch 6 (AGENTS.md chapter) |
| Ask Mode vs. Code Mode | No | Ch 4 (conducting chapter) |
| Five supervisory capacities | No | Ch 5 |
| Conducting framework | No | Ch 4 |

**Front-loading decision:** Terminal installation and first session
walkthrough are addressed in Chapter 1 as primary instruction.

---

# PART 3 — BOOK TYPE AND DEPLOYMENT SPECIFICATION

## Book type

**PRIMARY TYPE:** Practitioner handbook with course-textbook
bones. The student reads it cover to cover once, then consults
it chapter by chapter during builds.

**NOT:** A course textbook adopted for a semester (though it can
be assigned); a reference work (sequence dependency is too high).

## Deployment specification

**Primary adoption context:**
Self-directed high school student finding it independently —
through recommendation, through the $1 Kindle price, through
a teacher who assigned it. Not a classroom adoption play primary.

**Secondary adoption context:**
K-12 CS teachers using it alongside
`codex-for-teachers-a-practitioners-guide` — the teachers book
references this book explicitly. At $1 on Kindle, assignable
without a budget conversation.

**Tertiary adoption context:**
University intro CS courses addressing AI-assisted coding
discipline in the first semester.

**What the book is NOT designed for:**
Generic AI literacy courses; students who want prompt engineering
tips; courses where the teacher wants to ban AI use.

**Price point:** $1 Kindle — designed for assignment without
budget friction.

---

# PART 4 — FIELD POSITIONING

## The positioning statement — consolidated

The competitive landscape has a clean gap:

- Generic AI literacy courses treat students as problems to manage.
  This book treats them as builders to equip.
- Prompt engineering guides optimize for output quality. This book
  optimizes for capability retention.
- The Vanderbilt/Coursera Codex course targets developers. This
  book targets students.
- No existing book takes a technically fluent student seriously,
  hands them a production discipline, and is co-authored by one
  of them.

## Positioning statements by comparable

**vs. generic AI ethics / responsible use curriculum:**
"Unlike responsible AI use curricula, which address students as
potential plagiarists to be managed, this book addresses students
as builders who need a concrete operational discipline — the
conducting framework — that protects their own learning while
enabling ambitious builds."

**vs. the teachers book in this series:**
"This book is the student's half of a two-book pair.
`codex-for-teachers` is the teacher's half. Together they give
both sides of the classroom the same discipline in role-appropriate
form. At $1 on Kindle, this book is assignable by any teacher
who reads the companion."

## Research validation

The Anthropic RCT (January 2026): 17 percentage point decline in
conceptual test performance for AI-assisted groups vs. hand-coders
(Cohen's d = 0.738, p = 0.01). The three low-scoring interaction
patterns (AI Delegation, Progressive AI Reliance, Iterative AI
Debugging) are the behaviors this book's discipline prevents.

The Bastani finding and Kosmyna EEG study (up to 55% reduction
in brain connectivity during AI-assisted writing) ground the
neurobiological argument in Chapter 1.

The OpenAI internal use doc confirms the conducting discipline
from the practitioner side: "Start with Ask Mode to get a plan,
then switch to Code Mode" — the same Explore → Plan gate this
book teaches, named differently.

---

# PART 5 — THREE-ACT LEARNING ARC

## The arc statement

This book takes the reader from **technically fluent but
cognitively unguarded** to **disciplined conductor** — first by
naming the risk Seth saw in his friends before he had vocabulary
for it, then by building the operational framework piece by piece
through real build tools, then by running a complete project from
problem formulation through verified output using the full
discipline.

## The pebble-in-the-pond opening

Chapter 0 gives the reader Seth in AP Computer Science, watching
a friend rip through a problem set and freeze on the quiz.
Chapter 1 gives them the neurobiological mechanism. The problem
is felt before the framework is named. Act One ends with the
reader understanding exactly what is at stake before Act Two
gives them the tools.

## Act One — The Problem (Chapters 0–3)

**Starting state:** The reader is technically fluent but has no
discipline for where to draw the line.
**Inciting question:** "If I can get Codex to write the code, why
am I learning anything?"
**Transition condition:** The reader must feel the cost of
undisciplined AI use and want a concrete alternative.

## Act Two — The Discipline (Chapters 4–10)

**Starting state:** The reader wants the discipline but has no
framework.
**Ending state:** The reader can run a structured build with
explicit handoff conditions, naming which cognitive work they
keep and which they delegate.
**Hardest moment:** Chapter 9 — the dangerous middle. Codex
produces output that passes every handoff condition the student
wrote and is still wrong.
**Transition condition:** The reader must be able to state, for
any build step, whether it belongs to Codex or to them, and why.

## Act Three — The Build (Chapters 11–14)

**Starting state:** The reader has the framework but has not
applied it to a complete real project.
**Terminal deliverable:** The reader's first fully conducted
build — planned, executed, verified, documented in a post-build
learning record.
**The arc closes on the reader, not Seth.** Seth's arc ends at
the Act Two/Three bridge. The reader owns the final chapter.

---

# PART 6 — PREREQUISITE MAP

## Prerequisite chain by chapter

| Chapter | Prerequisite capabilities | Load-bearing? |
|---|---|---|
| Ch 0 (Introduction) | None | No |
| Ch 1 (Homework/Quiz Gap) | None | No |
| Ch 2 (Division of Labor) | Ch 1 risk established | No |
| Ch 3 (Teacher-Student Gap) | Ch 1 + Ch 2 | No |
| Ch 4 (Conducting) | Ch 1–3 problem understood | Yes — core metaphor |
| Ch 5 (Five Capacities) | Ch 4 conducting metaphor | Yes |
| Ch 6 (AGENTS.md) | Ch 4–5 framework | Yes |
| Ch 7 (Problem Formulation) | Ch 6 AGENTS.md concept | Yes |
| Ch 8 (Specifications) | Ch 7 problem formulation | Yes |
| Ch 9 (Handoff Conditions) | Ch 8 specifications | Yes |
| Ch 10 (Brutalist) | Ch 4–9 full framework | No |
| Ch 11 (Planning) | Ch 4–10 full framework | Yes |
| Ch 12 (Running the Build) | Ch 11 plan complete | Yes |
| Ch 13 (Verification) | Ch 12 build in progress | No |
| Ch 14 (Full Build) | Ch 11–13 full sequence | No |

---

# PART 7 — BUILD ARC AND TERMINAL DELIVERABLES

## The three build tiers

| Tier | Chapters | Build | Terminal artifact |
|---|---|---|---|
| Foundations | 1–3 | None — observation only | Personal AI audit |
| Framework | 4–10 | Tool-level exercises | Labor separation rule; DESIGN.md |
| Full build | 11–14 | Complete conducted project | Post-build learning document |

## Terminal deliverable specification

**The reader's first fully conducted build:**

Required components:
1. Problem formulation (one-sentence, Ask Mode interrogation)
2. Minimum viable spec document (five core sections)
3. Ask Mode plan reviewed and approved before Code Mode begins
4. Build execution log (specification per step, handoff condition
   evaluation per step, supervisory capacity label per step)
5. Verification pass against spec needs
6. Post-build learning document (five sections)

**Post-build learning document — five sections:**
1. What I built (one paragraph, plain language)
2. What I delegated to Codex and why
3. What I kept for myself and why
4. What I learned that I didn't know before
5. What I would do differently

**Success criterion:** Not a perfect build. A build where the
reader can account for every decision.

## Chapter exercise structure

Every chapter: minimum three assessable exercises.
- One at Apply level or above (Bloom's)
- One requiring the student to produce something
- One connecting to their own current project

Bloom's distribution:
- Remember/Understand: Ch 0–3
- Apply: Ch 4–9
- Analyze/Evaluate: Ch 9–13
- Create: Ch 11–14

---

# PART 8 — CHAPTER-BY-CHAPTER TOC

---

## ACT ONE — THE PROBLEM
*Chapters 0–3: From technically fluent to cognitively aware*

---

### Chapter 0 — Introduction: The Cautious Builder

**One-line:** Meet Seth. He noticed something his friends didn't.

**Learning outcomes:**
1. (Remember) Name the difference between borrowing capability
   and building it.
2. (Understand) Explain why a high homework grade with a low
   quiz grade is a signal, not a coincidence.
3. (Understand) Describe what "conducting" Codex means vs.
   letting it run.

**Opening:** Seth in AP Computer Science, watching a friend rip
through a problem set in thirty seconds with Codex. The friend
gets an A on the homework. Two weeks later, same friend freezes
on the in-class quiz. Seth already knows something is wrong.
He doesn't yet know what to call it.

<!-- → [DIAGRAM: Seth's arc from observer to practitioner — simple
two-point timeline showing "watches friends" → "builds the
discipline". Minimal. Editorial style. No color.] -->

**Core content:**
- Seth's observation: the homework/quiz gap
- Why fluency with the tool is not the same as fluency in the domain
- What this book is and is not
- How to read this book: Seth's voice and your own experience

**Assessable exercises:**
1. (Remember) Before reading Chapter 1: write down three things
   you have built with AI in the last month. For each: could you
   explain every decision in it to someone who has never seen
   the code?
2. (Understand) What is the difference between using a calculator
   and learning arithmetic? Write one paragraph. Keep it. You
   will return to it in Chapter 14.
3. (Understand) Name one thing Seth noticed that you have also
   noticed in your own class. One sentence.

**Wayback Machine:** 🕰️ **Norbert Wiener** (1894–1964) — founder
of cybernetics; the first systematic thinker about the feedback
loop between human and machine. His question — what does the
machine do to the human who uses it? — is the question this
chapter asks.

**Bridge:** The feeling Seth has is real. Chapter 1 gives it a
name, a number, and a neurobiological mechanism.

---

### Chapter 1 — The Homework/Quiz Gap: What's Actually Happening

**One-line:** Students who use AI freely during practice score
dramatically lower on unassisted tests — and feel like they
learned more, not less.

**Learning outcomes:**
1. (Understand) Explain why AI-assisted practice can produce
   the feeling of mastery without the cognitive events that
   constitute it.
2. (Analyze) Distinguish between capability borrowed from the
   machine and capability built in the learner.
3. (Evaluate) Assess their own recent AI use against this
   distinction.

**Opening:** The Bastani finding stated plainly: 48% higher scores
during AI-assisted practice, 17 percentage points lower on the
unassisted exam. Not slightly worse. Dramatically worse.

<!-- → [TABLE: Bastani RCT results — two columns: AI-Assisted
group vs. Hand-Coding group. Rows: practice score, exam score,
score gap, Cohen's d, p-value. No color.] -->

**Core content:**
- What actually happens in the brain when you struggle with a
  problem (plain language)
- What happens when AI does the struggling for you
- The fluency trap: AI output that looks finished and feels like
  understanding
- The Kosmyna EEG result: up to 55% reduction in brain
  connectivity during AI-assisted writing
- The three low-scoring interaction patterns from the Anthropic
  RCT: AI Delegation, Progressive AI Reliance, Iterative
  AI Debugging
- The debugging gap: largest performance disparity on diagnostic
  questions
- Start with Ask Mode questions before building: before your
  first build session with Codex, use Ask Mode to interrogate
  the codebase or problem space. OpenAI engineers do this on
  day one — it teaches you the boundary before you delegate
  anything.

<!-- → [DIAGRAM: The fluency trap — two-path diagram. Path A:
struggle → consolidation → durable capability. Path B: delegate
→ fluent output → no consolidation → atrophy. Editorial style.
No color.] -->

**Worked example:** Two students, same problem set, same final
grade. One builds a database schema by working through it. One
asks Codex for the schema and reads it. Six weeks later: one can
design a database. One can describe what a database looks like.

**Assessable exercises:**
1. (Apply) Identify three recent AI interactions of your own.
   For each: did you build the capability or borrow it?
2. (Analyze) Given two assignment transcripts (provided),
   identify which student is building and which is borrowing.
3. (Evaluate) Design a rule for your own AI use that would
   prevent the homework/quiz gap in your next unit.

**Wayback Machine:** 🕰️ **William James** (1842–1910) — psychologist
who first described habit formation as the nervous system's
mechanism for consolidating repeated struggle into durable
capability. His 1890 account of what repetition does to the
brain is the mechanism the Bastani finding measures.

**Bridge:** The reader knows the risk. They don't yet know which
specific cognitive capacities are at stake.

---

### Chapter 2 — What You're Actually Good At (And What Codex Is Better At)

**One-line:** Pattern recognition is Codex's domain. Supervisory
intelligence is yours. Knowing which is which is the whole game.

**Learning outcomes:**
1. (Understand) Distinguish pattern recognition (where AI is
   superhuman) from supervisory intelligence (where AI is
   structurally weak).
2. (Apply) Classify a set of build tasks as Codex work or
   human work.
3. (Analyze) Identify the specific supervisory capacity being
   exercised at a given step in a build.

**Opening:** Seth asks Codex to write a function. Codex produces
something that compiles, passes basic tests, and looks correct.
Seth runs it. Edge case: silent failure. Codex didn't know the
edge case existed. Seth almost didn't catch it. That near-miss
is the chapter.

<!-- → [TABLE: Division of labor — two columns: Codex does /
Human does. Rows: pattern completion, code generation, syntax
resolution, test execution (Codex) vs. plausibility auditing,
problem formulation, interpretive judgment, tool orchestration,
executive integration (Human). No color.] -->

**Core content:**
- What pattern recognition means and why Codex is superhuman at
  it — "excels at moving fast and covering ground" (OpenAI
  internal use doc)
- What supervisory intelligence means: the five capacities in
  plain language
- Why AI is structurally weak at supervision: same weights that
  produced the output are doing the audit
- The solve-verify asymmetry: Codex solves faster, that gap
  won't close; verification against domain reality is
  irreducibly human
- The dangerous middle: tasks that look like pattern work but
  require supervisory judgment
- From OpenAI's internal use doc: "Codex works best with
  well-scoped tasks that would take you or a teammate about an
  hour to complete" — the scoping judgment is the human's

<!-- → [DIAGRAM: The solve-verify asymmetry — timeline. Codex's
solve speed increasing. Human verification capacity stable.
The gap widens. The human's job: not to solve faster but to
verify better.] -->

**Worked example:** The same build step analyzed twice — once
with Codex running unattended, once with the student exercising
each supervisory capacity explicitly. Same Codex output.
Different results.

**Assessable exercises:**
1. (Apply) Given a list of 10 build tasks, classify each as
   Codex work, human work, or "dangerous middle."
2. (Analyze) Read a provided Codex transcript. Identify every
   moment where a supervisory capacity should have been
   exercised but wasn't.
3. (Create) Write your own labor separation rule for a project
   you are currently working on.

**Wayback Machine:** 🕰️ **Frederick Winslow Taylor** (1856–1915) —
the first systematic analyst of the division of labor between
human judgment and mechanical execution. His question — which
cognitive work belongs to the engineer and which to the machine?
— is the question this chapter answers for AI.

**Bridge:** The reader can name the capacities. Chapter 3
explains why school isn't teaching them.

---

### Chapter 3 — The Teacher-Student AI Gap: Why You're On Your Own

**One-line:** You know more than your teachers about the tools,
and less than you need to about the domains. That gap is exactly
where AI is most dangerous.

**Learning outcomes:**
1. (Understand) Explain the teacher-student AI gap and why it
   produces a specific kind of risk for technically fluent
   students.
2. (Analyze) Distinguish technical fluency from domain depth.
3. (Evaluate) Assess their own domain depth in a subject they
   use AI for regularly.

**Opening:** The frustration named directly. Your teachers are
trying to ban the thing you already know better than they do.
The curriculum is three years behind the tools. You need a
discipline that works without institutional scaffolding.

**Core content:**
- Why technical fluency without domain depth is the specific
  danger zone
- The hallucination problem: Codex produces confident output in
  domains where the student has no basis to audit it
- Nicholas's observation: polished output with no soul, no
  aesthetic stance, no genuine human intent
- Why the answer is not to use AI less but to build supervisory
  capacity
- What "domain depth" means in practice and how you build it

**Worked example:** Seth encounters a Codex explanation of a
concept he doesn't know well enough to audit. He accepts it.
It's wrong. He doesn't find out until the test.

**Assessable exercises:**
1. (Apply) Choose a subject where you use AI regularly. List
   three claims Codex has made that you accepted without
   verification. Go verify them now.
2. (Analyze) Identify a domain where your depth is sufficient
   to audit Codex and a domain where it isn't.
3. (Evaluate) Design a personal audit protocol for Codex
   outputs in your weakest domain.

**Wayback Machine:** 🕰️ **Ivan Illich** (1926–2002) — philosopher
who argued in *Tools for Conviviality* (1973) that tools become
counterproductive when they outpace the human capacity to use
them wisely. His concept of "counter-productivity" is the
teacher-student gap applied to institutions.

**Bridge:** The reader understands the problem. Chapter 4
introduces conducting.

---

## ACT TWO — THE DISCIPLINE
*Chapters 4–10: From awareness to operational framework*

---

### Chapter 4 — Conducting, Not Prompting: The Core Idea

**One-line:** Programming as conducting. Codex does what it's
superhuman at. You do what only you can.

**Learning outcomes:**
1. (Understand) Explain the difference between prompting Codex
   and conducting a build with Codex.
2. (Apply) Use Ask Mode for planning and Code Mode for
   execution — and explain why the gate between them matters.
3. (Understand) Explain what a handoff condition is and why
   it matters.

**Opening:** The orchestra metaphor in Seth's voice. You are
the conductor. Codex is the orchestra. The orchestra is
excellent. They will play exactly what they understood you to
mean. The gap between what you meant and what they understood
is where everything breaks.

<!-- → [DIAGRAM: The Ask Mode / Code Mode gate. Human in Ask Mode:
interrogate, plan, formulate. Gate: plan reviewed and approved.
Human in Code Mode: execute against plan, verify output.
Editorial style. No color.] -->

**Core content:**
- Ask Mode: Codex operates in read-only planning mode —
  interrogates the codebase, answers questions, proposes plans.
  The human reviews and approves before any code is written.
  From OpenAI: "For large changes, start by prompting Codex
  for an implementation plan using Ask Mode."
- Code Mode: Codex executes. The human verifies at every step.
- The Ask Mode → Code Mode gate is the conducting discipline's
  most important mechanism. Nothing executes until the plan
  is reviewed.
- What a handoff condition is: not "looks good" but a specific,
  testable condition that must be true before the next step
- The conducting metaphor: you are the composer who writes the
  score, not the player who executes it
- Best-of-N as a supervisory tool: generate multiple Codex
  responses and choose — the selection judgment is irreducibly
  human

**Worked example:** One build step done twice. First: "write
me a login function" directly in Code Mode. Second: Ask Mode
plan reviewed and approved, then Code Mode with a specification.
Same Codex. Completely different results.

**Assessable exercises:**
1. (Apply) Take a prompt you've used in the past week. Rewrite
   it as an Ask Mode interrogation followed by a Code Mode
   specification.
2. (Apply) Write a handoff condition for a Codex task in a
   current project.
3. (Analyze) Given a provided Codex transcript, identify where
   the Ask Mode / Code Mode gate was skipped and what broke.

**Wayback Machine:** 🕰️ **Herbert Simon** (1916–2001) — Nobel
laureate who formalized bounded rationality: the human decision-
maker who works within real cognitive limits by designing systems
that extend those limits. The Ask Mode / Code Mode gate is
bounded rationality applied to AI-assisted development.

**Bridge:** The reader has the metaphor and the basic mechanics.
Chapter 5 names the five things the human must never delegate.

---

### Chapter 5 — The Five Supervisory Capacities

**One-line:** These are the five things you do that Codex cannot.
Name them. Practice them. Never delegate them.

**Learning outcomes:**
1. (Remember) Name and define the five supervisory capacities.
2. (Apply) Identify which supervisory capacity is being
   exercised at each step of a provided build sequence.
3. (Analyze) Diagnose a build that went wrong by identifying
   which supervisory capacity was absent.

**Opening:** Seth mid-build. Something is wrong with Codex's
output. It compiles. It seems fine. He almost ships it. What
capacity would have caught this? Answer: plausibility auditing.

<!-- → [DIAGRAM: Five supervisory capacities as a five-column
layout. Each: label (PA, PF, TO, IJ, EI), plain-language name,
one-sentence definition. Editorial style. No color.] -->

**Core content:**

**[PA] Plausibility Auditing:** Hearing the wrong note before
verification. "This compiles and the tests pass. But why does
it feel wrong?" Checking output against domain knowledge that
isn't in the prompt.

**[PF] Problem Formulation:** Deciding what the build IS before
Codex sees it. From OpenAI's internal use doc: "When I'm in
meetings all day, Codex works in the background — but I give it
the direction first." Codex optimizes within the frame. If the
frame is wrong, the output is wrong, elegantly.

**[TO] Tool Orchestration:** Choosing which Codex mode, which
task, in what order, with what trust level. Deciding when to
use Ask Mode vs. Code Mode. Deciding when to use Best-of-N.

**[IJ] Interpretive Judgment:** Supplying meaning Codex's output
cannot carry. "This output is technically correct. What does it
mean for this specific project, this specific deadline, this
specific user?"

**[EI] Executive Integration:** Holding the whole build toward
a single goal. "Three prompts ago we agreed on an architecture.
This new output is undermining it. Stop."

<!-- → [TABLE: Five supervisory capacities — label, name, what
it catches, example failure when absent. Five rows.] -->

**Worked example:** A complete build sequence analyzed step by
step, with each supervisory capacity labeled where it appears.

**Assessable exercises:**
1. (Apply) Label the supervisory capacity required at each step
   of a provided build plan.
2. (Analyze) A build transcript is provided where one supervisory
   capacity is systematically missing. Identify which one and
   trace the consequences.
3. (Create) Design a personal checklist for a current project
   using the five capacity labels.

**Wayback Machine:** 🕰️ **Douglas Engelbart** (1925–2013) —
inventor of the mouse and pioneer of human-computer interaction
who argued that computers should augment human intellect, not
replace it. His 1962 framework for augmenting human capability
is the conceptual ancestor of the five supervisory capacities.

**Bridge:** Chapter 6 introduces AGENTS.md — the file that
makes every subsequent Codex session smarter.

---

### Chapter 6 — AGENTS.md: Your Coding Constitution

**One-line:** AGENTS.md is the file Codex reads at the start of
every session. It is the difference between a Codex that knows
your project and a Codex that guesses.

**Learning outcomes:**
1. (Understand) Explain what AGENTS.md is, where it lives, and
   when Codex reads it.
2. (Apply) Write an AGENTS.md for a student build project using
   the five-element format.
3. (Analyze) Distinguish AGENTS.md content (always-on rules)
   from task-queue items (session-specific) and Best-of-N
   selection criteria (judgment layer).

**Opening:** Seth starts his second Codex session. Codex has no
memory of the first one. Seth types the same context he typed
yesterday. This chapter ends that.

<!-- → [DIAGRAM: AGENTS.md in the session context — loaded at
session start, persists across the session, governs every prompt.
Contrast: without AGENTS.md (Codex guesses) vs. with AGENTS.md
(Codex knows). Editorial style.] -->

**Core content:**
- What AGENTS.md is: a markdown file Codex reads at every
  session start — project conventions, build commands,
  "never do X" rules
- The five-element format: bash commands Codex can't guess,
  code style deviations, test runners, architectural decisions,
  environment quirks
- What NOT to include: anything Codex can figure out from code,
  standard conventions, anything that changes frequently
- The size rule: keep it tight. Bloated AGENTS.md causes Codex
  to lose track of important rules.
- AGENTS.md vs. task queue: AGENTS.md is persistent context,
  task queue is session-specific work. From OpenAI: "Maintain
  an AGENTS.md to help Codex operate more effectively in your
  repo across prompts."
- File discovery: global (~/.codex/AGENTS.md), project root
  (./AGENTS.md), subdirectory files loaded when Codex works
  in those directories
- Checking it works: if Codex makes the same mistake twice,
  add the fix to AGENTS.md

**Worked example:** Seth writes his first AGENTS.md for a
project. Five entries. The next session, Codex uses the
conventions without being told.

<!-- → [TABLE: AGENTS.md include/exclude — two columns. Include:
bash commands, code style deviations, test runners, architectural
decisions, quirks. Exclude: what Codex can figure out, standard
conventions, changing content. No color.] -->

**Assessable exercises:**
1. (Apply) Write an AGENTS.md for a current project. Keep it
   under 200 lines.
2. (Analyze) Run Codex without your AGENTS.md, then with it,
   on the same task. Document three specific differences.
3. (Evaluate) After one week of use: which entries is Codex
   ignoring? Why? Fix one.

**Wayback Machine:** 🕰️ **Donald Knuth** (born 1938) — computer
scientist who invented literate programming: writing the
explanation of a program alongside the code, so both the human
and the machine are served simultaneously. AGENTS.md is literate
programming applied to AI collaboration.

**Bridge:** The reader has AGENTS.md. Chapter 7 teaches them
to formulate problems before Codex sees them.

---

### Chapter 7 — Problem Formulation: The Mission Before the Build

**One-line:** The most expensive mistake in an AI-assisted build
happens before the first prompt. Formulate the problem first.

**Learning outcomes:**
1. (Understand) Explain why problem formulation is the most
   important step in a conducted build.
2. (Apply) Use Ask Mode to interrogate a problem before writing
   a single specification prompt.
3. (Analyze) Identify the sections of a problem brief most
   likely to reveal a formulation gap.

**Opening:** Seth starts a build. Three hours in, he realizes
he is building the wrong thing. Not the wrong code — the wrong
system. The decision he needed to make was at the beginning,
not the middle. From OpenAI: "When kicking off a new feature,
engineers use Codex to scaffold boilerplate — but first they
specify what they want."

<!-- → [DIAGRAM: The problem formulation gate — one sentence that
names the thing being built, where it sits, and what it produces.
Below the gate: specification prompts. Above the gate: nothing.
Editorial style.] -->

**Core content:**
- The one-sentence formulation: what are you building, where
  does it sit, what does it produce? These are different
  questions from "what problem exists."
- Using Ask Mode for interrogation before specification:
  ask Codex questions about the problem space before asking
  it to build anything. "What would need to change in this
  codebase to support X?" is an Ask Mode question.
- The minimum viable spec: problem statement, architecture
  principles, core user flows, user needs — the five sections
  that matter most at student scale
- The planning gate: nothing goes to Code Mode until the
  Ask Mode plan has been reviewed and the problem statement
  has passed the one-sentence test
- What the spec unlocks: a Code Mode execution that is
  actually trustworthy

**Worked example:** A complete minimum viable spec for a
student project — a task tracker with a specific user, a
specific workflow, and three architecture principles.

<!-- → [TABLE: Weak vs. strong problem statements — two columns.
Five examples. Left: weak ("improve the app," "fix the bug").
Right: strong (specific system, specific location, specific
output). No color.] -->

**Assessable exercises:**
1. (Apply) Write a one-sentence problem formulation for a
   project you want to build. It must name the thing, the
   location, and the output.
2. (Apply) Use Ask Mode on the same project. What did Codex
   surface that you hadn't considered?
3. (Analyze) A provided spec has a weak needs section.
   Identify which needs are feature descriptions and rewrite
   them as testable outcomes.

**Wayback Machine:** 🕰️ **Frederick Brooks** (1931–2022) — author
of *The Mythical Man-Month* (1975), who established that the
most expensive bugs come from building the wrong thing.
His principle that design precedes implementation is the
Ask Mode gate stated as a founding principle of software
engineering.

**Bridge:** The reader has a problem formulation. Chapter 8
teaches them to write Codex prompts that are specifications,
not requests.

---

### Chapter 8 — Writing Codex Prompts That Are Specifications

**One-line:** "Write me a login function" is not a prompt.
A prompt names the thing, the invariants, the output format,
and what not to touch.

**Learning outcomes:**
1. (Understand) Distinguish a prompt (request) from a
   specification (complete task definition).
2. (Apply) Rewrite a weak prompt as a complete specification
   using the five-element format.
3. (Analyze) Identify what is missing from a set of provided
   prompts that would cause Codex to produce incorrect output.

**Opening:** Side by side: the same task written as a prompt
and as a specification. Codex's output for each. The difference
is not Codex — it's the precision of the instruction.

<!-- → [TABLE: Prompt vs. specification — two columns, five rows.
Each row: one element. Left: weak prompt version. Right:
specification version.] -->

**Core content:**
- The five elements of a specification prompt:
  1. The specific task (the one thing, not "help me with X")
  2. The invariants (what must not change)
  3. The context (AGENTS.md sections governing this step)
  4. The output format (what done looks like)
  5. The negative constraint (what Codex must not do)
- Why Codex has no memory between sessions and what this means
- The dangerous middle: prompts that are almost specifications
- Prompt quality self-check: five questions before sending
- From OpenAI: "Structure your prompt as if you are writing a
  GitHub issue — include file paths, component names, diffs,
  and doc snippets when relevant."
- Give Codex a way to verify its own work: include a test
  suite or bash command Codex can run to check output before
  reporting done. Codex iterating against a feedback mechanism
  produces significantly better results than single-pass
  generation.

**Worked example:** A complete build sequence of five prompts,
each written as a full specification with all five elements.

**Assessable exercises:**
1. (Apply) Take three prompts from your recent Codex history.
   Rewrite each as a specification.
2. (Analyze) A provided specification is missing two elements.
   Identify which ones and explain what Codex will do wrong.
3. (Create) Write a complete five-element specification for the
   next step in your current project.

**Wayback Machine:** 🕰️ **Ada Lovelace** (1815–1852) — the first
person to write what we would recognize as a computer program:
a precise specification of operations in dependency order,
written before any machine existed to run them. Her Notes on
Babbage's Analytical Engine are the conceptual ancestor of the
five-element specification format.

**Bridge:** The reader can write specifications. Chapter 9
addresses what happens when the specification is right and the
output is still wrong.

---

### Chapter 9 — Handoff Conditions and the Dangerous Middle

**One-line:** Not "looks good." A specific, testable condition
that must be true before the next step begins.

**Learning outcomes:**
1. (Understand) Explain what a handoff condition is and why
   "looks good" fails as one.
2. (Apply) Write handoff conditions for a set of provided
   Codex tasks.
3. (Analyze) Identify the dangerous middle — tasks where
   Codex's output requires specific verification that isn't
   in the obvious checklist.

**Opening:** Seth approves a Codex output. It looks correct.
It compiles. It passes every test he thought to write. Six days
later: silent failure. The condition that wasn't met was the
one he didn't know to check. That's the dangerous middle.

<!-- → [DIAGRAM: The handoff condition as a gate between build
steps. Step N → [Handoff condition check] → Step N+1. If check
fails: rewind to step N and respecify. Editorial style.] -->

**Core content:**
- What a handoff condition is: specific, testable, binary
- The dangerous middle: tasks where Codex produces plausible
  but domain-incorrect output that passes surface checks
- The scope creep prompt: "while I'm here" improvements that
  break things. From OpenAI: "Avoid excessive looping or
  repetition; if you find Codex re-reading or re-editing the
  same files without clear progress, stop and reframe."
- How to write a handoff condition that catches what you'd
  otherwise miss
- When output fails the handoff condition: revert to the
  previous state and respecify. Do not correct forward —
  forward correction pollutes the context window. After two
  failed corrections on the same step, start the task fresh
  with a better specification.
- Best-of-N as a verification tool: when uncertain, generate
  multiple Codex responses and evaluate all of them. The
  judgment that selects is irreducibly human.

<!-- → [TABLE: Strong vs. weak handoff conditions. Five examples.
Left: weak. Right: strong. No color.] -->

**Worked example:** Three handoff conditions analyzed — one
strong, one weak, one that missed the dangerous middle.

**Assessable exercises:**
1. (Apply) Write handoff conditions for five Codex tasks in a
   provided build sequence.
2. (Analyze) A provided build transcript shows Codex crossing
   into the dangerous middle. Identify the exact moment and
   the handoff condition that would have caught it.
3. (Create) Apply Best-of-N to a step in your current project.
   Generate three Codex responses. Document which you chose
   and why — the reasoning is the exercise.

**Wayback Machine:** 🕰️ **Grace Hopper** (1906–1992) — computer
scientist who insisted that "the most dangerous phrase in the
language is 'we've always done it this way'" and who first
formalized verification criteria in software. Her insistence
on explicit correctness criteria is the handoff condition
principle stated at the founding of software engineering.

**Bridge:** The reader has the full framework for code builds.
Chapter 10 applies the same discipline to creative work.

---

### Chapter 10 — Brutalist: When the Build Is Creative

**One-line:** The technical barrier is now low enough that any
student can produce ambitious creative work. The question is
whether the creative judgment stays theirs.

**Learning outcomes:**
1. (Understand) Explain how the fluency trap manifests in
   creative AI use.
2. (Apply) Apply the Brutalist three-file system (AGENTS.md,
   DESIGN.md, PROJECT.md) to a creative project.
3. (Analyze) Distinguish creative judgment (irreducibly human)
   from technical execution (Codex's domain) in a provided
   creative build.

**Opening:** Nicholas's observation: his classmates let AI
generate the prose, the graphics, the music. The output is
polished. It has no soul. He can feel the void underneath it.

<!-- → [DIAGRAM: The three-file system as three nested layers.
Outer: AGENTS.md (technical constitution). Middle: DESIGN.md
(visual constitution). Inner: PROJECT.md (project state —
Intent Layer is human, always). Editorial style.] -->

**Core content:**
- Why creative work has the same problem as code: the fluency
  trap is about the feeling of authorship, not the domain
- The Brutalist governing principle: maximally informed,
  minimally autonomous, by design
- The three files: AGENTS.md (what Codex never improvises),
  DESIGN.md (every aesthetic decision specified or escalated),
  PROJECT.md (Intent Layer is human, always)
- Refusal behavior: the system says no when Codex is asked to
  make a creative judgment. This is the feature.
- Nicholas's principle: AI handles technical execution, you
  keep creative judgment

<!-- → [TABLE: Labor separation in creative builds — two columns.
Codex handles / Human keeps. Examples from visual, writing,
and music domains. No color.] -->

**Worked example:** A student data visualization project built
twice — once with Codex running unattended on aesthetic
decisions, once with the Brutalist system. Same Codex. Same
technical output. Different authorship.

**Assessable exercises:**
1. (Apply) Create a DESIGN.md for a creative project. Name
   six design decisions that are fully specified and two that
   are escalated to you.
2. (Analyze) A provided creative build transcript shows Codex
   making an aesthetic judgment. Identify the moment.
3. (Create) Write the Intent Layer of a PROJECT.md for a
   creative project. Have a classmate read it. Can they tell
   what you are trying to make?

**Links:** [brutalist.art](https://brutalist.art)

**Wayback Machine:** 🕰️ **Sol LeWitt** (1928–2007) — conceptual
artist who created works defined by written instructions that
others executed. His argument that the person who holds the
intent and writes the instruction is the author, regardless of
who executes, is the Brutalist principle stated fifty years
before AI made it urgent.

**Bridge:** The reader has the full discipline. Chapter 11 is
the planning phase of their first complete build.

---

## ACT THREE — THE BUILD
*Chapters 11–14: From framework to first fully conducted build*

---

### Chapter 11 — Planning Your First Conducted Build

**One-line:** Before Codex enters Code Mode, you know exactly
what you are building, why, and which steps belong to you.

**Learning outcomes:**
1. (Apply) Complete a minimum viable spec for a student-scale
   project using Ask Mode interrogation.
2. (Apply) Generate an execution plan using Ask Mode and
   review it before switching to Code Mode.
3. (Analyze) Identify the three steps in the build most likely
   to hit the dangerous middle.

**Opening:** Seth planning his first fully conducted build. The
Ask Mode interrogation. The spec. The plan reviewed and
approved. The moment he realizes: he is not thinking about
Codex yet. He is thinking about the problem. That is the
discipline working.

<!-- → [DIAGRAM: The planning sequence — Ask Mode interrogation
→ Problem formulation → Spec → Ask Mode plan → Review and
approve → Code Mode execution. Phase gates labeled.
Editorial style.] -->

**Core content:**
- The planning sequence: Ask Mode interrogation → one-sentence
  formulation → minimum viable spec → Ask Mode plan → review
  and approve → Code Mode
- How to scope a student project for a first conducted build
- Reading a Codex Ask Mode plan: what to check, what to
  correct, what constitutes approval
- What to do when the Ask Mode plan reveals a thin spec section
- The planning gate: what must be true before Code Mode begins
- Task queue as a planning tool: fire off Ask Mode tasks to
  capture parallel ideas without losing the main build's focus

**Worked example:** Seth's complete planning document for a
real project — one-sentence formulation, abbreviated spec, and
Ask Mode plan for Phase 1. Students see the full artifact.

<!-- → [TABLE: Ask Mode plan evaluation — what to check.
Columns: plan element, what strong looks like, what weak looks
like, what to do when weak. Five rows.] -->

**Assessable exercises:**
1. (Apply) Produce a one-sentence formulation, minimum viable
   spec, and Ask Mode plan for the project you will build in
   Chapter 12.
2. (Analyze) Review the provided Ask Mode plan and identify
   the two highest-risk steps. Write handoff conditions.
3. (Evaluate) Is your spec ready to govern a build? What is
   the weakest section? Fix it before Chapter 12.

**Wayback Machine:** 🕰️ **Christopher Alexander** (1936–2022) —
architect whose *A Pattern Language* (1977) argued that good
design begins with a clear statement of the problem before any
solution is attempted. His principle that design starts with
what the inhabitant needs is the Ask Mode interrogation applied
to architecture.

**Bridge:** The plan is complete and approved. Chapter 12
executes it.

---

### Chapter 12 — Running the Build: Codex Tasks and Human Tasks

**One-line:** The plan is approved. Now you execute it in Code
Mode — one step at a time, with explicit handoff conditions
between every step.

**Learning outcomes:**
1. (Apply) Execute a conducted build sequence in Code Mode,
   completing Codex tasks and human tasks in dependency order.
2. (Apply) Apply each of the five supervisory capacities at
   the steps requiring them.
3. (Analyze) Identify when a build is going off-script and
   stop before it breaks.

**Opening:** Seth mid-build. The plan is approved. Code Mode
is running. He is doing his job — not approving output, but
evaluating it against the handoff condition. He rejects one
output. Respecifies. The revision passes. The build continues.
This is what conducting feels like.

**Core content:**
- Running Code Mode: executing against the approved plan, one
  step at a time
- Human tasks labeled by supervisory capacity at each step
- What to do when Codex fails a handoff condition: revert to
  previous state and respecify. Do not correct forward.
- What to do when output passes but feels wrong: plausibility
  auditing — trust the feeling, investigate it
- The scope creep moment: Codex suggests "while I'm here"
  improvements. Log it in the task queue. Do not execute.
- Give Codex a way to verify its own work: tests, linters,
  bash commands Codex can run. From OpenAI: "When Codex has
  some way to check its work, it can iterate — and that
  iteration produces dramatically better results."
- After two failed corrections: stop, start the task fresh
  with a better specification. Don't accumulate failed
  approaches in the session.
- Best-of-N when uncertain: generate multiple responses,
  evaluate all, select. The selection is the human's work.

**Worked example:** A complete build session — Seth's actual
prompts, Codex's actual outputs, Seth's handoff evaluations,
one rejection and revision, the final accepted output.

<!-- → [DIAGRAM: The build loop. Specification → Codex executes
in Code Mode → Handoff condition check → Pass: next step /
Fail: revert and respecify. Supervisory capacity label at the
check step.] -->

**Assessable exercises:**
1. (Apply) Execute Phase 1 of your conducted build. Document
   each handoff evaluation.
2. (Analyze) At least one Codex output will require revision.
   Document the failure and the revision specification.
3. (Evaluate) At the end of Phase 1: what did you learn?
   What would you change in the spec?

**Wayback Machine:** 🕰️ **W. Edwards Deming** (1900–1993) —
statistician whose Plan-Do-Check-Act cycle became the foundation
of quality management. His argument that quality is built into
a process through verification at every step is the handoff
condition principle applied to manufacturing.

**Bridge:** The build is done when it passes the handoff
conditions. Chapter 13 defines what "done" actually means.

---

### Chapter 13 — Verification: How You Know It Works

**One-line:** The build is done when it passes the handoff
conditions — not when Codex says it's done.

**Learning outcomes:**
1. (Apply) Run a structured verification pass on a completed
   build using explicit criteria from the spec.
2. (Analyze) Distinguish build failures from test quality gaps.
3. (Evaluate) Produce a post-build assessment.

**Opening:** Seth's build passes all his tests. He is about to
declare it done. He runs one more check — the one he almost
skipped. It fails. The failure is not a bug in the code. It's
a gap in the original spec.

<!-- → [DIAGRAM: The verification sequence — three passes.
Pass 1: functional verification. Pass 2: edge case verification.
Pass 3: spec needs verification. Binary result at each.
Resolution path if any check fails.] -->

**Core content:**
- Verification against spec needs, not just "does it run"
- The verification sequence: functional, edge case, spec needs
- Test quality vs. build quality: a test that passes because
  it's a bad test is not a passing test
- The post-build learning document: the most important output
  of the build
- From OpenAI's internal use doc: "I point Codex at low-
  coverage modules overnight and wake up to runnable unit-test
  PRs" — but the test quality check is still the human's job

**Worked example:** Seth's verification pass on Phase 1 —
three passes, one failure, a fix, and the post-build document.

**Assessable exercises:**
1. (Apply) Run a structured verification pass on your
   Chapter 12 build. Document each check and its result.
2. (Analyze) One of your tests passes but you're not sure
   it's testing the right thing. Diagnose and rewrite the test.
3. (Create) Write a post-build learning document for your
   Chapter 12 build.

**Wayback Machine:** 🕰️ **Barbara Liskov** (born 1939) —
computer scientist who formalized the behavioral contract:
"correct" must be defined before it can be verified. Her
contribution to formal specification is the spec needs
verification pass stated as a principle.

**Bridge:** The reader has the discipline. Chapter 14 hands
them the build.

---

### Chapter 14 — Your First Full Build: From Problem to Verified Output

**One-line:** You have the discipline. Here is the project.
Run it.

**Learning outcomes:**
1. (Create) Plan, execute, and verify a student-scale project
   using the complete conducting framework.
2. (Evaluate) Assess their own build against the five
   supervisory capacities.
3. (Create) Produce a post-build document.

**Opening:** Not Seth's build. Yours. The chapter gives the
student the project brief, the tools, and the sequence.
Everything else is their decision.

**Core content:**
- The project brief: a student-scale project requiring all
  five supervisory capacities
- The complete sequence: Ask Mode interrogation → one-sentence
  formulation → spec → Ask Mode plan → review and approve →
  Code Mode build → verification → post-build document
- What success looks like: a build where you can account for
  every decision
- Where to go next: the Irreducibly Human series, OpenAI's
  documentation, the next project

**Terminal deliverable:** The reader's first fully conducted
build — planned with a spec, executed with an approved Ask Mode
plan, verified against spec needs, documented in a post-build
learning record.

<!-- → [DIAGRAM: The complete arc — minimal timeline. Four
milestones: Problem named / Discipline learned / First build
planned / First build verified. Editorial style.] -->

**Closing:**
"You built it. You know what every line does. You know why
every decision was made. You know what you would do differently.
That is what learning looks like. That is what Codex is for."

**Assessable exercises:**
1. (Create) Complete your full conducted build. Submit the
   post-build learning document alongside the code.
2. (Evaluate) Return to the paragraph you wrote in Chapter 0
   exercise 2. Rewrite it now. What changed?
3. (Evaluate) Which of the five supervisory capacities was
   hardest to exercise consistently? Design a practice
   exercise targeting that capacity.

**Wayback Machine:** 🕰️ **John Dewey** (1859–1952) — philosopher
of education who argued that learning is not the transmission
of information but the transformation of the learner through
purposeful experience. His concept of "learning by doing" is
the post-build learning document — the record that the
experience changed the person, not just the repository.

---

# PART 9 — CHAPTER ANATOMY TEMPLATE

All 14 chapters follow this structure. The Cowork enrichment
pass processes items marked with comments.

1. **One-line descriptor** (capability build, not topic)
2. **Learning outcomes** (3–5, Bloom's level labeled)
3. **Chapter opening** (failure case or Seth narrative —
   problem before framework)
4. **Figure comments** — `<!-- → [DIAGRAM: ... ] -->` or
   `<!-- → [TABLE: ... ] -->` embedded at point of use
5. **Core content sections** (4–6): concept → example →
   application
6. **Worked example** (Seth's story or provided transcript;
   Act Three: real build session)
7. **Assessable exercises** (minimum 3; at least one at Apply
   or above; at least one requiring production)
8. **Links** (where applicable: brutalist.art, irreducibly.xyz,
   platform.openai.com/docs)
9. **AI Wayback Machine** — `## 🕰️ AI Wayback Machine` — one
   pre-2000 figure per chapter, connected to chapter's
   intellectual substance
10. **Bridge** (one sentence; raises what next chapter answers)

**Enforcement:** A draft chapter missing items 4 (figures),
7 (exercises), 9 (Wayback Machine), or 10 (bridge) is an
incomplete draft.

**Seth's voice rule:** Chapters 0–3 and 11–12 are primarily
Seth's narrative voice. Chapters 4–10 are framework-forward
with Seth as illustration. Chapters 13–14 transition to the
reader as primary actor.

---

# PART 10 — CASE STUDY STRATEGY

## Domain coverage map

| Domain | Chapters | Primary use |
|---|---|---|
| AP Computer Science / homework | 0, 1, 3 | Seth's observation |
| Software function / login system | 4, 8 | Prompt vs. specification |
| Task tracker / student project | 7, 11 | Spec and planning |
| Data visualization | 10 | Brutalist creative build |
| Generic student build | 12, 13, 14 | Full build arc |
| Creative work | 10 | Nicholas angle |

## Case escalation

Act One: observation only — Seth watching, no build.
Act Two: single-concept worked examples — one tool applied
to one scenario.
Act Three: Seth's real build session — multi-step, real
prompts, real Codex outputs, one documented failure and
revision.

## Sourcing requirement

Every factual claim (Bastani finding, Kosmyna EEG, Anthropic
RCT numbers) requires a citation. OpenAI internal use doc
quotes are attributed. Seth's narrative is explicitly labeled.

---

# PART 11 — HARD TOPICS, CONTESTED CLAIMS, AGING RISK

## Contested claims

| Claim | Status | Book's position |
|---|---|---|
| AI use always reduces learning | Disputed | "Without the conducting discipline" — the RCT shows this specifically for unstructured delegation |
| Five supervisory capacities are permanent | Emerging dispute | "Currently requires human judgment" — "currently" qualifier throughout |
| Ask Mode / Code Mode gate prevents all errors | Overstated | The gate reduces errors; the human supervisory layer is still required |
| Codex is appropriate for all student builds | Platform-specific | Framework applies to any agentic coding tool; Codex is current implementation |

## Hard chapters

**Chapter 9 (Dangerous Middle):** Must produce genuine
discomfort. The worked example where the failure is specific
and the missed handoff condition is non-obvious requires
a real Codex session, not a hypothetical.

**Chapter 1 (Homework/Quiz Gap):** The empirical foundation.
The Bastani finding must be stated precisely. Cohen's d and
p-value must be present.

## Aging risk

| Content type | Risk | Review cadence |
|---|---|---|
| Codex feature references (Ask Mode, Code Mode, Best-of-N) | High | Before each edition |
| AGENTS.md syntax | High | Before each edition |
| Bastani / Kosmyna / Anthropic RCT numbers | Low-Medium | On new major studies |
| Five supervisory capacities | Low | On major framework revision |
| Brutalist three-file system | Medium | On major tool change |
| Seth's narrative | None | N/A |

**Aging mitigation:** The book teaches the discipline, not the
feature set. All Codex feature references are in Appendix A,
updatable without touching main text.

---

# PART 12 — MARKET POSITIONING SUMMARY

## The gap this book fills

No book takes a technically fluent student seriously, hands
them a production discipline, and is co-authored by one of
them — for the Codex ecosystem specifically. The Ask Mode /
Code Mode gate, AGENTS.md, and Best-of-N are Codex-native
mechanisms that the Claude Code book cannot teach. This book
serves the OpenAI-native student reader.

## Market size estimate

Primary: technically fluent high school and early university
students using Codex via ChatGPT Plus/Pro or the API.
Secondary: K-12 CS teachers assigning it alongside the
teachers companion.
Kindle price ($1) removes the adoption barrier entirely.

---

# PART 13 — FEATURE LIST

| Feature | Priority | Production effort |
|---|---|---|
| 14-chapter architecture | ESSENTIAL | Low |
| Three-act learning arc | ESSENTIAL | Low |
| Seth's co-authored narrative | ESSENTIAL | Medium |
| Five supervisory capacities framework | ESSENTIAL | Low |
| Ask Mode / Code Mode gate | ESSENTIAL | Low |
| AGENTS.md full chapter treatment | ESSENTIAL | Medium |
| Assessable exercises (3+ per chapter) | ESSENTIAL | Medium |
| Terminal deliverable (post-build document) | ESSENTIAL | Low |
| AI Wayback Machine (14 figures) | IMPORTANT | Low |
| Worked examples (Seth's real builds) | IMPORTANT | High |
| Figure comments for Cowork enrichment | IMPORTANT | Medium |
| Brutalist three-file system chapter | IMPORTANT | Medium |
| Appendix A (Codex quick reference) | IMPORTANT | Low |
| Appendix B (Brutalist quick-start) | IMPORTANT | Low |
| Appendix C (post-build document template) | IMPORTANT | Low |
| Medhavy integration | VALUABLE | High |

---

# PART 14 — OUT OF SCOPE

| Topic | Reason | Covered better in |
|---|---|---|
| Generic prompt engineering tips | Optimizes for output, not capability | Any prompt engineering guide |
| AI ethics and academic integrity policy | Treats students as problems | codex-for-teachers companion |
| Full Codex API and advanced orchestration | Developer scope | platform.openai.com/docs |
| Advanced agent pipelines / AGENTS.override.md | Power user pattern | Advanced follow-on |
| Walker (Unity refactoring) | Second edition | Future edition |
| Server-side deployment | Too complex for first build | Advanced follow-on |
| Multi-agent coordination | Power user pattern | Advanced follow-on |

---

# PART 15 — ADOPTION RISK REGISTER

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| 1 | "Just tell me the prompts" reader | High | Medium | Seth's voice in Ch 0–1 must land; Bastani finding is the hook |
| 2 | Aging risk: Codex feature specifics | High | Medium | Discipline in main text; features in appendices; live URLs |
| 3 | School adoption barrier | Medium | Low | Primary reader self-directs; $1 Kindle; teachers companion bridges |
| 4 | Seth's co-authorship scope | Medium | Medium | Seth drafts Ch 0, narrative sections, real build logs |
| 5 | Ask Mode / Code Mode gate not yet stable | Medium | Medium | "Currently" qualifier; appendix updatable; framework is tool-agnostic |
| 6 | Codex access requires ChatGPT Plus/Pro | Medium | Medium | API access section in Appendix A; free tier workaround documented |

---

# PART 16 — OPEN QUESTIONS

| # | Question | Stakes | Decision deadline | Owner |
|---|---|---|---|---|
| 1 | What is Seth's specific terminal build project for Chapters 11–13? Real, reproducible. | Worked example quality | Before chapter drafting | Seth + Author |
| 2 | Does the book include a free-tier access appendix for students without ChatGPT Plus? | Accessibility | Before production | Author |
| 3 | Seth's co-authorship credit: cover attribution and royalty structure? | Legal; contract | Before contract | Author + Publisher |
| 4 | Nicholas's feedback credit: acknowledgments or contributor credit? | Attribution | Before manuscript | Author + Nicholas |
| 5 | Should Appendix A (Codex quick reference) be maintained on a web page rather than in the book? | Aging risk | Before production | Author |
| 6 | Best-of-N feature availability on student-accessible plans? Confirm before Chapter 9. | Accessibility | Before Ch 9 drafting | Author |

---

## Appendix A — Codex Quick Reference for Students

| Feature | What it does | When to use it |
|---|---|---|
| Ask Mode | Codex reads and plans without executing. Returns plans and answers. | Before any Code Mode execution. Problem interrogation. |
| Code Mode | Codex executes. Writes files, runs commands, iterates. | After Ask Mode plan is reviewed and approved. |
| Best-of-N | Generates N parallel responses for the same task. | When uncertain which approach is best. Selection is the human's work. |
| Task queue | Stores tasks for background or later execution. | Capturing tangential ideas, parallel work, deferred fixes. |
| AGENTS.md | Persistent context file read at every session start. | Always. Write it before the first build session. |

Full documentation: [platform.openai.com/docs](https://platform.openai.com/docs)

---

## Appendix B — Brutalist Quick-Start for Student Creative Projects

Three-file setup in under an hour:
- AGENTS.md template for student creative projects
- DESIGN.md template for student visual projects
- PROJECT.md template with Intent Layer prompts

Full documentation: [brutalist.art](https://brutalist.art)

---

## Appendix C — Post-Build Learning Document Template

Five sections. One document. Honest.

1. What I built (one paragraph, plain language)
2. What I delegated to Codex and why
3. What I kept for myself and why
4. What I learned that I didn't know before
5. What I would do differently

---

## Series Links

[brutalist.art](https://brutalist.art) ·
[frictional.xyz](https://frictional.xyz) ·
[irreducibly.xyz](https://irreducibly.xyz) ·
[nikbearbrown.com](https://nikbearbrown.com) ·
[platform.openai.com/docs](https://platform.openai.com/docs)

---

*Full TOC Draft v1.0 — compiled from all phase outputs*
*All phases complete: Vision (i1–i4), Learning Architecture
(l1–l4), Chapter Architecture (c1–c4), Scope & Market (m1–m4)*
*One blocker before production: OQ-1 (Seth's terminal build
project for Chapters 11–13)*
