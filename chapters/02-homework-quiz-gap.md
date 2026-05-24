# Chapter 2 — The Homework/Quiz Gap: What's Actually Happening

*The fluency of the answer is indistinguishable, from inside your own head, from the fluency of understanding.*

---

Here is a number: **48**.

Forty-eight percent higher scores during AI-assisted practice. Students who used a large language model freely on their problem sets outperformed their peers by nearly half a standard deviation. If you stopped the experiment there, you would conclude that AI is a remarkably effective learning tool. You would be wrong.

Here is the second number: **−17**.

Seventeen percentage points *lower* on the unassisted exam. The same students who had apparently learned so much during practice performed dramatically worse when the AI was removed. Not slightly worse. Dramatically worse. And the remarkable thing — the thing that makes this worth a chapter — is that the students in the AI-assisted group reported feeling like they had learned *more*, not less. The experience of working with AI felt like mastery. The exam revealed it was not.

This is the Bastani finding. Omar Abdel Hamid Bastani and colleagues ran a randomized controlled trial with middle-school students learning algebra. The result is not a speculation about what might happen. It is a measurement of what does happen, with controls.

| Measure | AI-assisted group | Hand-coding group |
|---|---|---|
| Practice score | 48% above control | At control baseline |
| Unassisted exam score | 17 points below control | At control baseline |
| Score gap (practice → exam) | −65 points | ~0 |
| Cohen's *d* (effect size) | 0.738 | — |
| *p* value | 0.01 | — |

The name I am going to give this phenomenon is the **homework/quiz gap**: the divergence between performance when AI is available and performance when it is not. The gap is the empirical foundation of everything this book argues. If I cannot convince you the gap is real, the rest reads as opinion. So let me try to convince you — not by asserting it, but by explaining the mechanism that produces it.

---

## What the brain is doing when it learns

I want to tell you what is actually happening in your head when you learn something. Not metaphorically — actually. The mechanism.

When you attempt a problem and cannot yet solve it, a specific sequence of events occurs. Your brain made a prediction — either explicitly (you thought you knew the answer) or implicitly (it expected a familiar pattern and did not find one). The mismatch between the prediction and reality is registered. This is called **prediction error**. Dopamine is released in response to the mismatch. Brain-derived neurotrophic factor — BDNF, a protein — upregulates. The synaptic connections involved in the attempt strengthen. Dendritic spines form. This is what **memory consolidation** looks like at the level of neurons.

![Six-node linear chain — Attempt, Prediction Error, Dopamine Release, BDNF Upregulation, Synaptic Strengthening, Consolidation. The Attempt node is highlighted in red as the link that AI delegation removes.](images/02-homework-quiz-gap-fig-01.png)
*Figure 2.1 — The consolidation chain*

None of that is metaphor. Those are the physical events. The struggle is not a tax you pay in exchange for learning. The struggle *is* the mechanism of learning. Prediction error, dopamine, BDNF, synaptic strengthening — these are the events that convert today's confusion into tomorrow's capability. The technical literature calls this **desirable difficulty**: the cognitive friction that triggers the biological machinery. John Sweller's Cognitive Load Theory (1988 and forward) is the formal framework; the point here is simpler. You cannot get the consolidation without the friction. They are the same event.

Now consider what happens when the AI does the problem for you.

Your brain reads the AI's response. You process it. Some information registers. The experience, phenomenologically, feels like understanding — the response is coherent, often elegant, and your brain processes coherent elegant information the same way it processes anything else you read. But the prediction error did not occur. You did not attempt the problem and fail. There was no mismatch to register, no dopamine to fire for the right thing, no BDNF upregulation, no synaptic strengthening. The output of the session is fine. The brain that participated in the session is unchanged.

This is what it means, mechanistically, to **borrow capability from the machine**. The product looks identical. The practitioner is not.

---

## Why you cannot tell from the inside

The trap is not that the AI produces bad output. The trap is that good AI output feels like understanding.

When Codex produces a working function — the right algorithm, clean variable names, a sensible structure — reading it feels like comprehension. Your brain processes the response the same way it processes a textbook explanation you genuinely followed. You finish the session with the same phenomenological signature you would have if you had worked through the problem yourself: a sense of clarity, a sense of progress, the absence of confusion.

That sense is **fluency**. It is the appearance of understanding, produced by processing fluent text. It is not the same thing as the underlying capability — the neural wiring that would let you reproduce the thinking on a different problem next week — but it feels identical from the inside. You cannot tell, in the moment, whether you have *built* something or merely *read* something that felt right.

The Bastani students felt like they had learned more. They had not. But the signal they were reading — subjective fluency, the absence of struggle, the sense of having moved through the material — was genuinely present. The signal was just measuring the wrong thing.

This is the **fluency trap**: AI-assisted work is experienced as mastery because fluency and mastery feel the same from inside your head. The test that distinguishes them is the unassisted task. That is why the homework/quiz gap exists. The exam is not a different kind of evaluation; it is the first time the unassisted task appears. That is when you discover what you actually built.

![Two-panel illustration. Left: stylized brain reading smooth output with a checkmark, labeled FLUENCY. Right: same brain mid-struggle with a struck-through prediction and a corrected line, labeled MASTERY.](images/02-homework-quiz-gap-fig-02.png)
*Figure 2.2 — Fluency vs. Mastery: the phenomenological experience is identical; the neural events are not*

---

## Independent confirmation from a different surface

In June 2025, Nataliya Kosmyna and colleagues at MIT Media Lab measured brain connectivity during AI-assisted writing using EEG. Three groups: writing with no AI, writing with a search engine, writing with ChatGPT. The headline finding: the AI-assisted group showed reductions in functional brain connectivity of up to 55% during writing compared to the brain-only group. The neural networks involved in comprehension, synthesis, and memory formation were less active when the AI was doing more of the cognitive work.

A follow-up session had the AI-assisted group write without AI, after multiple AI-assisted sessions. They could not remember what they had written. The consolidation that would have built durable memory for their own essays had not occurred.

![Bar chart of functional brain connectivity across three writing conditions — Brain Only at 100%, Search Engine at 72%, AI-Assisted at 45%. The 55% drop in the AI-Assisted condition is annotated against the dashed baseline.](images/02-homework-quiz-gap-fig-03.png)
*Figure 2.3 — Kosmyna et al.: functional brain connectivity during writing*

Bastani measured algebra. Kosmyna measured essays. The same pattern appears: outsource the cognitive work and the brain does not consolidate. The mechanism is domain-general. Prediction error, dopamine, BDNF — these are not specific to math or writing. They are triggered by effortful cognition and suppressed when the effort is removed. The surface differs; the mechanism does not.

---

## The Anthropic 2026 result

Now programming specifically, since that is the domain this book is about.

Anthropic ran a randomized controlled trial with 52 junior engineers learning Python's Trio asynchronous library — a technology none of them had used before.[^2] Half coded by hand; half had AI assistance. After the learning period, both groups took a 14-question conceptual comprehension quiz.

The numbers:
- AI-assisted group: **50%** average.
- Hand-coding group: **67%** average.
- **17 percentage points. Cohen's d = 0.738. p = 0.01.**

This is the homework/quiz gap measured directly in a programming context, with a frontier model, in 2026. The model is recent. The gap is real.

![Bar chart of the Anthropic 2026 RCT — AI-assisted group at 50% and hand-coding group at 67% on the 14-question conceptual quiz. Error bars and a 17-point gap annotation.](images/02-homework-quiz-gap-fig-04.png)
*Figure 2.4 — Anthropic 2026 RCT: quiz score by condition*

The study did not stop at the headline number. It identified the specific interaction patterns that predicted low performance — averaging below 40%:

**AI Delegation**: the engineer prompts the AI, runs the output, moves on. No engagement with the mechanism that produced it.

**Progressive AI Reliance**: the engineer starts with their own attempt, then increasingly hands work to the AI as difficulty increases. By the end of the problem, the AI is doing the substantive cognitive work.

**Iterative AI Debugging**: the engineer relies on the AI not just to fix errors but to *find* them and *name* them. The debugging skill atrophies because the cognitive event that develops it — noticing the gap between what should happen and what does happen — is outsourced.

And three patterns that predicted high performance — averaging 65% or higher:

Asking follow-up questions about generated code *before* using it. Combining code generation with explanations of *why* the code is correct. Using AI for conceptual questions while coding the actual implementation by hand.

| Pattern | Description | Avg quiz score | Why it affects consolidation |
|---|---|---|---|
| **Low-scoring (< 40%)** | | | |
| AI Delegation | Prompt, run the output, move on — no engagement with the mechanism | < 40% | No prediction is formed; no mismatch can register |
| Progressive AI Reliance | Start by hand, hand off more to the AI as difficulty climbs | < 40% | Outsources exactly the hard step that triggers the dopamine signal |
| Iterative AI Debugging | Let the AI find and name the bugs as well as fix them | < 40% | Skips the prediction-vs-observation loop that builds debugging skill |
| **High-scoring (≥ 65%)** | | | |
| Follow-up questioning | Ask *why this data structure, why not the obvious approach* before using the code | ≥ 65% | Forces prediction and comparison against the generated answer |
| Explanation pairing | Have Codex explain the design decision, then verify the explanation against what you know | ≥ 65% | Verification surfaces prediction errors and consolidates them |
| Concepts up, code by hand | Use Codex for API and concept lookup, write the implementation yourself | ≥ 65% | Strips extraneous load while preserving the germane reasoning |

The low-scoring patterns share one property: the engineer's cognition is not engaged with the generated material. The high-scoring patterns share the opposite: the engineer is predicting, reading, questioning, comparing. The biological machinery is running. Prediction error is occurring. The consolidation proceeds.

---

## Where the gap is largest

A specific finding from the Anthropic study that carries more weight than the headline number.

The largest performance disparity between the two groups appeared on **diagnostic questions** — questions that required identifying what was wrong in a piece of code and explaining why. On declarative questions (what does this function do?), the gap was smaller. On generative questions (write a function that does X), the gap was moderate. On diagnostic questions — find and explain the bug — the gap was largest.

This is not a surprise if you understand the mechanism. Debugging is the irreducible cognitive skill of the working engineer. It requires holding the code in your head, predicting what should happen, noticing what does happen, and reasoning about the gap. That reasoning loop — prediction, observation, mismatch, explanation — is exactly the sequence that triggers the biological machinery of consolidation. Every debugging session is, at the neural level, a learning event. The engineer who lets the AI do the debugging is skipping the event that develops the skill.

![Horizontal bar chart of the performance gap by question type — Declarative 7 points, Generative 13 points, Diagnostic 23 points. The Diagnostic bar is highlighted in red.](images/02-homework-quiz-gap-fig-05.png)
*Figure 2.5 — Where the gap is largest: diagnostic questions dwarf the others*

The Iterative AI Debugging pattern is the fastest way to create a practitioner who can produce code but cannot maintain it. The maintenance — the finding of what is wrong and why — is the test that exposes what was built. It is the unassisted task in its purest form.

---

## Two students, six weeks apart

Let me make this concrete with a scenario — labeled as such, because I am not going to fabricate a case study.

Suppose two students with the same starting level face the same problem set.

Student A opens Codex, types the problem, reads the response, pastes the code. Tests pass. Submits. The experience is smooth. Twenty minutes.

Student B opens Codex, but first asks Codex to explain how it would approach the problem — not to write the code, to explain the approach. Student B reads the explanation and forms a prediction: *this is roughly what the code will look like*. Then Student B asks Codex to write the code. Student B reads the code against the prediction. Notices one place where the actual code differs from the prediction. Runs the code in a notebook with an intermediate print statement to see what the code is doing at that step. Modifies one operation to see what changes. Then submits.

The artifact Student A and Student B produced is indistinguishable. Same grade.

Six weeks later: a variant of the problem appears on the quiz, without AI.

Student A freezes. The code they submitted six weeks ago was produced by a process that left nothing in their head. There is no prediction to make, no structure to reach for. The session produced fluency. The quiz reveals the fluency was not the thing.

Student B writes the variant from scratch. Not perfectly — they make an error, catch it, fix it. Twenty minutes. They pass.

The Bastani finding, the Kosmyna finding, and the Anthropic finding all describe the same divergence between these two students. The mechanism is the same in algebra, in essay writing, in Python. Student A borrowed capability. Student B built it. The difference is not detectable at week one. It is exactly detectable at the unassisted task.

![Two-track six-week timeline. Student A's cognitive engagement plots flat at zero across all six weeks. Student B's plots incrementally upward each week. Both tracks start at the same point in week one. At week six the quiz score diverges sharply.](images/02-homework-quiz-gap-fig-06.png)
*Figure 2.6 — Two students, six weeks: the artifact was identical; the practitioner was not*

---

## What the high-scoring patterns actually are

The Anthropic study found three high-scoring engagement patterns. I want to say clearly what they are, because they are easy to describe but easy to misunderstand.

**Asking follow-up questions before using the code** does not mean asking "is this right?" It means engaging with the mechanism: *why did you use this data structure here? what happens if the input is empty? why not the obvious approach?* The questions force prediction and comparison. The biological machinery runs.

**Combining code generation with explanation** does not mean asking Codex to comment the code. It means asking Codex to explain *why* a design decision was made, then evaluating whether the explanation is correct against what you know. You are doing the work of verification. Verification triggers prediction error when you find you were wrong. Prediction error is the event.

**Using AI for conceptual questions while coding by hand** is the most direct operationalization of the mechanism. You outsource the lookup — *what is the signature of this function? what does this parameter control?* — and preserve the cognitive work — *I will write this myself, now that I know the API exists.* The extraneous load drops. The germane load, the part that builds the skill, is preserved.

These three patterns are not a methodology. They are descriptions of what practitioners who built durable skill were observed doing. The description comes after the measurement. But they map cleanly onto a principle: *keep the cognitive work in your head, use the AI to remove the extraneous parts.* Syntax lookup is extraneous. Boilerplate scaffolding is extraneous. Algorithmic reasoning is not. Design judgment is not. Debugging is not.

Senior engineers at OpenAI describe the same pattern from the other direction: *"For large changes, start by prompting for an implementation plan using Ask Mode."*[^3] They are preserving the design cognition and offloading the mechanical parts. The pattern is theirs; the mechanism is domain-general.

---

## What would change my mind

I want to be clear about the epistemic status of what I have just argued.

The three foundational studies — Bastani, Kosmyna, Anthropic 2026 — converge on a real and measurable cognitive cost to unguarded AI delegation. The convergence is across domains (algebra, essays, Python), research groups (separate teams), and methodologies (RCT, EEG, RCT). The mechanistic account — prediction error, BDNF, synaptic consolidation — is not invented for this book; it is the established neuroscience of learning.

What would soften the empirical foundation: a sufficiently large 2027 or 2028 follow-up, with frontier-generation models, across more diverse populations, that failed to replicate the gap. It is possible. The studies are recent, the population sizes are bounded, and frontier models improve fast. I am watching for replication.

What would soften the operational claim: a controlled study that compared students using the engagement discipline this book teaches to students using AI without it, and found no measurable difference in unassisted-task performance. That study does not exist yet. The claim is mechanistically grounded but operationally unconfirmed at scale.

Both are empirically open. I am operating on convergent evidence and mechanistic plausibility. I am telling you what the evidence says, and I am telling you what the evidence does not yet say.

---

## What is still puzzling

Three things I do not know and think are worth naming.

**How quickly atrophy sets in.** Bastani's exam was at the end of a multi-week study. Whether a single week of unguarded delegation is enough to produce a measurable gap, or whether it requires longer sustained exposure, is not directly measured. My assumption is: faster than you expect. The mechanism is continuous. Every session either builds or borrows.

**Whether some students are systematically more or less vulnerable.** Individual variation is real. Some practitioners are better at engaging with AI output even without a deliberate discipline; some are worse. The population-level finding tells you the average. It does not tell you where you are in the distribution. The book's stance: assume you are vulnerable and use the discipline. If you are not vulnerable, the cost of the discipline is small. If you are vulnerable, the cost of skipping it is large.

**What the long-horizon effects look like.** Bastani measures weeks. Kosmyna measures sessions. Anthropic measures a learning period. There is no six-month, one-year, five-year study. My working assumption is that the atrophy compounds — that a practitioner who delegates for a year is qualitatively different from one who borrowed for a week. The compounding is mechanistically plausible; it is empirically open.

---

## AI Wayback Machine

🕰️ **William James** (1842–1910) — American psychologist whose chapter on **Habit** in *The Principles of Psychology* (1890) is the foundational account of how repeated effortful engagement consolidates into durable capability.[^4] James wrote: *"All our life, so far as it has definite form, is but a mass of habits — practical, emotional, and intellectual — systematically organized for our weal or woe, and bearing us irresistibly toward our destiny, whatever the latter may be."*

James was describing, in the language of 1890, exactly the mechanism the three studies measured. The consolidation of repeated effortful work into durable neural structure. The struggle as the mechanism, not the cost. Without the struggle, the habit does not form. Without the habit, the practitioner does not change. The fluency trap is the trap James warned about — the appearance of skill without the underlying formation that constitutes it. He did not have randomized controlled trials or EEG. He had careful observation and a very clear idea of what learning actually was.

The mechanism has not changed. Only the tool that obscures it is new.

---

## Bridge

You now know the risk and the mechanism that produces it. The next question is anatomical: *what specifically are you good at, what is Codex better at, and where does the dangerous middle between them live?* That is where Chapter 3 begins.

---

## Exercises

**Warm-up**

1. *(Tests: mechanism of consolidation)* State in your own words — without consulting the chapter — what prediction error is and why it is necessary for memory consolidation. Then check your answer against the chapter. Where were you right? Where did you miss something?

2. *(Tests: borrowed vs. built capability)* Describe the difference between borrowing capability from an AI and building capability in yourself. Give one concrete example of each from a coding or writing task you have done in the past month.

3. *(Tests: fluency trap)* A student says: "I read through the AI's solution and it made total sense to me. I think I understand it." What is the most important question you would ask this student to test whether that feeling is accurate?

**Application**

4. *(Tests: low-scoring interaction patterns)* You are given a transcript of an engineer working with Codex for 40 minutes. The engineer prompts for a function, runs it, prompts for another, runs it, asks Codex to find the bug in the second function, applies the fix, and submits. Identify which low-scoring pattern or patterns from the Anthropic study this transcript exhibits. Explain why each pattern you identify is likely to suppress consolidation.

5. *(Tests: high-scoring interaction patterns)* Rewrite the same 40-minute session from exercise 4 so that it exhibits at least two high-scoring patterns. You do not need to change what gets built — only how the engineer engages with the AI while building it. Be specific about what the engineer says and does differently.

6. *(Tests: diagnostic gap)* The Anthropic study found that the performance gap between AI-assisted and hand-coding groups was largest on diagnostic questions. Using the mechanistic account from this chapter, explain why that specific question type would show the largest gap. Your explanation should reference at least one named cognitive event from the consolidation sequence.

**Synthesis**

7. *(Tests: mechanism + interaction patterns together)* The three high-scoring engagement patterns the Anthropic study identified were not designed in advance — they were observed in the data. Yet each one maps onto the consolidation mechanism described in the neuroscience section. For each of the three patterns, identify the specific step in the consolidation sequence (prediction, prediction error, dopamine, BDNF, synaptic strengthening) that the pattern preserves or triggers. Explain your mapping.

8. *(Tests: fluency trap + homework/quiz gap)* A student completes a problem set entirely with AI assistance, scores 90%, and tells you they are confident going into the exam. Using the concepts of the fluency trap and the homework/quiz gap, write a two-paragraph explanation — addressed to that student — of exactly why their confidence may be miscalibrated and what they could do before the exam to find out.

**Challenge**

9. *(Open-ended)* The chapter argues that the homework/quiz gap is produced by a domain-general mechanism — that the same consolidation failure that Bastani measured in algebra and Kosmyna measured in essays applies to programming. Design a study that would either confirm or challenge the domain-generality claim specifically for debugging skill. What would you measure, how would you measure it, and what result would constitute a real challenge to the chapter's argument?

---

[^1]: Kosmyna, N. et al. "Your Brain on ChatGPT: Accumulation of Cognitive Debt when Using an AI Assistant for Essay Writing Task." arXiv:2506.08872, MIT Media Lab, June 2025.
[^2]: "How AI assistance impacts the formation of coding skills." Anthropic, 2026; arXiv:2601.20245.
[^3]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^4]: James, W. *The Principles of Psychology*. Henry Holt, 1890. The Dover reprint (1950) is the standard modern edition.

---
