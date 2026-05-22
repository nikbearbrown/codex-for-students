# Chapter 9 — Writing Codex Prompts That Are Specifications

*"Write me a login function" is not a prompt. A prompt names the thing, the invariants, the output format, and what not to touch.*

---

Here is a thing Ada Lovelace understood in 1843 that most people typing Codex prompts today have not internalized.

Lovelace was writing what we now recognize as the first computer program — an algorithm for computing Bernoulli numbers, written for Babbage's Analytical Engine before any physical machine existed that could run it. She did not write a request. She did not write "compute the Bernoulli numbers." She wrote a specification: every operation named, every dependency stated, every intermediate result accounted for. She specified it that way because she knew the machine could not interpret. The specification had to leave no room.

The problem in 2025 is the mirror image. Codex *can* interpret. That is precisely what makes vague prompts dangerous. When you write "add user authentication," Codex interprets. It fills in what "authentication" means from the average of everything it has ever seen. The average is not your project. The output is fluent, confident, and calibrated to someone else's situation.

The discipline of this chapter is simple to state: write prompts as specifications, not as requests. The rest of the chapter is the five elements that make a specification complete.

---

## What a Specification Is

A request names what you want. A specification names what you want with enough precision that the executor — human or machine — cannot reasonably misinterpret.

The difference is not length. A specification can be fifty words or two hundred. The difference is per-element completeness. A complete specification answers five questions before Codex runs, not after:

1. **What is the one operation?** Not "help me with X" — the single thing to do, stated as a verb acting on a named target.
2. **What must not change?** The existing files, tests, function signatures, interfaces that are off-limits.
3. **What context governs this step?** Which sections of AGENTS.md apply. Which existing files Codex should treat as templates.
4. **What does done look like?** Where the new code lives, what shape the change takes, what tests must exist.
5. **What must Codex not do?** The explicit negative constraints — "do not modify file X," "do not add a dependency," "do not change the interface."

Same task, two approaches:

> *"Add user authentication."*

> *"Implement a function `authenticate(username, password) → Optional[User]` in `auth/auth.py`. Use the existing `bcrypt` password hashing pattern from `auth/hashing.py`. Return the User if credentials match a row in the `users` table; return None otherwise. Do not modify the User class. Do not modify the database schema. Do not add a new dependency. Do not change `auth/hashing.py`. Add a unit test in `tests/auth/test_auth.py` for the success case, the missing-user case, and the wrong-password case."*

Same Codex. The five elements are the difference.

| Element | Weak prompt version | Specification version |
|---|---|---|
| Specific task | "Add user authentication." | Implement `authenticate(username, password) → Optional[User]` in `auth/auth.py`. |
| Invariants | (implicit — anything may be touched) | Do not modify the `User` class. Do not modify the database schema. Do not change `auth/hashing.py`. |
| Context | (none stated) | Use the existing `bcrypt` pattern from `auth/hashing.py`. Match against the `users` table. |
| Output format | (whatever Codex returns) | Return the `User` on match, `None` otherwise. Add `tests/auth/test_auth.py` covering success, missing user, and wrong password. |
| Negative constraint | (none) | Do not add a new dependency. Do not change interfaces touched by other modules. |

---

## Why Each Element Does What It Does

Each element is not bureaucracy. Each element protects against a specific, predictable class of failure. Let me go through them one at a time.

**The specific task** protects against Codex choosing a different verb than you meant. "Help me with auth" can become "redesign auth," "add OAuth," or "patch the existing flow." The model picks the most probable interpretation of your words, which is the most probable across its training data, which is not necessarily yours. Naming the operation — `implement authenticate(username, password) → Optional[User] in auth/auth.py` — removes the interpretation. There is now one verb, one function name, one file. Codex cannot reasonably produce anything else.

**The invariants** protect against Codex modifying code you did not ask it to touch. Without explicit invariants, the model may "improve" your existing User class as part of adding authentication — because improving related code is a natural thing to do, on average. The diff that lands in your review touches files you did not expect. The invariant statement is the protection: *the User class is unchanged. The database schema is unchanged.* Stating what must not change costs ten words. Discovering unexpected modifications in a diff costs much more.

**The context** is the link between this prompt and the accumulated project knowledge in AGENTS.md. You do not need to repeat everything in AGENTS.md — Codex has it loaded. You do need to indicate which sections govern this step. *Reference the Voice section. Reference the never-generate-grade rule.* Without this link, Codex may produce output that is correct for a generic project and wrong for yours, even with AGENTS.md present. The context element closes the gap between "loaded" and "applied."

**The output format** is your handoff condition written in prose. It names where the new code lives, what shape the change takes, what tests must exist. Writing the output format in advance forces you to decide what done means before Codex runs — and it makes it harder for Codex to declare something done that isn't. The most common form of this: *"A complete `articles/feedback.py`. A unit test in `tests/articles/test_feedback.py` covering at least: success, failure, empty input."* Codex cannot submit output that lacks the test file and call it complete.

**The negative constraint** is the most under-used element and the one that catches the dangerous middle most reliably. The dangerous middle is tasks that look like pattern work but require situational judgment — tasks where Codex produces fluent, plausible output that is wrong for your situation in a way that is invisible until later. Negative constraints are the operational form of the "never" rules in AGENTS.md, instantiated for this specific prompt. *Do not rewrite the article's prose.* That is one negative constraint. *Do not introduce a new dependency.* That is another. They feel like paranoia; they are not. They are the lessons from previous builds, written in the prompt instead of discovered in the diff.

| Element | Failure it prevents | Cost of omitting it |
|---|---|---|
| Specific task | Codex picks the wrong verb — "redesign auth" instead of "implement `authenticate()`" | A working artifact pointed at the wrong problem |
| Invariants | Codex "improves" code you didn't ask it to touch | A diff that touches files you didn't expect — sometimes irreversibly |
| Context | Output that's correct for the generic case and wrong for your project | The dangerous middle: fluent code that silently violates your AGENTS.md rules |
| Output format | Codex declares done before the tests, files, or shape are in place | "Complete" output you have to chase back through several prompts to actually finish |
| Negative constraint | Codex re-introduces a thing a previous build taught you not to do | Re-discovering an old lesson in the diff instead of asserting it in the prompt |

---

## Worked Example

The task: add a structural-feedback step to the article-review tool. Here is what the specification looks like in full.

**Prompt (don't do this):**

> "Add a feedback generator to the article tool."

**Specification:**

> **Operation:** Implement `generate_feedback(article, target) → str` in `articles/feedback.py`.
>
> **Invariants:** The `articles/target.py` module is unchanged. The `Article` and `Target` classes are unchanged. The tool's CLI entry point in `articles/__main__.py` is unchanged.
>
> **Context:** AGENTS.md sections on the "never" rules (specifically: NEVER rewrite the article's prose; the feedback is structural notes only). Reference `articles/scorer.py` for the existing pattern of producing per-section output. The Voice section of AGENTS.md applies — feedback is matter-of-fact, second-person, no encouragement boilerplate.
>
> **Output format:** A complete `articles/feedback.py` with the function defined. A unit test in `tests/articles/test_feedback.py` covering at least: an article that hits all target criteria; an article that misses two criteria (length, missing section); an empty article. The function returns a string under 500 characters.
>
> **Negative constraint:** Do not rewrite the article's prose in any form (no suggested sentences, no edited paragraphs, no replacement headlines). Do not modify `articles/scorer.py` or `articles/target.py`. Do not add a new dependency. Do not produce feedback that reads like a writing coach's pep talk (no generic "great voice!" or "keep going!").

That specification is roughly 150 words. The resulting Code Mode output is targeted: a function in the right file, following the project's voice rules, respecting the never-rewrite-prose rule, with tests covering the specified cases.

The weaker prompt — "add a feedback generator" — would have produced a function that might rewrite paragraphs (because that is the most probable thing a writing-feedback generator does, averaged across the training corpus), might modify scorer.py (because scorer.py is the most probable place to look for related code), and might produce generically encouraging feedback (because that is the most probable voice the model learned). Each failure is invisible until later. By then, the build has drifted.

The 150 words of specification are the protection against all three.

| Element | Weak prompt | Specification |
|---|---|---|
| Specific task | "Add a feedback generator to the article tool." | Implement `generate_feedback(article, target) → str` in `articles/feedback.py`. |
| Invariants | (none stated; `scorer.py` and `target.py` are fair game) | `articles/scorer.py`, `articles/target.py`, the `Article` and `Target` classes, and the CLI entry point in `articles/__main__.py` are unchanged. |
| Context | (none stated; default voice and conventions apply) | AGENTS.md NEVER-rewrite-prose rule applies. Reference `articles/scorer.py` for the per-section output pattern. Voice section: matter-of-fact, second-person, no encouragement boilerplate. |
| Output format | (whatever Codex returns) | Complete `articles/feedback.py`; `tests/articles/test_feedback.py` covering all-criteria-hit, two-criteria-miss (length + missing section), and empty article; return string under 500 characters. |
| Negative constraint | (none) | Do not rewrite the article's prose. Do not produce coach-style "great voice!" feedback. Do not modify `scorer.py` or `target.py`. Do not add a new dependency. |

<!-- → [IMAGE: Annotated diff view — left side shows the output from the weak prompt (grade generated, scorer.py modified, generic voice), right side shows output from the specification (no grade, scorer.py untouched, matter-of-fact voice). Callout annotations point to the three specific differences. Caption: "Same Codex. The specification is the entire difference."] -->

---

## What This Looks Like At Full Scale

The five elements do not stop applying when the build is bigger. They become more important.

Seth has written an Operation Detonation Game Design Document — a twelve-section production specification for a hypothetical cooperative shooter built around physics-driven environmental destruction. The document is roughly 6,000 words. It is structurally the same five elements as a 150-word Codex prompt, scaled up across an entire production. The Operation appears as a Vision Summary stating the one experience the game delivers. The Invariants appear as four Design Pillars, each with an explicit *VIOLATES* clause naming what the pillar refuses, plus a documented Pillar Collision Test that resolves the tension between two pillars in advance (*"Earned Spectacle versus Controlled Danger — Controlled Danger is primary in conflict"*). The Context appears as a cross-reference network: each Core Mechanic block names which pillars it serves and which loop positions it occupies, and any mechanic that fails to map to a Player Experience Goal gets flagged before it is documented further. The Output Format appears as a priority-tagged feature list (CORE / IMPORTANT / NICE-TO-HAVE / EXPERIMENTAL) with a hard rule — if CORE exceeds 40%, attempt re-prioritization; if it still won't fit, surface a cut-or-extend choice — and an MVP Specification naming exactly what the player experiences with CORE features only. The Negative Constraints appear as an Out-of-Scope section, each excluded item carrying a *REASON FOR EXCLUSION*, a *DECISION DATE AND OWNER*, and a *REOPEN CONDITION*.

The pattern does not change with scale. A studio that operates without a GDD-level specification is the production equivalent of a programmer who types *"add user authentication"* and accepts whatever comes back. The five elements are what protect the build from drift, whether the build is fifteen lines of `auth.py` or a thirty-six-month $200-million production.

---

## The Verification Move

There is a sixth practice worth naming separately, because it changes the output more dramatically than any single element.

OpenAI's engineers describe it this way: *"When Codex has some way to check its work — like run tests or screenshot the UI — it can iterate and get dramatically better results."*[^1] Including a verification command in the specification — a test suite to run, a lint check to pass, a specific output to produce — gives Codex a loop. It generates, checks, revises. The output you receive has already been iterated against your verification criteria before it surfaces.

The output format element is the specification of what done looks like. The verification move is the specification of how to *check* whether done has been reached. They compound. A specification with both a clear output format and an explicit verification command produces output that has been self-checked against your criteria before you see it.

This is not magic. The loop is limited by what the test suite covers. But it makes the specification into a feedback system rather than a one-shot request, and the difference in output quality is measurable.

<!-- → [DIAGRAM: Verification loop — Codex generates → runs verification command → output passes? → yes: surface to human / no: revise and rerun. Shows the loop the verification move creates vs. the one-shot flow without it.] -->

---

## What This Is Not

**It is not a requirement to write 200 words for every prompt.** The five elements scale with the stakes and complexity of the task. A typo fix in a CSS file: one sentence, one negative constraint, done. A new module with side effects touching shared state: the full format. The book's heuristic is rough but useful — skip the format for tasks whose entire description fits in one verifiable sentence. When the operation, the invariants, the context, the output format, and the negative constraints all fit in a single sentence naturally, they are already there.

**More words is not the same as a better specification.** Precision per element, not verbosity. A 50-word specification with each element specific is better than a 300-word one that buries the constraints in prose. The test is specificity per element, not total length.

**Editing after generation is not a substitute.** The dangerous middle is precisely the category where the generated code is not visibly wrong on review. It compiles. It passes the tests that were written with the same understanding as the code. The wrongness lives in the mismatch between the code's assumptions and your project's actual situation. Specification prevents the dangerous middle before the prompt runs. Editing after catches only the cases where the wrongness surfaces — which are not all of them.

**Negative constraints are not paranoia.** They are the operational form of the lessons from previous builds and the "never" rules in AGENTS.md. A negative constraint exists because something happened — in this build or a previous one — that taught you this boundary needs to be stated. Writing it in the prompt is cheaper than re-discovering it in the diff.

---

## The Structural Reason This Matters

There is a deeper principle underneath the five-element format, and it is worth naming because it explains why the format works and not just that it does.

Codex fills missing information with the most probable answer from its training data. This is not a flaw; it is how the system works. The most probable answer is often correct. The average programmer adding authentication does want OAuth. The average feedback generator does produce final grades. The average voice for student feedback is encouraging.

You are not the average. Your project has specific constraints. Your AGENTS.md has specific never-rules. Your article-review tool has a specific reason to never rewrite prose that has nothing to do with what most writing-feedback tools do.

The five elements are the mechanism for replacing Codex's defaults with your actuals. Each element takes one dimension of the task where Codex would otherwise fill in an average and replaces it with a specific. The specific task replaces the average interpretation of your intent. The invariants replace the average assumption about what is safe to modify. The context replaces the average project's conventions with yours. The output format replaces the average definition of done with yours. The negative constraint replaces the average behavior with the explicit boundary your project requires.

This is why vague prompts are not just inefficient — they are structurally incorrect. A vague prompt does not invite Codex to do its best; it invites Codex to produce output calibrated to a different project. Specificity is not style. It is the mechanism by which the prompt produces output for your situation rather than for the average situation.

<!-- → [INFOGRAPHIC: Five-element specification as five layers — each layer labels one element, with a one-line note on what default it replaces. Visual: five stacked horizontal bands, narrow to wide, representing the increasing specificity each element adds.] -->

---

## Lovelace's Lesson

It is worth returning to Lovelace, because the historical parallel is not decorative.

Lovelace specified the Bernoulli-number algorithm for a machine that could not interpret. Every operation had to be explicit because the Analytical Engine had no inference capability. The specification had to be complete because there was no fallback.

Codex has inference capability. Codex can interpret. And this is precisely what creates the failure mode. Lovelace's machine could not produce a plausible-looking wrong answer; it would simply halt or produce obvious nonsense. Codex produces plausible-looking wrong answers fluently and confidently. The wrongness is harder to see because the output is so well-formed.

The discipline she demonstrated — make every decision explicit, leave no interpretation room — is now more necessary, not less, because the cost of leaving interpretation room has changed. In 1843 the cost was a halted machine. In 2025 the cost is a silently wrong build that passes its tests.

The five-element format is Lovelace's discipline applied to a system that guesses too well.

---

## Exercises

**LLM Exercises**

1. **(Apply)** Take three Codex prompts from your recent history. Rewrite each as a five-element specification. Run both versions through Codex. Document three specific ways the outputs differ.

2. **(Analyze)** A provided specification is missing two elements. Identify which ones and explain, concretely, what Codex will do wrong as a result — not in general, but for this specific task.

3. **(Create)** Write a complete five-element specification for the next step in your current project. Apply the Chapter 7 gate: Ask Mode plan first, review, then Code Mode with the specification.

---

## What Would Change My Mind

The chapter's operational claim is that the five-element format produces materially better Codex output than free-form prompts. If a controlled comparison found no measurable difference in output quality, build time, or correction rate on matched tasks, the format softens from load-bearing discipline to useful first habit. The case for applying it to every consequential prompt weakens; the case for teaching it as a starting point holds.

I do not expect this result. The structural argument — that specificity replaces defaults with actuals — predicts the improvement, and the prompt-engineering literature broadly supports specificity. But the claim is empirical and the evidence is currently indirect.

---

## Still Puzzling

How much of the format can be omitted as models improve. Frontier-generation models handle vague prompts better than 2023-generation models did. Some elements may become less essential as model behavior matures. The book's prescription assumes the current state; the threshold for "when can I skip an element" is a moving target.

Whether there is a fast path for trivial prompts. A typo fix. A one-line adjustment. The five-element format feels heavy for these. The heuristic of "fits in one verifiable sentence" is directionally right but fuzzy at the boundary.

---

## AI Wayback Machine

🕰️ **Ada Lovelace** (1815–1852) — English mathematician who wrote what is widely recognized as the first computer program: an algorithm for computing Bernoulli numbers, specified as an explicit ordered sequence of operations with dependencies and side effects, written for Babbage's Analytical Engine before any machine existed that could run it.[^2] Lovelace did not write a request for the machine to compute Bernoulli numbers. She wrote a specification — every operation named, every dependency stated, every intermediate result accounted for. She wrote it that way because she knew the machine could not interpret or guess; the specification had to leave no room. The five-element format is Lovelace's discipline applied to AI-generated code. Codex can interpret and guess; that is precisely the problem the specification prevents. Lovelace specified for a machine that could not interpret; you specify for a CLI that interprets too freely. The discipline she demonstrated in 1843 — make every decision explicit, leave no interpretation room — is the discipline this chapter operationalizes.

---

## Bridge

You have specifications. The next chapter addresses what happens when the specification is right and the output is *still* wrong — the dangerous middle, and how to catch it.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Lovelace, A. *Notes on the Analytical Engine* (1843). Multiple scholarly editions; Note G is the key reference for the Bernoulli-number algorithm.
