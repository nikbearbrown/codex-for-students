# Chapter 6 — AGENTS.md: Your Coding Constitution

> AGENTS.md is the file Codex reads at the start of every session. It is the difference between a Codex that knows your project and a Codex that guesses.

---

## Learning outcomes

1. **(Understand)** Explain what AGENTS.md is, where it lives, and when Codex reads it.
2. **(Apply)** Write an AGENTS.md for a student build project using the five-element format.
3. **(Analyze)** Distinguish AGENTS.md content (always-on rules) from task-queue items (session-specific) and Best-of-N selection criteria (judgment layer).

---

## Opening

You start your second Codex session on the same project.

Yesterday you spent twenty minutes telling Codex about your project — the structure, the conventions, the things to avoid. By the end of the session, Codex's outputs were good. Today, Codex has no memory of yesterday. You type the same context into your first prompt. By the third prompt you are repeating yesterday's context. By the fifth, you have spent twelve of your thirty Monday-night minutes telling Codex things you told it yesterday.

This chapter ends that.

The file is **AGENTS.md**. It is a markdown file Codex reads automatically at the start of every session — the project conventions, the architectural decisions, the things Codex must not do. It is loaded into Codex's context before your first prompt. The context yesterday produced now persists across sessions, embedded in a file that lives with the project.

AGENTS.md is the simplest and most-undervalued affordance Codex provides. Most students do not know about it. Most students who know about it write a thin one once and never update it. Students who maintain AGENTS.md across builds report substantially less repetition and substantially fewer "Codex did the wrong thing because it didn't know X" failures.

This chapter is how to write and maintain it.

<!-- → [DIAGRAM: AGENTS.md in the session context — loaded at session start, persists across the session, governs every prompt. Contrast: without AGENTS.md (Codex guesses) vs. with AGENTS.md (Codex knows). Editorial style.] -->

---

## What AGENTS.md is

A markdown file Codex reads at the start of every session.

**Where it lives.** Three locations, in order of precedence:

- `./AGENTS.md` in your project root (most common; specific to this project).
- `.claude/AGENTS.md` or similar tool-specific paths if your editor expects them.
- `~/.codex/AGENTS.md` in your home directory (your personal defaults, applied to every project; less commonly used).
- For organizational use: a managed policy location for district-wide rules.

For student work, the project-root `AGENTS.md` is the one that matters. Place it at the root of your project directory. Commit it to git.

**When Codex reads it.** At the start of every session. Whenever you open Codex in the project directory, AGENTS.md is in context before your first prompt. You do not need to paste it. You do not need to reference it. Codex sees it automatically.

**How big it should be.** Under 200 lines. Codex's technical limit is higher (32 KiB combined across the hierarchy), but the *operational* limit beyond which Codex starts ignoring instructions is much lower. Practitioner consensus, supported by OpenAI's own guidance: **keep it tight.** A focused 80-line AGENTS.md gets followed more reliably than a 500-line one that contains everything you could think of.

---

## The five-element format

A useful AGENTS.md for a student build has five categories of entry. None of them are required to be a separate section, but each one should appear somewhere.

**1. Stack and build commands.** What language, what framework, how to run, how to test. The specific commands. Codex can infer some of this from your code, but the explicit form gets followed more reliably.

```markdown
## Stack
- Python 3.11
- pytest for tests
- ruff for linting
- run tests with `make test`
- run the app with `python -m app`
```

**2. Code style deviations.** Standard conventions Codex already knows. The deviations from those conventions are what to write down. If your project uses 4-space indentation in Python, do not write that — Codex knows. If your project uses snake_case for one specific reason and you do not want Codex "fixing" it to camelCase, write that.

```markdown
## Style
- snake_case for variables (yes, even for one-letter throwaways).
- Type hints required on public functions; optional on helpers.
- f-strings, never .format() or % formatting.
- No `from x import *` ever.
```

**3. Architectural decisions.** Decisions you have made that you do not want Codex to revisit every session. "We use SQLite, not Postgres." "All requests go through the central client; no direct HTTP calls in feature modules." "The data layer is one module; no business logic in there."

```markdown
## Architecture
- Single-file scripts in `scripts/`; importable modules in `src/`.
- The `grading` module is the only one that touches student data; other
  modules call into it but do not parse submissions themselves.
- No background tasks for now (no celery, no cron); cron the script externally.
```

**4. Environment quirks.** Things about your machine or your school environment that affect what works. Old library versions. Missing tools. Permissions issues. Anything that has bitten you before and might bite you again.

```markdown
## Environment
- macOS; default `python` is 3.9. Use `python3.11` explicitly.
- School machine does not have node; do not propose JS tooling.
- No write access to /tmp; use ./tmp/ instead.
```

**5. Lessons learned.** This section grows over time. Each entry: a date, the mistake, the fix. By month six, this section is the most valuable part of the file.

```markdown
## Lessons learned
- 2026-03-19: Codex generated code that bypassed the central client and made
  direct HTTP calls in a feature module. Reverted. Rule added to Architecture
  section above.
- 2026-04-02: Codex assumed grading rubrics were stored in YAML. They are in
  JSON. Specified in Stack section above.
- 2026-04-15: Codex iterated on a failing test by inserting `try/except` to
  pass the test. The exception was hiding a real bug. New rule: do not add
  exception handling to make tests pass; fix the underlying bug.
```

---

## What NOT to include

A common failure mode is the AGENTS.md that contains too much. Things that do not belong:

- **Things Codex can figure out from the code.** If the project uses pytest and the `Makefile` references it, Codex does not need a line about pytest in AGENTS.md. Codex reads the Makefile. Save the AGENTS.md for things Codex *cannot* infer.
- **Standard conventions.** Codex knows that Python uses snake_case by default. Save the AGENTS.md for the deviations.
- **Constantly changing state.** Do not put "the current sprint is on the auth feature" in AGENTS.md. Sprints change. Put it in your prompt for the session.
- **Secrets, credentials, API keys.** AGENTS.md goes in git. Anything sensitive belongs in a `.env` file that is not committed, or in your shell environment.
- **Personal preferences unrelated to the project.** Your preference for vertical-bar separators in markdown tables belongs in your personal `~/.codex/AGENTS.md`, not the project AGENTS.md.

The discipline: **does Codex need to know this *every* session in *this* project?** If yes, AGENTS.md. If no, prompt or omit.

<!-- → [TABLE: AGENTS.md include/exclude — two columns. Include: bash commands, code style deviations, test runners, architectural decisions, quirks. Exclude: what Codex can figure out, standard conventions, changing content. No color.] -->

---

## AGENTS.md vs. task queue vs. Best-of-N selection criteria

A distinction worth being precise about, because the three things can blur.

**AGENTS.md is persistent project knowledge.** Rules, conventions, lessons learned. True *every session*. Stable. Versioned in git. Slowly accumulating.

**The task queue is session-specific work.** Codex's task queue (when you use the Codex app) holds the things you want Codex to do *this session* — including parallel tasks running in background containers. Items in the queue are transient; they complete or get reassessed. The queue is the per-session agenda.

**Best-of-N selection criteria are judgment-layer notes.** When you generate multiple Codex responses (Chapter 4) and select among them, the *criteria* by which you select are not AGENTS.md content (they are session-specific) and not task-queue content (they are about how you judge, not what you do). They are notes you make in your own build log or in a scratch file: "for this design step, I chose the simpler approach because the team can debug it without me; I chose against the more efficient approach because efficiency is not the bottleneck."

A simple test: if you are about to add something to AGENTS.md and your second thought is "but this is just for this week" — it does not belong there. It belongs in your prompt, in the task queue, or in your build log.

---

## How to start: `/init` and refine

Codex has a starter command. In a project directory, run `/init` (in the Codex CLI) and Codex will read your codebase and produce a first-draft AGENTS.md. Run it on day one of any project that does not have one.

The starter will be imperfect. It will guess at conventions that are not yet visible. It will miss things you know that are not in the code. That is fine. **Treat `/init` as the rough draft. Your refinement is the work.**

A typical refinement pass:

1. Run `/init`. Read what Codex proposed.
2. Delete anything Codex obviously already knows from the code.
3. Add the architectural decisions you have made that aren't visible.
4. Add the environment quirks.
5. Start the lessons-learned section empty.
6. Save. Commit to git.

Total time: 20–30 minutes the first time. Less for subsequent projects. This is the upstream investment that pays back every session for the life of the project.

---

## Maintenance over time

The lessons-learned section is the most valuable part of the file, and it only stays valuable if you update it.

The discipline: at the end of every significant session, take two minutes to add a lessons-learned entry. Date, mistake (or near-mistake — count the catches), fix.

The bar for "significant" is low. Did Codex generate something the plan-review caught? Entry. Did you notice a default Codex assumed that you needed to override? Entry. Did the dangerous middle (Chapter 9) surface in a way you would not have anticipated? Entry.

The two minutes are the upstream investment that protects every subsequent session. By the end of the semester, the lessons-learned section is the most useful reference for the project.

---

## Common misconceptions

**"AGENTS.md is documentation for Codex."** It is written by humans and read by Codex (and by the next human who works on the project). The writing is for *you* — articulating the project's rules forces you to make them explicit. Codex is the secondary beneficiary.

**"More content is better."** No. The 200-line ceiling is real. A focused 80-line AGENTS.md beats a 500-line one. If your file grows past 200 lines, prune.

**"I'll start AGENTS.md when the project is bigger."** The smallest project benefits. Codex's first session on the project is *also* the first session that benefits from knowing your conventions. Start now.

**"`/init` is sufficient."** `/init` is the starter. The refinement is the discipline.

**"AGENTS.md replaces the supervisory capacities."** No. AGENTS.md gives Codex the context Codex did not have. It does not replace your job to review, audit, judge, and integrate. The supervisory capacities still fire on every step.

---

## Exercises

1. **(Apply)** Write an AGENTS.md for your class website project (or a current project). Use `/init` to start; refine. Five-element format. Under 200 lines. Commit to git.

2. **(Analyze)** Run Codex without your AGENTS.md, then with it, on the same task. Document three specific differences in the generated output. (You can rename the file temporarily to test.)

3. **(Evaluate)** After one week of use: which entries is Codex ignoring? Why? Fix one entry that isn't being followed.

---

## What would change my mind

The chapter's central operational claim is that **AGENTS.md materially improves Codex output quality** when maintained properly. If a controlled comparison — same project, same prompts, with and without AGENTS.md — showed no measurable difference in output quality or in session length, the artifact becomes useful for documentation but not for output quality. The chapter would still recommend it; the case for the discipline overhead weakens.

OpenAI's own documentation, the broader practitioner literature, and personal experience all converge on the claim that AGENTS.md helps materially. A clean negative result would be surprising but not impossible.

---

## Still puzzling

- **When AGENTS.md hurts more than it helps.** Anecdotally, bloated files cause Codex to ignore instructions. The threshold is fuzzy. 200 lines is the working rule of thumb; the actual cliff is undocumented.

- **`AGENTS.override.md` and managed-policy locations.** Some enterprise Codex deployments enforce district-level AGENTS.md from a managed-policy location, and individual `AGENTS.override.md` files take precedence when present. The book's primary reader is the individual student; managed-policy is out of scope.

- **AGENTS.md vs. Skills.** Codex Skills (2025+ feature) are reusable workflow templates, adjacent to AGENTS.md. The book uses AGENTS.md as the spine; Skills appear in the *Claude Code* sister books under a different name. Whether Codex Skills will become central to student workflows in a near-term edition is open.

---

## AI Wayback Machine

🕰️ **Donald Knuth** (born 1938) — computer scientist who created TeX and articulated the principle of **literate programming**: that a program should be written for humans first, with the code as a side effect, so that the human reader and the machine reader are served by the same artifact.[^1] In Knuth's 1984 *Literate Programming*: *"Instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want a computer to do."* AGENTS.md is literate programming applied to AI collaboration. The artifact you write — the five-element format, the lessons-learned entries, the never-rules — is read by you (when you refer to it), by your future collaborators (when you share the project), and by Codex (automatically, every session). The discipline of writing for *all three readers at once* is what makes AGENTS.md valuable. Knuth's argument was that the human-readable explanation is the primary artifact and the executable code is the by-product. The form scales to AI-readable project context.

---

## Bridge

You have AGENTS.md. Codex knows your project at every session. Chapter 7 teaches the discipline upstream of the suggest prompt: problem formulation.

---

[^1]: Knuth, D. E. "Literate Programming." *The Computer Journal* 27, no. 2 (1984): 97–111.
