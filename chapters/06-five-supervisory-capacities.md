# Chapter 6 — The Five Supervisory Capacities

*These are the five things you do that Codex cannot. Name them. Practice them. Never delegate them.*

---

Seth was mid-build on a small project. The code compiled. The tests passed. The output looked correct on every case he had checked. He was about to ship.

He stopped.

He could not say what was wrong. The function was fluent. His handoff conditions would have passed. Something matched his knowledge of the situation poorly enough that he did not push the button. He sat with the feeling for a minute. Then he could name it: the function was using a default parameter value that he knew, from another part of the project, was about to change next sprint. The function would work today. It would silently produce wrong results next Tuesday.

That feeling has a name. The chapter is about giving names to all five of them.

---

## Why Naming Matters

You already do supervisory work. Every time you stop before accepting a Codex output and think *wait* — that is supervisory work. Every time you reword a prompt because the first one was vague — that is supervisory work. Every time you decide to write a function by hand because the operation is too consequential to delegate — that is supervisory work.

The point of the framework is not to introduce something you do not do. It is to *name* what you already do, so you can deploy it deliberately, and so you can diagnose what went wrong when something breaks.

A build that goes wrong is not a mystery. It is one of five named failures. The post-mortem becomes structured: which capacity was absent at which step? Naming makes recovery operational.

There is a practical note on the abbreviations. The capacities have two-letter codes — PA, PF, TO, IJ, EI — because you will use them in build logs. The same five appear across the companion books for Claude Code and GitHub Copilot CLI. The abbreviations are interoperable. When you read someone else's annotated build log, you will be reading the same vocabulary.

<!-- → [DIAGRAM: The five supervisory capacities as a pentagon or five-column layout. Each entry: abbreviation (PA, PF, TO, IJ, EI), plain-language name, one-sentence definition. Editorial style, no color.] -->

---

## PA: Plausibility Auditing

The first capacity is the one Seth exercised in the opening. The name for it is **plausibility auditing**: the capacity to hear the wrong note before verification catches it.

Here is the structure of what PA does. Codex generates output calibrated for accuracy against the patterns it learned. The accuracy is real. The match against your specific situation is partial — because your situation is not in the training corpus, and the parameters of your project are not in the prompt. PA is what fires when fluent output and your knowledge of the situation diverge.

The feeling is real before you can articulate the content. Seth felt it before he could say *default parameter, next sprint*. The feeling is data. PA is the discipline of attending to the feeling rather than overriding it in the interest of shipping.

<!-- → [IMAGE: A simple two-panel illustration — left panel: Codex output that looks correct (green checkmarks, passing tests); right panel: the same output with one detail circled in amber, labeled "PA fires here." Caption: "PA catches the divergence the surface checks miss."] -->

What PA is not: paranoia. The practice is vigilance, not anxiety. You are not suspecting all output. You are attending to the off-feeling when it arises. In most prompts PA stays quiet. When it fires, you investigate.

PA is also the capacity that grows fastest with deliberate practice. Every time you stop to investigate an off-feeling and find something, the pattern recognition sharpens. Every time you stop and find nothing, you have practiced the discipline anyway. The cumulative effect is a more reliable early-warning system calibrated to your specific domain.

The dangerous middle from Chapter 3 — tasks that look like pattern work but require situational judgment — is exactly where PA earns its keep. The output passes the surface checks. PA is what catches the divergence beneath.

---

## PF: Problem Formulation

The second capacity runs upstream of everything else. **Problem formulation** is the capacity to decide what the build *is* before Codex sees it.

Codex optimizes within the frame you give it. If the frame is wrong, the output is wrong, elegantly. A well-specified problem produces a well-specified solution to the wrong question. The elegance of the output is no evidence that the problem was correctly formulated.

There is an observation from an OpenAI internal document on how their engineers actually use Codex: *"When I'm in meetings all day, Codex works in the background — but I give it the direction first."*[^1] The direction is PF. Without it, Codex is running unsupervised toward a destination you have not set.

What PF looks like in practice: before you type a Code Mode prompt, you have decided what the function does, what it takes as input, what it returns, what it does not do, and where it lives in the codebase. Not in Codex's mind — in yours. The Ask Mode interrogation from Chapter 4 is the operational tool for PF. *What would I need to know about the existing code before adding this feature?* is a PF question. The interrogation surfaces frames you had not considered before you commit to one.

<!-- → [INFOGRAPHIC: PF as a funnel — wide top labeled "vague intent" narrowing to a point labeled "well-specified prompt." Annotations on the funnel walls: "what does it do / what does it take / what does it return / what does it NOT do / where does it live." Shows what PF converts before Codex sees the prompt.] -->

PF is the most under-exercised of the five. The pressure to get going skips it. The student who pauses to formulate properly produces builds that work. The student who jumps to suggest produces builds that spend the next hour recovering. The time cost of PF is measured in minutes. The time cost of skipping it is measured in hours.

---

## TO: Tool Orchestration

The third capacity is the scheduling work. **Tool orchestration** is the capacity to choose which Codex mode, which task, in what order, with what trust level — and to know when not to use Codex at all.

You have Ask Mode. You have Code Mode. You have Best-of-N as a technique. You have AGENTS.md (the next chapter) as a way to constrain what Codex knows about your project. And you have the option of writing the code by hand, which is still sometimes the right answer.

TO is the conducting metaphor at its most literal. You are deciding which instrument plays now. Ask Mode for interrogation. Code Mode for implementation of a well-specified change. Best-of-N for choosing between two reasonable approaches. By hand for steps too consequential or too small to benefit from generation.

The mistake TO prevents is using Code Mode when Ask Mode should have run first. This is the most common sequencing error. The student generates code against a prompt, finds the code is architecturally wrong for the codebase, and has to redo it — not because the code was bad but because the interrogation step was skipped. Ask Mode would have surfaced the architectural constraint in thirty seconds. Code Mode without Ask Mode spent two hours to produce the same thirty seconds of information, buried in a function that has to be thrown away.

<!-- → [TABLE: TO decision guide — four rows: Ask Mode / Code Mode / Best-of-N / By hand. Two columns: "use when" and "common mistake." Designed as a quick reference for tool-choice junctions.] -->

TO also governs trust calibration. Not all Codex output deserves the same level of review. A utility function with clear inputs and outputs warrants less review than a function with side effects touching shared state. TO includes the judgment of which outputs you read carefully and which you accept with lighter review — and the discipline of not collapsing that judgment into "I trust Codex" across the board.

---

## IJ: Interpretive Judgment

The fourth capacity is where meaning enters. **Interpretive judgment** is the capacity to supply what Codex's output cannot supply: what the code means in the context of your project, your goal, and the consequence horizon you care about.

The distinction from PA is worth being precise about. PA fires *defensively* — something is off, investigate. IJ fires *constructively* — here is what this output means. PA notices the mismatch; IJ supplies the interpretation. In practice they often fire together, but they are doing different things.

An example. The generated function returns a list of dicts. The list looks correct. PA is quiet — nothing feels off. IJ fires anyway: what does this list mean for the downstream summary report? Is the dict shape what the report expects? The key names? The value types on edge cases? The output is technically correct. The *meaning* of the output, in the context of the larger build, is yours to verify.

IJ requires more domain knowledge than any of the other four capacities. The student who is shallow in the domain exercises IJ less reliably — not because the capacity is absent but because the interpretation depends on knowing what the output is supposed to mean, and that knowledge is domain-specific. The student who has been working in the area for months has stronger IJ than the same student in week one. The discipline of *attending to* the question *what does this mean in context* is what builds the capacity over time.

The word "modified" meaning different things to an OS and to a human — the example from Chapter 3 — is an IJ failure. The output was technically correct. The interpretation of "modified" in the student's context was not supplied. IJ is what supplies it.

---

## EI: Executive Integration

The fifth capacity operates at the longest range. **Executive integration** is the capacity to hold the whole build toward a single goal across many prompts, many sessions, many days.

A long Codex session has many prompts. Each is locally reasonable. Without EI, you finish the session and find that the cumulative result violates a constraint you set thirty prompts ago — because Codex does not remember the constraint, and you stopped checking against it.

An example. Three prompts ago you agreed with yourself that the grading tool would never generate a final grade. The current Code Mode output is generating grades. EI fires: stop. The constraint from three prompts ago is being violated. Revert the prompt; respecify.

EI is the integration check. It is what makes the build coherent across its duration. It is also the capacity that most benefits from an external artifact — because holding all constraints in working memory across a multi-hour session is not reliable. AGENTS.md, which the next chapter develops in full, is the persistent record for EI. The constraint that Codex cannot remember between sessions is the constraint EI will catch this session *if* it is in AGENTS.md. Without the file, EI depends entirely on your memory. With it, EI has a scaffold.

<!-- → [DIAGRAM: EI as a timeline — horizontal axis is session duration (many prompts left to right). A constraint set at prompt 3 is marked. Drift accumulates silently across prompts 4–12. EI fires at prompt 13 when the cumulative result violates the original constraint. Shows the long-range nature of EI vs. the prompt-level focus of PA and IJ.] -->

The failure mode EI prevents is drift. Not dramatic failure — the build doesn't crash — but quiet drift away from the original intent, one locally-reasonable prompt at a time, until the cumulative result is something you did not mean to build.

---

## The Capacities in Motion

Here is a small feature build with the capacities labeled at each step. I want you to see them in sequence, not as a list.

| Step | Action | Capacity |
|------|--------|----------|
| 1 | Decide what the feature is — one-sentence formulation | **PF** |
| 2 | Open Ask Mode; ask Codex to read the relevant files | **TO** |
| 3 | Read the Ask Mode summary; notice a convention I forgot | **PA** + **IJ** |
| 4 | Update my formulation to respect the convention | **PF** revisit |
| 5 | Ask Mode plan for the implementation | **TO** |
| 6 | Read the plan; correct one assumption Codex made | **IJ** |
| 7 | Switch to Code Mode | **TO** |
| 8 | Codex implements; I read the output | — |
| 9 | Output looks correct; tests pass; something feels off about an edge case | **PA** |
| 10 | Investigate; find Codex is using a default that conflicts with the convention | **IJ** + **PA** |
| 11 | Update Code Mode prompt with explicit constraint; rerun | **PF** revisit |
| 12 | Output now correct; verify handoff conditions | — |
| 13 | Update AGENTS.md with the lesson about the convention | **EI** |

Thirteen steps. All five capacities fired. Step 8 and step 12 have no capacity label — not every step is supervisory work. The generation itself is Codex's. The verification in step 12 is a mechanical check, not a supervisory judgment. The discipline is not about labeling everything; it is about knowing which steps require your attention and why.

<!-- → [TABLE: The worked sequence annotated — same thirteen rows, with a fifth column "Why this capacity, not another?" filling in the reasoning at each labeled step. Helps the reader internalize the distinctions rather than just seeing the labels.] -->

---

## Diagnosing What Went Wrong

The capacities are diagnostic. When a build produces a bad outcome, ask: which capacity was absent?

Output is fluent but wrong, and there were no warning signs — PA was absent, or fired and was overridden. The whole build solved the wrong problem — PF was absent at step zero. The wrong tool ran at the wrong time — TO was absent. The output is technically correct but means the wrong thing in context — IJ was absent. The final result violates a constraint set earlier — EI was absent.

The post-mortem is structured. Not *what should I have done* but *which of the five was absent, where, and what would the build have looked like if it had fired?* The structured question makes the next build better. The un-structured question produces guilt, not improvement.

<!-- → [TABLE: Diagnostic guide — two columns: symptom / absent capacity. Five rows. Designed as a reference card for post-mortems.] -->

---

## Three Things This Framework Is Not

**It is not a checklist to run through consciously at every prompt.** Most prompts exercise one or two capacities. PF dominates at the start. TO fires at tool-choice junctions. PA fires on output review. IJ fires on interpretation. EI fires when drift threatens. The decomposition is for the early period when you are learning to recognize what you are doing. Later the names recede; the practice remains.

**PA is not paranoia.** The practice is vigilance, not anxiety. Attending to the off-feeling is not the same as suspecting all output. In most prompts PA stays quiet.

**The capacities are not rendered obsolete by better models.** The capacities that require knowledge Codex does not have — PA against domain knowledge, PF against intent, IJ against project context, EI across persistent constraints — will remain irreducibly yours for as long as Codex does not have access to your project, your goals, and your history. That horizon is not close. The structural argument from Chapter 3 holds: better models handle the average case more gracefully; the cases specific to your situation are still yours to navigate.

---

## Exercises

**LLM Exercises**

1. **(Apply)** Take your most recent significant Codex session. Label the capacity exercised at each step. Be honest about steps where no capacity fired — those are data too.

2. **(Analyze)** A build transcript is provided where one capacity is systematically missing. Identify which one and trace the consequences across the build.

3. **(Create)** Design a personal checklist for a current project using the five capacity labels. Under ten lines. Usable in real time, not as a retrospective.

---

## What Would Change My Mind

The chapter's structural claim is that the five capacities are not closable by improvements to Codex itself — that the supervisory work they describe requires knowledge Codex structurally does not have.

If a future model demonstrated reliable performance of all five capacities on its own — including PA on its own output, which would require meta-cognition the current generation does not have, and EI across multiple sessions, which would require persistent context the current generation does not have — the claim softens. The chapter would still hold directionally but the framing would shift from structural to gradient.

I do not expect this on the next-edition timeline. The arguments are about what information Codex has access to, and the gap is not narrowing there.

---

## Still Puzzling

The exact boundary between PA and IJ. Both involve interpretation against domain knowledge. The book's distinction — PA defensive, IJ constructive — is operational but fuzzy in practice. There are steps where both are firing simultaneously and it is not clear which one caught the problem. Whether the right response is a finer decomposition or acceptance that two capacities can fire together on the same thing is an open question.

Also: whether the five-capacity decomposition is the right number. Other frameworks use three or seven. The book's five is the synthesis that has been useful in practice. Whether it is correct is not a question with a clean answer.

---

## AI Wayback Machine

🕰️ **Douglas Engelbart** (1925–2013) — whose 1962 SRI report *Augmenting Human Intellect: A Conceptual Framework* established the modern intellectual project of computer-human partnership. Engelbart argued that the right question about computer technology was not *can it replace the human* but *can it extend what the human can think and do?* His framework identified the cognitive capacities that augmentation should target and the capacities it should leave to the human.[^2] The five supervisory capacities in this chapter are Engelbart's framework applied to AI-assisted coding — a precise specification of which cognitive work the tool extends (pattern completion, code generation) and which it does not (plausibility auditing, problem formulation, tool orchestration, interpretive judgment, executive integration). Engelbart never wrote about Codex. His framework anticipated it.

---

## Bridge

You have the capacities. The next chapter introduces AGENTS.md — the file Codex reads at every session that makes EI scale across sessions and gives the discipline a persistent home.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Engelbart, D. C. *Augmenting Human Intellect: A Conceptual Framework*. SRI Project No. 3578 (Air Force Office of Scientific Research), 1962. Available via the Doug Engelbart Institute.
