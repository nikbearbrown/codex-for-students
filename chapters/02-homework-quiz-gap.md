# Chapter 1 — The Homework/Quiz Gap: What's Actually Happening

> Students who use AI freely during practice score dramatically lower on unassisted tests — and feel like they learned more, not less.

---

## Learning outcomes

1. **(Understand)** Explain why AI-assisted practice can produce the feeling of mastery without the cognitive events that constitute it.
2. **(Analyze)** Distinguish between capability borrowed from the machine and capability built in the learner.
3. **(Evaluate)** Assess your own recent AI use against this distinction.

---

## Opening

The Bastani finding, stated plainly:

**48% higher scores during AI-assisted practice. 17 percentage points *lower* on the unassisted exam.**

Not slightly worse. Dramatically worse. And the students in the AI-Base arm felt like they had learned more, not less, because the fluency of the AI's output gave the feeling of mastery even as the cognitive events that produce mastery never occurred.

This chapter is the empirical foundation for the book. If you walk away convinced that the homework/quiz gap is real, measurable, and the specific failure mode the discipline catches, the rest of the book has somewhere to stand. If not, the rest reads as opinion.

<!-- → [TABLE: Bastani RCT results — two columns: AI-Assisted group vs. Hand-Coding group. Rows: practice score, exam score, score gap, Cohen's d, p-value. No color. Editorial style.] -->

---

## What actually happens in the brain when you struggle

This is not a neuroscience textbook. Two paragraphs of plain language is all the chapter needs.

When your brain encounters something it cannot yet do, several things happen. The mismatch between what you predicted and what actually happens is registered as **prediction error**. Dopamine fires. Brain-derived neurotrophic factor (BDNF) upregulates. The neural pathways involved in the work strengthen — synapses connect more reliably, dendritic spines form. These are the physical events that constitute **memory consolidation**. They are triggered by *cognitive friction*: the productive struggle of working through something difficult.

When the AI does the work for you, the prediction error does not occur. The dopamine fires for the wrong thing (the surprise of getting an answer, not the surprise of figuring one out). BDNF does not upregulate to the same level. Synaptic strengthening proceeds at a fraction of its rate. The output of the work is fine; the brain that did the work is unchanged. This is what *borrowing capability* means at the level of neurons.

For details on the neurobiology, the Cognitive Load Theory literature (Sweller, 1988 and forward) is the academic foundation. The book's pedagogical point does not require the details. The pedagogical point is: *the struggle is the mechanism of learning, not the cost of learning.* AI that removes the struggle does not just save time; it removes the trigger for the events that turn struggle into capability.

---

## The fluency trap

The trap that catches almost every technically fluent student.

When Codex produces a competent response — a working function, a well-structured class, a clear explanation — the experience of reading it feels like understanding. Your brain processes the response like it processes anything else you read. Some of it registers. The feeling at the end is a feeling of *mastery*.

That feeling is partly accurate (you did process some material) and mostly false (the cognitive events that constitute durable mastery were not triggered, because you did not produce the material). Fluency is the *appearance* of understanding without the *underlying mechanism*. The trap is that the fluency is functionally indistinguishable, from inside your own head, from genuine understanding. You cannot tell, in the moment, whether you have understood or merely read.

You can tell, later, when the unassisted task arrives. That is the homework/quiz gap.

This is the **Frictional** principle that runs through the entire series of books. The struggle is the point. AI should make struggle more *productive*, not eliminate it. The discipline this book teaches is the operational form of keeping struggle productive while using Codex to remove the *extraneous* parts (the syntax lookup, the boilerplate scaffolding) and preserve the *germane* parts (the algorithmic reasoning, the design judgment, the debugging skill).

---

## The Kosmyna EEG result

Independent confirmation, from a different surface.

Kosmyna and colleagues at MIT Media Lab measured brain connectivity during AI-assisted writing using EEG, with three groups: brain-only (no AI), search-engine-only (Google), and AI-assisted (ChatGPT).[^1] The arXiv preprint is from June 2025.

The headline finding: the AI-assisted group showed reductions in functional brain connectivity of up to 55% during writing compared to the brain-only group. The neural networks involved in comprehension, synthesis, and memory formation were less active in the AI-assisted condition.

A follow-up session had the AI-assisted group write *without* AI, after multiple AI-assisted sessions. They could not remember what they had written. The cognitive consolidation that would have built durable memory for their own essays had not occurred — because the cognitive work that produces consolidation had been outsourced.

The Kosmyna study measured essays. The Bastani study measured math. The Anthropic study (next section) measured Python. The same pattern appears across all three: outsource the cognitive work and the brain does not consolidate. The struggle is what produces the consolidation.

---

## The Anthropic 2026 RCT

The closest direct measurement of the conducting discipline.

Anthropic ran a randomized controlled trial with 52 mostly junior engineers learning Python's Trio asynchronous library — a technology none of them had used before.[^2] Half coded by hand; half had AI assistance. After the learning period, both groups took a 14-question conceptual comprehension quiz.

The numbers:
- AI-assisted group: **50%** average.
- Hand-coding group: **67%** average.
- **17 percentage points.** **Cohen's d = 0.738. p = 0.01.**

The study went further. It identified three **low-scoring interaction patterns**, averaging below 40%:

- **AI Delegation** — the engineer asks the AI to generate; runs the output; moves on. This is the friend from Chapter 0.
- **Progressive AI Reliance** — the engineer starts with their own attempt; increasingly hands work to the AI as the task gets harder. By the end, the AI is doing the substantive work.
- **Iterative AI Debugging** — the engineer relies on the AI to *diagnose* errors as well as fix them. The engineer's own debugging skill atrophies because the AI is doing the work that develops it.

And three **high-scoring patterns**, averaging 65% or higher:

- Asking follow-up questions about generated code before using it.
- Combining code generation with explanations of why the code is correct.
- Using AI for conceptual questions while coding the actual implementation by hand.

The high-scoring patterns share a property: *the engineer's cognition is engaged with the generated material rather than substituting for it.* They are operationally identical to the **Ask Mode → Code Mode gate** that this book teaches in Chapter 4. Anthropic measured what the discipline is. The discipline works because it produces the engagement patterns the study found correlate with skill formation.

---

## The debugging gap is the specific signal

A finding from the Anthropic study that bears on the homework/quiz gap directly.

The largest performance disparity between AI-assisted and hand-coding groups appeared specifically on **diagnostic questions** — questions that required the participant to identify what was wrong in a piece of code and explain why. On declarative questions (what does this function do?), the gap was smaller. On generative questions (write a function that does X), the gap was moderate. On diagnostic questions (find and explain the bug), the gap was *the largest*.

This is the homework/quiz gap in its most concentrated form. Debugging is the irreducible skill of the working engineer. It is the skill that requires the practitioner to hold the code in their head, predict what should happen, notice what does happen, and reason about the gap. The Iterative AI Debugging pattern — letting the AI find and fix the bugs — is the pattern that destroys this skill fastest, because it removes the cognitive event that develops it.

If you take one finding from this chapter into your practice, take this one: **never let the AI do your debugging for you**. The AI can help you think about the bug. It can explain a concept that the bug touches. It can suggest where to look. But the act of *finding and naming the bug yourself* is the irreducible cognitive work that the discipline is built to preserve.

---

## Worked example: two students, six weeks later

Two AP CS students. Same problem set. Same starting grade.

**Student A** opens Codex. Types the problem. Reads the response. Pastes the code. Tests pass. Submits.

**Student B** opens Codex. Asks Codex to *explain how it would approach the problem* (Ask Mode, conceptually). Reads the approach. Predicts what the code will look like. Asks Codex to write the code (Code Mode). Reads the code against the prediction. Notices where the actual code differs from the prediction. Runs it in a notebook with intermediate prints. Modifies one operation to see what changes. Submits.

Both submit working code. Both get the same grade.

Six weeks later: the variant on the quiz. Student A freezes. Student B writes the variant from scratch in twenty minutes.

The artifact A and B produced six weeks ago was indistinguishable. The *practitioners* are now different people. Student A delegated. Student B conducted. The Anthropic patterns map exactly: Student A is *AI Delegation*. Student B is *follow-up questions + code-with-explanation + AI-for-conceptual-only*.

This is the homework/quiz gap in a single classroom over six weeks.

---

## Start with Ask Mode questions

A practical move that the empirical foundation justifies.

Before your first Code Mode session with a new codebase or a new problem, spend time in **Ask Mode** asking questions. *"What does this function do?"* *"Where would I add a new feature like X?"* *"What conventions does this code follow?"* *"What would I need to know before writing the next function?"*

Two things happen.

You **calibrate Codex** — you see what it gets right (file structure, conventions, the obvious patterns) and what it gets wrong (the specific conventions you care about that aren't visible in the code yet). The gap between what Codex sees and what you know is the gap your AGENTS.md (Chapter 6) will close.

You **engage with the material** — you predict, read, predict, read, in a way that triggers the cognitive events the Anthropic high-scoring patterns name. The engagement starts now, not after the first Code Mode session.

Senior OpenAI engineers describe this as their standard practice when they open a new codebase: *"For large changes, start by prompting Codex for an implementation plan using Ask Mode."*[^3] The pattern is theirs; the discipline is yours.

---

## Common misconceptions

**"My friend uses Codex without the discipline and is doing fine."** Survival bias. Your friend has not yet hit the unassisted task that exposes the gap. The students in the Bastani GPT Base arm felt like they were doing fine on the practice. They were not.

**"I'll catch it before it gets bad."** Probably not. The whole point of the fluency trap is that you cannot reliably tell, from inside your own head, whether you have understood or merely read. The discipline is the external check that catches what your own perception cannot.

**"Studies of essay writing and math don't apply to coding."** They apply by mechanism. The cognitive events that produce consolidation (prediction error, dopamine, BDNF, synaptic strengthening) are domain-general. The Anthropic study measured coding directly. The pattern holds.

**"The frontier models have closed the gap."** No. The Anthropic 2026 RCT used a frontier model. The 17-point gap is recent. Improvements in model capability do not close the homework/quiz gap, because the gap is about *who is doing the cognitive work*, not about the quality of the output.

**"I can read fast; I'll still consolidate even if I don't write."** Reading produces partial consolidation. The cognitive events that produce durable consolidation are triggered by *active prediction and verification* — predicting before reading, testing your prediction, noticing where you were wrong. Passive reading produces fluency without the underlying mechanism.

---

## Exercises

1. **(Apply)** Identify three recent AI interactions of your own. For each: did you *build* the capability or *borrow* it? Be honest. Mark the ones where you borrowed.

2. **(Analyze)** Given two assignment transcripts (provided in class, or constructed from your own history), identify which student is building and which is borrowing. Defend your answer with specific moves each student made.

3. **(Evaluate)** Design a rule for your own AI use that would prevent the homework/quiz gap on your next unit. The rule should be specific (something you could check by inspecting your terminal or chat history) and binary (you did or didn't follow it).

---

## What would change my mind

The chapter's central empirical claim is that **the three foundational studies — Bastani, Kosmyna, Anthropic 2026 — converge on a real and measurable cognitive cost to unguarded AI delegation**, and that the cost is the homework/quiz gap. If a sufficiently large 2027 or 2028 follow-up with frontier-generation models across more diverse populations failed to replicate the gap, the empirical foundation softens.

The operational claim is that **the high-scoring engagement patterns the Anthropic study identified are what the conducting discipline operationalizes**. If a controlled study compared students using the Ask Mode → Code Mode gate to students using Codex without it and found no measurable difference in unassisted-task performance, the gate becomes recommended practice rather than load-bearing.

Both are empirically open. The chapter operates on the convergent evidence and on the mechanistic plausibility of the underlying claim.

---

## Still puzzling

- **How quickly the atrophy sets in.** Bastani's exam was at the end of a multi-week study. Whether two weeks of unguarded delegation is enough to produce a measurable effect, or whether the effect requires longer, is not directly measured.

- **Whether some students are systematically more or less vulnerable.** Individual variation is real but unmeasured at population scale. The book's stance: assume you are vulnerable and use the discipline. The cost is small; the alternative cost is large if you guessed wrong.

- **What the long-horizon effects look like.** The studies measure at weeks-to-months. Six-month, year-long, multi-year effects are not directly studied. The book's working assumption is that the atrophy compounds; the empirical case for compounding is open.

---

## AI Wayback Machine

🕰️ **William James** (1842–1910) — American psychologist whose chapter on **Habit** in *The Principles of Psychology* (1890) is the foundational account of how repeated engagement consolidates effortful cognitive struggle into durable capability.[^4] James wrote: *"All our life, so far as it has definite form, is but a mass of habits — practical, emotional, and intellectual — systematically organized for our weal or woe, and bearing us irresistibly toward our destiny, whatever the latter may be."* The neural mechanism James was describing — the consolidation of repeated effortful work into durable structure — is exactly the mechanism the Bastani, Kosmyna, and Anthropic findings show is broken by unguarded AI delegation. James argued, more than a century before the studies measured it, that *the struggle is the mechanism*. Without the struggle, the consolidation that would convert today's Codex session into tomorrow's capability does not occur. The fluency trap is exactly the trap James warned about — the appearance of skill without the underlying habit-formation that constitutes it.

---

## Bridge

You know the risk. You don't yet know which specific cognitive capacities are at stake. Chapter 2 names what you are *good at* (and what Codex is *better at*) — and where the dangerous middle between them lives.

---

[^1]: Kosmyna, N. et al. "Your Brain on ChatGPT: Accumulation of Cognitive Debt when Using an AI Assistant for Essay Writing Task." arXiv:2506.08872, MIT Media Lab, June 2025.
[^2]: "How AI assistance impacts the formation of coding skills." Anthropic, 2026; arXiv:2601.20245.
[^3]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^4]: James, W. *The Principles of Psychology*. Henry Holt, 1890. The Dover reprint (1950) is the standard modern edition.
