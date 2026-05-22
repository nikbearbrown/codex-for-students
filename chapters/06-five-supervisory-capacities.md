# Chapter 5 — The Five Supervisory Capacities

> These are the five things you do that Codex cannot. Name them. Practice them. Never delegate them.

---

## Learning outcomes

1. **(Remember)** Name and define the five supervisory capacities.
2. **(Apply)** Identify which supervisory capacity is being exercised at each step of a provided build sequence.
3. **(Analyze)** Diagnose a build that went wrong by identifying which supervisory capacity was absent.

---

## Opening

Seth was mid-build on a small project. The CLI's output had passed its tests and was about to ship. Seth almost shipped it.

He stopped.

He could not point to what was wrong. The code compiled. The tests he had written passed. The output looked correct on the cases he had checked. Something felt off, in a way he could not articulate yet. He sat with the feeling for a minute. Then he could say it: the function was using a default parameter value that he knew, from another part of the project, was about to change in the next sprint. The function would work today and would silently produce wrong results next Tuesday when the default changed.

The feeling has a name. It is called **plausibility auditing** — the supervisory capacity of *hearing the wrong note before verification catches it*. The capacity is one of five that this chapter names. Together, the five are the work the conducting discipline keeps yours.

<!-- → [DIAGRAM: The five supervisory capacities as a pentagon or five-column layout. Each: label (PA, PF, TO, IJ, EI), plain-language name, one-sentence definition. Editorial style. No color.] -->

---

## Why name them

You already do supervisory work, even without the framework. Every time you stop before accepting a Codex output and think "wait" — that is supervisory work. Every time you reword a prompt because the first one was vague — that is supervisory work. Every time you decide to write a function by hand because the operation is too consequential to delegate — that is supervisory work.

The point of the framework is not to introduce something you do not do. It is to *name* what you already do, so you can deploy it deliberately and so you can diagnose what went wrong when something breaks.

A build that goes wrong is not a mystery. It is one of five named failures. The post-mortem becomes structured: *which capacity was absent at which step?* Naming makes recovery operational.

The capacities have two-letter abbreviations because you will use them in build logs. The book uses the same five across the series; the abbreviations are interoperable between the *Codex*, *Claude Code*, and *GitHub Copilot CLI* student books.

---

## [PA] Plausibility Auditing

**The capacity to hear the wrong note before verification catches it.**

The feeling Seth had in the chapter opening was PA. The output was fluent. The handoff conditions he had written would have passed. Something matched what he knew about his situation poorly enough that he stopped before shipping.

PA catches the dangerous middle. Codex generates output calibrated for *accuracy against the patterns it learned*. The accuracy is real but partial. The match against *your specific situation* is yours to check. PA is what fires when the fluent output and your knowledge of the situation diverge.

PA in Codex work:

> The function compiles, the tests pass, the output on three sample inputs looks correct. Something feels wrong about the way the function handles the empty-string case. PA fires: investigate before accepting.

PA requires that you have domain knowledge of the situation. The more you have done with the relevant code patterns, the more reliable your PA becomes. It is cumulative. PA is the supervisory capacity that grows the fastest with deliberate practice — every time you stop to investigate a feeling of off-ness, you exercise it.

---

## [PF] Problem Formulation

**The capacity to decide what the build IS before Codex sees it.**

Codex optimizes within the frame you give it. If the frame is wrong, the output is wrong, elegantly.

From the OpenAI internal use doc: *"When I'm in meetings all day, Codex works in the background — but I give it the direction first."*[^1] The direction is PF. Without it, Codex produces an answer to the wrong question.

PF in Codex work:

> Before you type a Code Mode prompt, you have decided: what does this function do, what does it take as input, what does it return, what does it not do, where does it live in the codebase. The decisions are yours; the framing is yours.

The Ask Mode interrogation from Chapter 4 is the operational tool for PF. *"What would I need to know about the existing code before adding this feature?"* is a PF question. The interrogation surfaces frames you had not considered.

PF is the most under-exercised capacity. The pressure to *get going* skips it. The student who pauses to formulate properly produces builds that work; the student who jumps to suggest produces builds that the rest of the framework spends time recovering from.

---

## [TO] Tool Orchestration

**The capacity to choose which Codex mode, which task, in what order, with what trust level.**

You have Ask Mode. You have Code Mode. You have Best-of-N (as a technique, per Chapter 4). You have AGENTS.md (Chapter 6) that constrains what Codex knows. You have the option of *not* using Codex and writing the code by hand.

TO is the scheduling work. *Which* tool for *this* step, *now*, with *what* context.

TO in Codex work:

> The step requires interrogating the existing code: Ask Mode. The step requires implementing a well-specified change: Code Mode. The step requires choosing between two reasonable approaches: Best-of-N. The step is too consequential or too short to benefit from Codex generation: write it by hand.

TO is the conducting metaphor's most literal expression. You are scheduling the orchestra. The decision of *which instrument plays now* is the conductor's work.

---

## [IJ] Interpretive Judgment

**The capacity to supply meaning Codex's output cannot supply.**

The explanation tells you what the code *does*. IJ tells you what the code *means* — in your project, for your goal, given the consequence horizon you care about.

IJ in Codex work:

> The generated function returns a list of dicts. The list looks correct. IJ fires: what does this list mean for the downstream summary report? Is the dict shape what the report expects? The output is technically correct; the *meaning* of the output, in the context of the larger build, is yours to verify.

IJ overlaps with PA but is distinct. PA fires *defensively* — something is off. IJ fires *constructively* — here is what the output means. PA notices the mismatch; IJ supplies the interpretation. In practice they often run together.

IJ requires the most domain knowledge of the capacities. The student who is shallow in the domain will exercise IJ less reliably. The student who has been working in the area for months has stronger IJ than the same student in week one. The discipline of *attending to* IJ — pausing to ask what the output means in context — is what builds the capacity over time.

---

## [EI] Executive Integration

**The capacity to hold the whole build toward a single goal across many prompts.**

A long Codex session has many prompts. Each is locally reasonable. Without EI, you finish the session and find that the cumulative result violates a constraint you set thirty prompts ago.

EI in Codex work:

> Three prompts ago you agreed with yourself that the grading tool would never generate a final grade. The current Code Mode output is generating grades. EI fires: stop. The constraint from three prompts ago is being violated. Revert this prompt; respecify.

EI is the capacity that catches drift. It requires holding the project's constraints across the duration of the session. It is exercised across sessions too, with AGENTS.md (Chapter 6) as the persistent record — the constraint that Codex cannot remember between sessions is the constraint EI will catch this session, *if* the constraint is in AGENTS.md.

EI is the integration check. It is what makes the build coherent.

---

## The capacities in motion: a worked sequence

A small feature build, with capacities labeled at each step.

| Step | Action | Capacity |
|------|--------|----------|
| 1 | Decide what the feature is (one-sentence formulation) | **PF** |
| 2 | Open Ask Mode; ask Codex to read the relevant files | **TO** |
| 3 | Read the Ask Mode summary; notice a convention I forgot | **PA** + **IJ** |
| 4 | Update my formulation to respect the convention | **PF** revisit |
| 5 | Ask Mode plan for the implementation | **TO** |
| 6 | Read the plan; correct one assumption Codex made | **IJ** |
| 7 | Switch to Code Mode | **TO** |
| 8 | Codex implements; I read the output | (no capacity active yet) |
| 9 | Output looks correct; tests pass; something feels off about edge case | **PA** |
| 10 | Investigate; find Codex is using a default that conflicts with the convention | **IJ** + **PA** |
| 11 | Update Code Mode prompt with explicit constraint; rerun | **PF** revisit |
| 12 | Output now correct; verify handoff conditions | (verification per Chapter 9) |
| 13 | Update AGENTS.md with the lesson about the convention | **EI** |

Thirteen steps. All five capacities fired. The build is a working feature. The discipline is naming the capacities as they fire, in build logs you keep for yourself.

---

## Diagnosing a build that went wrong

The capacities are diagnostic. When a build produces a bad outcome, ask: *which capacity was absent?*

- **Output is fluent but wrong, no warning signs.** PA was absent (or fired and was overridden).
- **The whole build solved the wrong problem.** PF was absent at step zero.
- **Wrong tool at the wrong time** (Code Mode when Ask Mode should have run; writing by hand when Codex would have been faster). TO was absent.
- **Output is technically correct but means the wrong thing in context.** IJ was absent.
- **Final result violates a constraint you set earlier.** EI was absent.

The framework is most useful in post-mortems. When a build breaks, the question is not "what should I have done?" but "which of the five was absent, where, and what would the build look like if it had fired?" The structured question makes the next build better.

---

## Common misconceptions

**"I have to consciously run through all five every prompt."** No. Most prompts exercise one or two. PF dominates at the start. TO fires at tool-choice. PA fires on output review. IJ fires on revision. EI fires when drift threatens.

**"With practice they fuse into a single 'judgment.'"** Yes — and you should let that happen. The named decomposition is for the early period when you are learning to recognize what you are doing. Later the names recede; the practice remains.

**"PA = paranoia."** No. Vigilance, not anxiety. PA is *attending to* the off-feeling, not the practice of suspecting all output.

**"These are abstract; I want concrete rules."** The five are the abstraction *under* the concrete rules. The concrete rules (run Ask Mode before Code Mode; review the plan before approving; write handoff conditions before steps) are how the capacities show up at the surface. Knowing the capacities lets you write new concrete rules for situations the book did not anticipate.

**"The capacities will be obsolete when Codex gets better."** Possibly in domains the CLI can fully reach. The capacities that require knowledge Codex does not have (PA against domain knowledge, PF against intent, IJ against project context, EI across persistent constraints) will remain irreducibly yours for as long as Codex does not have access to your project, your goals, and your history. None is on the immediate horizon.

---

## Exercises

1. **(Apply)** Take your most recent significant Codex session. Label the capacity exercised at each step. Be honest about cases where no capacity fired.

2. **(Analyze)** A build transcript is provided where one capacity is systematically missing. Identify which one and trace the consequences across the build.

3. **(Create)** Design a personal checklist for a current project using the five capacity labels. The checklist should be short (under ten lines) and usable in real time.

---

## What would change my mind

The chapter's structural claim is that **the five capacities are not closable by improvements to Codex itself** — that the supervisory work they describe requires knowledge Codex structurally does not have. If a 2027 or later model demonstrated reliable performance of *all* five capacities on its own — including PA on its own output (which would require some form of meta-cognition the current generation does not have) and EI across multiple sessions (which would require persistent context the current generation does not have) — the claim softens.

I do not expect this on the next-edition timeline. The structural arguments are about what information Codex has access to, and the gap is not narrowing on the immediate horizon.

---

## Still puzzling

- **The exact boundary between PA and IJ.** Both involve interpretation against domain knowledge. The book's distinction (PA defensive, IJ constructive) is operational but fuzzy in practice.

- **Whether the framework's five-capacity decomposition is the right number.** Other frameworks use three or seven. The book's five is the synthesis that has been useful in practice; whether it is *correct* is not a question with a clean answer.

- **Whether the capacities transfer between agentic-tool surfaces.** The book teaches them for Codex. The companion books teach the same five for Claude Code and GitHub Copilot CLI. Whether the capacities a student practices in one tool develop into the same capacities for another is an empirical claim the book makes but does not directly measure.

---

## AI Wayback Machine

🕰️ **Douglas Engelbart** (1925–2013) — engineer whose 1962 SRI report *Augmenting Human Intellect: A Conceptual Framework* established the modern intellectual project of computer-human partnership. Engelbart argued that the right question about computer technology was not *can it replace the human?* but *can it extend what the human can think and do?* His framework identified the cognitive capacities that augmentation should target and the capacities it should leave to the human.[^2] The five supervisory capacities in this chapter are Engelbart's framework applied to AI-assisted coding — a precise specification of which cognitive work the tool extends (pattern completion, code generation) and which it does not (plausibility auditing, problem formulation, tool orchestration, interpretive judgment, executive integration). Engelbart never wrote about Codex. His framework anticipated it.

---

## Bridge

You have the capacities. Chapter 6 introduces AGENTS.md — the file Codex reads at every session that makes the EI capacity scale across sessions and gives the discipline a persistent home.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Engelbart, D. C. *Augmenting Human Intellect: A Conceptual Framework*. SRI Project No. 3578 (Air Force Office of Scientific Research), 1962. Available via the Doug Engelbart Institute.
