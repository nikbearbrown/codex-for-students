# Chapter 8 — Writing Codex Prompts That Are Specifications

> "Write me a login function" is not a prompt. A prompt names the thing, the invariants, the output format, and what not to touch.

---

## Learning outcomes

1. **(Understand)** Distinguish a prompt (request) from a specification (complete task definition).
2. **(Apply)** Rewrite a weak prompt as a complete specification using the five-element format.
3. **(Analyze)** Identify what is missing from a set of provided prompts that would cause Codex to produce incorrect output.

---

## Opening

The chapter is short. The discipline is one paragraph; the rest is the practice.

You have a problem formulation (Chapter 7). You have AGENTS.md with the project's rules (Chapter 6). You are about to type a Codex prompt. The discipline is to write the prompt as a **specification** — five elements, each specific enough that Codex cannot reasonably misinterpret.

The five elements:

1. **The specific task.** Not "help me with X" — the *one thing* to do, stated as an operation.
2. **The invariants.** What must not change. The existing files, the existing tests, the existing function signatures.
3. **The context.** Which AGENTS.md sections govern this step, which existing files Codex should reference as templates.
4. **The output format.** What done looks like. Where the new code lives, what shape the change takes.
5. **The negative constraint.** What Codex must *not* do. The "do not modify file X." The "do not add a new dependency." The "do not change the interface."

Same task, two prompts:

> *"Add user authentication."* (Request.)
>
> *"Implement a function `authenticate(username, password) → Optional[User]` in `auth/auth.py`. Use the existing `bcrypt` password hashing pattern from `auth/hashing.py`. Return the User if credentials match a row in the `users` table; return None otherwise. Do not modify the User class. Do not modify the database schema. Do not add a new dependency. Do not change `auth/hashing.py`. Add a unit test in `tests/auth/test_auth.py` for the success case, the missing-user case, and the wrong-password case."* (Specification.)

The CLI's output for the first prompt is generic. The CLI's output for the second prompt fits the project. Same Codex. The five elements are the difference.

<!-- → [TABLE: Prompt vs. specification — two columns, five rows. Each row: one element. Left: weak prompt version. Right: specification version. Applied to the same task.] -->

---

## Why each element matters

Each element protects against a class of failure.

**Specific task** protects against Codex choosing a different verb than you meant. "Help me with auth" can become "redesign auth" or "add OAuth" or "patch the existing flow." Specifying the operation forces Codex to your verb.

**Invariants** protect against Codex modifying code you do not want modified. Without invariants, the CLI may "improve" your existing User class as part of its work; you wake up to a diff that touches files you did not expect.

**Context** is the link between the prompt and AGENTS.md (and the rest of the codebase). Pointing at the relevant AGENTS.md sections and at template files sharpens the output dramatically. You do not need to repeat everything in AGENTS.md (Codex has it loaded); you do need to indicate which sections govern *this step*.

**Output format** is your handoff condition (Chapter 9) in prose form. The format names where the new code lives, what shape the change takes, what tests should exist. Writing the output format makes it harder for Codex to call something done that isn't.

**Negative constraint** is the most under-used element and the one that catches the dangerous middle most often. *"Do not change `auth/hashing.py`"* is one negative constraint. *"Do not introduce a new dependency"* is another. *"Do not modify the database schema"* is a third. Negative constraints are the operational form of the "never" rules in AGENTS.md, instantiated for this specific prompt.

---

## Worked example: the five elements in action

The task: add a feedback-generation step to the grading tool.

**Prompt (don't do this):**

> "Add a feedback generator to the grading tool."

**Specification:**

> **Operation:** Implement `generate_feedback(submission, rubric) → str` in `grading/feedback.py`.
>
> **Invariants:** The `grading/rubric.py` module is unchanged. The `Submission` and `Rubric` classes are unchanged. The grading tool's CLI entry point in `grading/__main__.py` is unchanged.
>
> **Context:** AGENTS.md sections on the "never" rules (specifically: NEVER generate a final grade; the feedback is a first-draft only). Reference `grading/scorer.py` for the existing pattern of producing per-criterion output. The Voice section of AGENTS.md applies — feedback is matter-of-fact, second-person, no encouragement boilerplate.
>
> **Output format:** A complete `grading/feedback.py` with the function defined. A unit test in `tests/grading/test_feedback.py` covering at least: a submission that passes all rubric criteria; a submission that fails two criteria; an empty submission. The function returns a string under 500 characters.
>
> **Negative constraint:** Do not generate a final grade in any form (no numbers, no letters, no percentages). Do not modify `grading/scorer.py` or `grading/rubric.py`. Do not add a new dependency. Do not produce feedback the teacher would not say to the student (no generic "great effort!" or "keep practicing!").

The specification is roughly 150 words. The CLI's resulting Code Mode output is targeted: a function that lives in the right file, follows the project's voice rules, respects the never-generate-grade rule, and has tests covering the right cases.

A weaker prompt — "add a feedback generator" — would have produced a function that might generate a grade (because that is the most-probable thing a "feedback generator" does, on average), might modify the scorer.py (because the scorer.py is the most-probable place to look for related code), and might produce generically chipper feedback (because that is the most-probable voice the model was trained on). Each of these failures is invisible until later; by then, the build has drifted.

The 150 words of specification are the protection.

---

## Common misconceptions

**"Specifications are for big builds."** Any operation that can fail silently deserves the five-element format. The cost of writing 150 words is small. The cost of catching a silent failure with a vague prompt is large.

**"More words = better."** No. Precision per element, not verbosity. A 50-word specification with each element specific is better than a 300-word one with vague elements. The test is per-element specificity, not length.

**"I can just edit the generated code."** Sometimes. The dangerous middle is when the generated code is not visibly wrong. Specification prevents the dangerous middle by giving Codex no room to interpret. Editing-after-the-fact only catches cases where the wrongness is visible.

**"Negative constraints feel paranoid."** They are not paranoia. They are the operational form of the "never" rules in AGENTS.md and of the lessons learned from previous builds. Writing them in the prompt is the protection against Codex producing output that violates them.

**"The CLI doesn't need all five; it'll figure out the rest."** The CLI will fill defaults for missing elements. Defaults are averages across the training data; you are not the average. The cost of relying on defaults is the silent failure when the default is wrong for your project.

**"Give Codex a way to verify its own work."** OpenAI's engineers describe this as the move that produces dramatic improvements in output: *"When Codex has some way to check its work — like run tests or screenshot the UI — it can iterate and get dramatically better results."*[^1] Include a test suite or a verification command in the specification; Codex iterates against it before reporting done. This is the in-prompt equivalent of the handoff condition (Chapter 9).

---

## Exercises

1. **(Apply)** Take three Codex prompts from your recent history. Rewrite each as a specification. Run both versions through Codex. Document three specific ways the outputs differ.

2. **(Analyze)** A provided specification is missing two elements. Identify which ones and explain what Codex will do wrong as a result.

3. **(Create)** Write a complete five-element specification for the next step in your current project. Apply the gate from Chapter 4 (Ask Mode plan first; review; then Code Mode with the specification).

---

## What would change my mind

The chapter's operational claim is that **the five-element specification format produces materially better Codex output** than free-form prompts. If a controlled comparison found no measurable difference in output quality, build time, or correction rate on matched tasks, the format becomes a checklist rather than load-bearing discipline. The chapter would still teach it as a first habit; the case for "every consequential prompt" softens.

The chapter operates on the prompt-engineering literature (which broadly supports specificity) and on the structural argument that explicit specification removes interpretation room the CLI would otherwise fill with averages.

---

## Still puzzling

- **How much of the format can be omitted as Codex improves.** Frontier-generation models handle vague prompts better than 2023-generation models. Some elements may become less essential as model behavior matures. The book's prescription assumes the current state.

- **Is there a fast path for trivial prompts?** A typo fix. A one-line CSS adjustment. The five-element format feels heavy for these. The book's heuristic: skip the format for tasks whose entire description fits in one verifiable sentence. Where the threshold is exactly is fuzzy.

- **How the format transfers across agentic tools.** The five elements are framed for Codex. The same five (operation, invariants, context, output format, negative constraint) work for Claude Code and GitHub Copilot CLI. Tool-agnostic at the conceptual level.

---

## AI Wayback Machine

🕰️ **Ada Lovelace** (1815–1852) — English mathematician who wrote what is widely recognized as the first computer program: an algorithm for computing Bernoulli numbers, specified as an explicit ordered sequence of operations with dependencies and side effects, written for Babbage's Analytical Engine before any machine existed that could run it.[^2] Lovelace did not write a *request* for the machine to compute Bernoulli numbers. She wrote a *specification* — every operation named, every dependency stated, every intermediate result accounted for. She wrote it that way because she knew the machine could not interpret or guess; the specification had to leave no room.

The five-element format is Lovelace's discipline applied to AI-generated code. Codex *can* interpret and guess; that is precisely the problem the specification prevents. Lovelace specified for a machine that could not interpret; you specify for a CLI that interprets too freely. The discipline she demonstrated in 1843 — make every decision explicit, leave no interpretation room — is the discipline the chapter operationalizes.

---

## Bridge

You have specifications. Chapter 9 addresses what happens when the specification is right and the output is *still wrong* — the dangerous middle, named.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Lovelace, A. *Notes on the Analytical Engine* (1843). Multiple scholarly editions; Note G is the key reference for the Bernoulli-number algorithm.
