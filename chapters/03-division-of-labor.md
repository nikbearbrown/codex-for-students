# Chapter 2 — What You're Actually Good At (And What Codex Is Better At)

> Pattern recognition is Codex's domain. Supervisory intelligence is yours. Knowing which is which is the whole game.

---

## Learning outcomes

1. **(Understand)** Distinguish pattern recognition (where AI is superhuman) from supervisory intelligence (where AI is structurally weak).
2. **(Apply)** Classify a set of build tasks as Codex work or human work.
3. **(Analyze)** Identify the specific supervisory capacity being exercised at a given step in a build.

---

## Opening

Seth asked Codex to write a function.

The function was a small utility — given a list of student submissions in a directory, return the ones that hadn't been modified in the last seven days. Codex generated something. The function compiled. The test cases Seth had written passed. The output looked correct.

Seth ran it on real data.

The output included a file that Seth knew had been modified yesterday. He looked closer. The function was reading file modification times from the operating system. On the file system Seth was using, the modification time reflected the *last time the content was written*, not the *last time the file was touched* — and the file he was looking at had been opened in an editor without changes saved, then closed. The file appeared modified to his eye (he had just been working on it) but was not modified in the sense his function was using.

The function was technically correct. The test cases were technically correct. The *meaning* of "modified" was where the gap lived. The CLI did not know what Seth meant by modified, because Seth had not been precise — and Seth had not been precise because, until the failure surfaced, he had not realized the term was ambiguous.

This is the chapter's question, in one example. Codex is superhuman at writing a function that detects file-modification-time changes. Codex is structurally blind to what Seth meant by "modified" for his specific use case. The gap between the two is where supervisory work lives, and it is the chapter's subject.

<!-- → [TABLE: Division of labor — two columns: Codex does / Human does. Rows: pattern completion, code generation, syntax resolution, test execution (Codex) vs. plausibility auditing, problem formulation, interpretive judgment, tool orchestration, executive integration (Human). No color.] -->

---

## What pattern recognition is

Codex was trained on an enormous corpus of code. It has seen Python functions that walk directories. It has seen file-modification-time checks. It has seen filter logic for "files older than N days." It has seen these patterns combined and recombined across millions of variations on Stack Overflow, GitHub, blog posts, library documentation.

When you ask Codex for "a function that returns files older than 7 days," it completes the pattern. The completion is informed by every variation it has ever seen. The result is, on average, more idiomatic and more correct than what a human would produce from memory — because the human has written maybe a few dozen such functions, and the CLI has effectively written a few million.

This is the CLI's domain. It is faster than you. It will stay faster than you. The gap will widen, not close.

What pattern completion does well:
- **Syntax.** The exact API calls, the right argument order, the proper exception handling for the language and library version.
- **Idiom.** The conventional way to structure the code. The expected loop pattern. The standard error-handling shape.
- **Recall.** Library calls and flags you forgot. Codex has the docs effectively memorized in a way you do not.
- **Translation.** Natural-language descriptions to code. Codex is genuinely good at this in a way that would have seemed implausible five years ago.

What pattern completion does *not* do:

Any of the work in the next section.

> "Codex excels at moving fast and covering ground." — OpenAI internal use doc, December 2025.[^1]

That is what the chapter argues about Codex. The next section is what the chapter argues about you.

---

## What supervisory intelligence is

**Supervisory intelligence** is the work of deciding which patterns are right *for your specific situation* — and noticing when something is wrong even though it looks right.

The CLI cannot do supervisory work, not because it is bad at it but because it does not have the information. The CLI cannot see:

- **Your project.** What other code is in the codebase. What conventions you've established that aren't visible in the snippet you pasted. What architectural decisions were made and what alternatives were rejected.
- **Your data.** What the inputs actually look like. What edge cases exist in your data that don't exist in the average dataset.
- **Your intent.** What "modified" means *in your context*. What "old" means. What "clean up" means. What success looks like for *this* user, *this* deadline, *this* purpose.
- **The consequence horizon.** Whether being wrong costs you five minutes or five days or affects someone else.

None of this is in the prompt. None can be in the prompt without you typing things you would not want to type repeatedly. The CLI is operating on enormous code knowledge and zero knowledge of *your specific situation*. The output is the most-probable answer to your prompt averaged across the people who might have typed it. You are not the average.

The five capacities (Chapter 5 names them formally):

1. **Plausibility Auditing.** Hearing the wrong note before verification catches it. The feeling Seth had when he saw the file in the output that he knew had been modified.
2. **Problem Formulation.** Deciding what the build IS before Codex sees it. The work upstream of any suggest.
3. **Tool Orchestration.** Choosing which Codex mode, in what order, with what context. Ask Mode vs. Code Mode; Best-of-N vs. single response.
4. **Interpretive Judgment.** Supplying meaning the CLI's output cannot supply. The "modified" disambiguation in the chapter opening.
5. **Executive Integration.** Holding the whole build toward a single goal across many prompts.

Each is irreducibly yours. None has a horizon on which a model release closes it, because the model does not have the information to do the work.

---

## The solve-verify asymmetry

A practical asymmetry worth naming.

Codex generates faster than you can verify. It writes a function in two seconds. Verifying that the function does what you actually need — reading the code, considering the edge cases, checking against the real data — takes longer than that. The gap is not narrowing; it is widening as Codex gets faster.

The asymmetry has an operational implication: your time during AI-assisted work is *almost entirely* verification time. The generation is essentially free. The verification is the bottleneck.

This reframes what good practice looks like. The student who tries to keep up with Codex's generation pace by skimming verification is operating against the asymmetry. The student who slows down at the verification step — running explain or asking Codex to walk through the code, checking against edge cases, predicting the consequence — is using their scarce resource where it matters.

<!-- → [DIAGRAM: The solve-verify asymmetry — simple timeline. Codex's solve speed increasing over time. Human verification capacity stable. The gap widens. The human's job is not to solve faster but to verify better.] -->

---

## The dangerous middle

The category that fits neither pure pattern work nor pure supervisory work.

Tasks that *look* like pattern work — the kind of task you expect Codex to handle — but that *require* supervisory judgment to do correctly. These are the **dangerous middle**.

A function to "filter old files" looks like pattern work. Codex generates a working function. The function uses file-modification-time, because that is the most common interpretation of "old." If your situation requires a different interpretation (file age since first creation, or time since last actual write to disk, or some project-specific notion of "no longer in use"), the function is wrong *for you* in a way that is invisible until your specific case surfaces.

A function to "validate user input" looks like pattern work. Codex generates input-validation code. The code rejects empty strings, very long strings, strings with special characters. If your situation requires accepting non-ASCII names (because your users have non-ASCII names) or rejecting specific patterns Codex would not have anticipated (because of your particular threat model), the code is wrong *for you*.

A function to "calculate the average" looks like pattern work. Codex generates `sum(xs) / len(xs)`. If your data contains outliers that should be excluded, or if the empty-list case should return a sentinel rather than raise, or if "average" in your domain means median, the function is wrong *for you*.

The dangerous middle is the largest category of failure in AI-assisted code. The output runs. The tests (which Codex may have written, against the same understanding it had when it wrote the code) pass. The wrongness surfaces only when your specific case hits — sometimes weeks or months after the code shipped.

Chapter 9 owns the dangerous middle as its full chapter. For now, recognize the shape: tasks where pattern completion *gets you most of the way* and *misses the part that matters for your situation*. Those are the tasks where the conducting discipline — supervisory capacities exercised explicitly — is most essential.

---

## Worked example: the same step, two outcomes

Same task: build a function that takes a list of student CSV submissions and produces a summary table of word counts per submission.

**Run one: Codex unattended.**

Seth types the request. Codex generates a function using `pandas` to read CSVs and `apply` to compute word counts. The function returns a DataFrame. Seth tests it on three sample CSVs. The counts look right. He uses the function in his project.

A month later, a submission with unusual quoting causes the `pandas` CSV reader to misinterpret column boundaries. The word counts for that submission are wildly wrong. Seth does not notice until the affected student questions the count on their submission. Seth realizes the bug. The fix takes an afternoon (debugging, root cause, regenerating the affected counts, apologizing).

**Run two: Codex with the supervisory capacities exercised.**

Seth types the same request. Codex generates the same function. Then Seth does the supervisory work.

**Plausibility audit.** Does the function feel right? Seth notices it uses `pandas` for what could be done with the CSV stdlib. Why? Codex defaults to `pandas` for tabular data; the choice is reasonable for most cases but Seth's case is simple. PA fires: investigate.

**Problem formulation revisit.** Seth's actual task: word counts for student feedback. Does he need the full `pandas` machinery? No — he needs to count words. PF revisits: a simpler function would be more reliable and easier to debug.

**Tool orchestration.** Seth chooses to ask Codex to rewrite the function using the standard library, with explicit handling of quoting and empty cells. The Ask Mode interrogation surfaces edge cases: non-UTF-8 files, embedded newlines in fields, empty rows. Seth notes these as test cases to add.

**Interpretive judgment.** What is a "word"? Codex's default is split-on-whitespace, which counts hyphenated terms as one. Seth's rubric counts hyphenated terms as one too — so the default matches his domain. He notes this as a `WORD_DEFINITION` comment in the function so future-Seth (or another teacher) does not have to re-derive it.

**Executive integration.** The function is part of a larger grading workflow. Seth checks that the function's output shape (a list of dicts with `submission_id`, `word_count`, and `errors`) matches what the downstream summary script expects. It does.

The function ships. A month later, the unusual-quoting submission comes through. The function's explicit error-handling catches the parsing issue and reports it; the summary script flags the submission for manual review. Seth catches it before the affected student's grade is finalized.

**The lesson:** pattern completion produced a function. Supervisory intelligence produced a function that did not break silently a month later.

**The limit:** the supervisory work took longer upfront. The fifteen extra minutes saved Seth an afternoon of debugging and an apology to a student. The math is favorable; the friction is real.

---

## Common misconceptions

**"With enough context in the prompt, Codex can do the supervisory work."** Partial. More context narrows Codex's guess; it does not eliminate the gap between the most-probable answer and the right answer for your specific situation. The supervisory work depends on knowledge you have not put in the prompt and would not want to put in every prompt.

**"This is just being careful."** Carefulness without structure is unreliable. The chapter is not arguing you should be more careful; it is arguing that the supervisory work has *named operations* that you can practice deliberately. Vague carefulness misses the dangerous middle.

**"Pattern completion will get good enough that supervisory judgment isn't a separate skill."** No. Pattern completion is about *averages over a corpus*; supervisory judgment is about *your specific situation*. The two are categorically different. Improving pattern completion does not give the model access to your project, your data, your intent.

**"I'll spot the dangerous middle when it happens."** Sometimes. The whole point of *silent* wrongness is that you cannot reliably spot it without the discipline. The discipline is what makes the spotting reliable.

---

## Exercises

1. **(Apply)** Given the following ten code tasks, classify each as **Codex work** (pure pattern completion), **human work** (pure supervisory judgment), or **dangerous middle** (pattern work with supervisory hazard). Defend each classification.
   - Implementing the Fibonacci function.
   - Designing the API for a new user-facing feature.
   - Adding TypeScript types to an existing JavaScript function.
   - Choosing the right data structure for a graph traversal problem.
   - Writing a regex to match phone numbers in your country.
   - Refactoring a long function into smaller ones.
   - Deciding whether a function should be sync or async.
   - Writing unit tests for an existing function.
   - Picking a sorting algorithm for a list of 10,000 items.
   - Filtering "old" files from a directory.

2. **(Analyze)** Read a provided Codex transcript (or one from your own history). At each step, identify the moment where a supervisory capacity should have been exercised but wasn't. Trace what would have gone wrong as a result.

3. **(Create)** Write your own labor separation rule for a project you are currently working on. The rule should be specific to the project: name which kinds of tasks you will delegate to Codex without explicit supervisory review and which kinds you will always review.

---

## What would change my mind

The chapter's strong structural claim is that **the pattern-completion / supervisory-intelligence division is categorical, not gradient** — that no improvement in pattern completion makes supervisory work a different problem. If a 2027 or later system could demonstrate reliable supervisory judgment in domains where it does not have access to the user's project context — perhaps through some form of context inference I am not anticipating — the categorical framing softens to gradient. The chapter would still hold (Codex is much weaker than humans at supervisory work), but the framing would be "the gap is narrowing" rather than "the gap is structural."

I think this is unlikely on the next-edition timeline because the structural argument (the model does not have the information) is hard to engineer around without giving the model project access — which has its own dangers and is not the path most agentic-coding tools are taking.

---

## Still puzzling

- **Where exactly pattern completion shades into supervisory work.** The chapter draws the line crisply for teaching. In practice, the line is fuzzier. Some tasks have *some* supervisory work the CLI can guess at and *some* it cannot.

- **Does the asymmetry hold for Codex variants that read the whole project?** Codex's Ask Mode can read the codebase before generating. This narrows the supervisory-judgment gap somewhat. Whether it narrows enough to change the chapter's argument is open; the book's working answer is no (intent is still not in the project files).

- **Whether some students are systematically better at supervisory work.** The book assumes the discipline is teachable. Some individuals will exercise the capacities more naturally; the book's bet is that everyone can develop them with practice.

---

## AI Wayback Machine

🕰️ **Frederick Winslow Taylor** (1856–1915) — American mechanical engineer who was the first systematic analyst of the division of labor between human judgment and mechanical execution. Taylor's project, *The Principles of Scientific Management* (1911), was to study work tasks and identify which components belonged to human cognition and which to mechanical (or organizational) execution.[^2] Taylor's legacy is complicated — the time-and-motion studies he pioneered were often used to dehumanize labor — but his analytical question is the right one for this chapter: *which cognitive work belongs to which agent?* For AI-assisted programming, Taylor's question becomes: which work belongs to Codex, which to the human, and what is the dangerous middle in between? The chapter's answer (pattern completion to Codex; supervisory intelligence to the human) is Taylor's framework applied to a tool he could not have anticipated. *(The pantry research recommended swapping Taylor for Lillian Gilbreth — Taylor's wife's collaborator and the humane half of the partnership — on grounds of both substantive fit and diversity. Either is defensible; the book uses Taylor here for series consistency.)*

---

## Bridge

You can name the capacities. Chapter 3 explains why school isn't teaching them — and why, at this specific moment in AI-assisted education, the technically fluent student is on their own.

---

[^1]: OpenAI engineers, "How OpenAI Engineers use Codex to Tackle Big Projects with Rigor" (forum.openai.com, December 4, 2025).
[^2]: Taylor, F. W. *The Principles of Scientific Management*. Harper & Brothers, 1911.
