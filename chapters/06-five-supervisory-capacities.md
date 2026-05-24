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

![Pentagon with five labeled vertices — PA, PF, TO, IJ, EI — each carrying a two-letter code, a plain-language name, and a one-sentence definition. The five capacities frame supervisory intelligence as a single whole; PA is highlighted because it was the capacity Seth exercised in the chapter opening.](images/06-five-supervisory-capacities-fig-01.png)

*Figure 6.1 — The five supervisory capacities. Each vertex names one capacity Codex does not supply; together they constitute supervisory intelligence.*

---

## PA: Plausibility Auditing

The first capacity is the one Seth exercised in the opening. The name for it is **plausibility auditing**: the capacity to hear the wrong note before verification catches it.

Here is the structure of what PA does. Codex generates output calibrated for accuracy against the patterns it learned. The accuracy is real. The match against your specific situation is partial — because your situation is not in the training corpus, and the parameters of your project are not in the prompt. PA is what fires when fluent output and your knowledge of the situation diverge.

The feeling is real before you can articulate the content. Seth felt it before he could say *default parameter, next sprint*. The feeling is data. PA is the discipline of attending to the feeling rather than overriding it in the interest of shipping.

![Two panels comparing the same fluent Codex output. Panel A — PA Fires: the surface checks pass but the off-feeling registers, prompting the supervisor to stop, investigate, and fix upstream. Panel B — PA Dormant: the off-feeling is absent or overridden, the output ships, and a silent failure propagates downstream.](images/06-five-supervisory-capacities-fig-02.png)

*Figure 6.2 — PA fires, PA dormant. The Codex output is identical; the supervisor's attention is the entire difference.*

What PA is not: paranoia. The practice is vigilance, not anxiety. You are not suspecting all output. You are attending to the off-feeling when it arises. In most prompts PA stays quiet. When it fires, you investigate.

PA is also the capacity that grows fastest with deliberate practice. Every time you stop to investigate an off-feeling and find something, the pattern recognition sharpens. Every time you stop and find nothing, you have practiced the discipline anyway. The cumulative effect is a more reliable early-warning system calibrated to your specific domain.

The dangerous middle from Chapter 3 — tasks that look like pattern work but require situational judgment — is exactly where PA earns its keep. The output passes the surface checks. PA is what catches the divergence beneath.

---

## PF: Problem Formulation

The second capacity runs upstream of everything else. **Problem formulation** is the capacity to decide what the build *is* before Codex sees it.

Codex optimizes within the frame you give it. If the frame is wrong, the output is wrong, elegantly. A well-specified problem produces a well-specified solution to the wrong question. The elegance of the output is no evidence that the problem was correctly formulated.

There is an observation from an OpenAI internal document on how their engineers actually use Codex: *"When I'm in meetings all day, Codex works in the background — but I give it the direction first."*[^1] The direction is PF. Without it, Codex is running unsupervised toward a destination you have not set.

What PF looks like in practice: before you type a Code Mode prompt, you have decided what the function does, what it takes as input, what it returns, what it does not do, and where it lives in the codebase. Not in Codex's mind — in yours. The Ask Mode interrogation from Chapter 4 is the operational tool for PF. *What would I need to know about the existing code before adding this feature?* is a PF question. The interrogation surfaces frames you had not considered before you commit to one.

![A funnel narrowing from a wide top labeled vague intent down to a narrow bottom labeled well-specified prompt. The funnel passes through interrogation, scope-and-invariants, and shape-and-done stages. Side annotations name the PF questions on the left and the failure modes the funnel excludes on the right.](images/06-five-supervisory-capacities-fig-03.png)

*Figure 6.3 — Problem Formulation as a funnel. PF collapses vague intent into the single sentence that becomes the prompt.*

PF is the most under-exercised of the five. The pressure to get going skips it. The student who pauses to formulate properly produces builds that work. The student who jumps to suggest produces builds that spend the next hour recovering. The time cost of PF is measured in minutes. The time cost of skipping it is measured in hours.

---

## TO: Tool Orchestration

The third capacity is the scheduling work. **Tool orchestration** is the capacity to choose which Codex mode, which task, in what order, with what trust level — and to know when not to use Codex at all.

You have Ask Mode. You have Code Mode. You have Best-of-N as a technique. You have AGENTS.md (the next chapter) as a way to constrain what Codex knows about your project. And you have the option of writing the code by hand, which is still sometimes the right answer.

TO is the conducting metaphor at its most literal. You are deciding which instrument plays now. Ask Mode for interrogation. Code Mode for implementation of a well-specified change. Best-of-N for choosing between two reasonable approaches. By hand for steps too consequential or too small to benefit from generation.

The mistake TO prevents is using Code Mode when Ask Mode should have run first. This is the most common sequencing error. The student generates code against a prompt, finds the code is architecturally wrong for the codebase, and has to redo it — not because the code was bad but because the interrogation step was skipped. Ask Mode would have surfaced the architectural constraint in thirty seconds. Code Mode without Ask Mode spent two hours to produce the same thirty seconds of information, buried in a function that has to be thrown away.

| Tool | Use when | Common mistake |
|---|---|---|
| Ask Mode | You need to understand the codebase, surface constraints, or plan a multi-step change | Skipping it and discovering the architectural conflict in Code Mode two hours later |
| Code Mode | The plan is reviewed, the spec is tight, and execution is the only thing left | Running it on a vague prompt and treating the resulting rollback as Codex's fault |
| Best-of-N | The spec under-constrains the answer — two reasonable approaches and judgment decides | Running it on a determined task and burning generation budget on cosmetic variation |
| By hand | The operation is too small to prompt for, or too consequential to delegate | Using Codex anyway because "it's faster" — then spending longer reviewing than typing |

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

An example. Three prompts ago you agreed with yourself that the article-review tool would never rewrite the article's voice — it would only flag length and structure issues. The current Code Mode output is producing a rewritten draft. EI fires: stop. The constraint from three prompts ago is being violated. Revert the prompt; respecify.

EI is the integration check. It is what makes the build coherent across its duration. It is also the capacity that most benefits from an external artifact — because holding all constraints in working memory across a multi-hour session is not reliable. AGENTS.md, which the next chapter develops in full, is the persistent record for EI. The constraint that Codex cannot remember between sessions is the constraint EI will catch this session *if* it is in AGENTS.md. Without the file, EI depends entirely on your memory. With it, EI has a scaffold.

![Horizontal timeline of a Codex session. A decision marker at prompt 3 sets a constraint. A drift zone runs through prompts 4 through 12, with a small redirect at prompt 7 and a pivot at prompt 11. At prompt 13 EI fires — the cumulative result violates the prompt-3 constraint and the build stops before shipping.](images/06-five-supervisory-capacities-fig-04.png)

*Figure 6.4 — EI across a session. The long-range capacity catches drift that no single prompt-level check would notice.*

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

| Step | Action | Capacity | Why this capacity, not another? |
|---|---|---|---|
| 1 | Decide what the feature is — one-sentence formulation | **PF** | The frame is being set; no output exists yet to audit or interpret |
| 2 | Open Ask Mode; ask Codex to read the relevant files | **TO** | A tool-choice junction — Ask before Code, not the other way around |
| 3 | Read the Ask Mode summary; notice a convention I forgot | **PA + IJ** | PA fires on the surprise; IJ supplies the meaning of the convention |
| 4 | Update the formulation to respect the convention | **PF revisit** | The frame has new information; the spec must catch up before code runs |
| 5 | Ask Mode plan for the implementation | **TO** | Another tool-choice junction — plan first, execute second |
| 6 | Read the plan; correct one assumption Codex made | **IJ** | The plan is fluent; what it means for *this* project is the human's call |
| 7 | Switch to Code Mode | **TO** | The plan is reviewed; the right instrument changes |
| 8 | Codex implements; I read the output | — | Generation is Codex's; no supervisory act yet |
| 9 | Output looks correct; tests pass; something feels off about an edge case | **PA** | The off-feeling is the signature of PA, not IJ — it precedes the articulation |
| 10 | Investigate; find Codex used a default that conflicts with the convention | **IJ + PA** | PA pointed; IJ named what the conflict meant in context |
| 11 | Update Code Mode prompt with explicit constraint; rerun | **PF revisit** | The spec was insufficient; the fix is upstream, not in another rollback |
| 12 | Output now correct; verify handoff conditions | — | Mechanical check, not supervisory judgment |
| 13 | Update AGENTS.md with the lesson about the convention | **EI** | The constraint must survive the session; that is EI's job, not PA's |

---

## Diagnosing What Went Wrong

The capacities are diagnostic. When a build produces a bad outcome, ask: which capacity was absent?

Output is fluent but wrong, and there were no warning signs — PA was absent, or fired and was overridden. The whole build solved the wrong problem — PF was absent at step zero. The wrong tool ran at the wrong time — TO was absent. The output is technically correct but means the wrong thing in context — IJ was absent. The final result violates a constraint set earlier — EI was absent.

The post-mortem is structured. Not *what should I have done* but *which of the five was absent, where, and what would the build have looked like if it had fired?* The structured question makes the next build better. The un-structured question produces guilt, not improvement.

| Symptom | Absent capacity |
|---|---|
| Fluent, passing output that turns out to be wrong, with no warning signs noticed in the moment | **PA** — fired faintly or was overridden |
| The whole build solves the wrong problem; the elegance is real and pointed in the wrong direction | **PF** — the frame was set badly or never set |
| The right work happened in the wrong order; Code Mode produced what Ask Mode should have surfaced | **TO** — the tool-choice junction was skipped |
| Output is technically correct but means the wrong thing for *this* project's terms or users | **IJ** — the interpretation in context wasn't supplied |
| The final result violates a constraint set thirty prompts ago; drift, not crash | **EI** — long-range integration broke |

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

---
