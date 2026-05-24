# Chapter 7 — AGENTS.md: Your Coding Constitution
*The twenty-minute file that stops you from repeating yourself across every session for the life of the project.*

> AGENTS.md is the file Codex reads at the start of every session. It is the difference between a Codex that knows your project and a Codex that guesses.

---

Here is a thing that happens to almost every student who uses Codex across multiple sessions on the same project.

The first session goes well. You spent twenty minutes at the start explaining the project — the structure, the conventions, the things to avoid, the decision to keep the inventory autoload untouched, the fact that you migrated this project from Unreal and Codex should never propose reintroducing Unreal-style components. By the end of the session, Codex's outputs were good. The context you had built up was doing its job.

The next session, Codex has no memory of any of it.

You type the same context into your first prompt. Codex generates something that touches the inventory autoload you explained yesterday. You correct it. By the third prompt you are repeating the same constraints again. By the fifth, you have spent twelve of your thirty Monday-night minutes re-establishing context you already established. The session is shorter than the last one. The outputs are weaker because the context never fully rebuilt.

There is a fix. It is a single file. It costs twenty minutes to set up and two minutes per session to maintain, and once it exists you never repeat yourself across sessions again.

The file is **AGENTS.md**.

---

## What AGENTS.md actually is

AGENTS.md is a markdown file that lives in your project root and is loaded into Codex's context automatically before your first prompt in every session. You do not paste it. You do not reference it. Codex reads it because it is there.

It holds the things that are true about your project every session: the stack, the conventions, the architectural decisions you have already made, the environment quirks, and — over time — the lessons learned from every session where Codex did something wrong and you caught it.

The context you are currently rebuilding by hand at the start of each session is exactly the content that belongs in this file.

![Central AGENTS.md card connected by hairline arrows to three Codex session cards — Monday feature work, Wednesday bug hunt, Friday refactor pass. Each session inherits the same persistent project knowledge. A dashed side note labeled WITHOUT AGENTS.MD shows the alternative: each session re-establishes context, twelve of thirty minutes lost to repetition.](images/07-agents-md-fig-01.png)

*Figure 7.1 — AGENTS.md at the center of every session. Codex loads it before the first prompt; you stop repeating yourself across sessions.*

Three locations matter, in order of precedence. The project-root `./AGENTS.md` is the one you write; it is specific to this project and is the one that covers the vast majority of student use. A home-directory `~/.codex/AGENTS.md` holds your personal defaults and applies to every project. Organizational deployments can enforce a managed-policy location. For student work, the project root is what matters. Put it there. Commit it to git.

One number to know: keep the file under 200 lines. The technical limit is much higher — 32 KiB combined across the hierarchy — but the *operational* limit, the point at which Codex starts ignoring entries, is closer to 200 lines of actual content. A focused 80-line AGENTS.md gets followed more reliably than a 500-line one. The discipline is not to add everything you could think of; it is to add the things Codex needs to know every session and cannot figure out from the code.

---

## The five things that belong in it

A useful AGENTS.md for a student build has five categories of entry. None of them require a specific section heading; each one should appear somewhere.

**Stack and build commands.** What language, what framework, how to run, how to test. The specific commands. Codex can infer some of this from your code, but the explicit form gets followed more reliably and removes the guessing.

```markdown
## Stack
- Godot 4.3, GDScript (no C#).
- GUT for unit tests.
- run tests with `godot --headless -s addons/gut/gut_cmdln.gd`
- run the game with `godot --path . scenes/main.tscn`
```

**Code style deviations.** Standard conventions Codex already knows. Write down the *deviations* from those conventions — the things Codex will default away from if you don't tell it otherwise. If your project uses snake_case for GDScript files, do not write that; Codex knows. If your project uses PascalCase for autoload singletons specifically and you do not want Codex "fixing" it, write that.

```markdown
## Style
- snake_case for variables and methods; PascalCase for autoload singletons.
- Type hints required on every public method; optional on locals.
- Signals declared at top of file, named in past tense (`died`, `picked_up`).
- No `preload()` inside `_ready()` — declare at file scope or use `load()`.
```

**Architectural decisions.** Decisions you have already made that you do not want revisited every session. "Inventory is an autoload; do not refactor it into a component." "All networked state changes go through the `NetSync` autoload; no direct RPCs in scene scripts." "Save-game schema is versioned; never break field names without a migration." These are the decisions that, if Codex ignores them, produce a refactor you did not ask for.

```markdown
## Architecture
- Player scenes in `scenes/player/`; AI behavior trees in `scripts/ai/`.
- The `Inventory` autoload is the only system that mutates player items;
  other scripts call into it but do not modify the inventory dict directly.
- Save schema is versioned (`save_version: int`); never rename fields
  without writing a migration in `scripts/save/migrations.gd`.
```

**Environment quirks.** Things about your machine or build environment that affect what works. Old library versions. Missing tools. Permissions issues. Anything that has already bitten you.

```markdown
## Environment
- macOS; Godot 4.3 binary lives at `/Applications/Godot.app/Contents/MacOS/Godot`.
- Windows export template required for co-op playtests; mac builds for solo only.
- Do not propose `.NET` / C# tooling — this project is GDScript-only.
```

**Lessons learned.** This section starts empty and grows. Each entry: a date, the mistake or near-miss, the fix. By month six, this section is the most valuable part of the file — a running record of every place where Codex's default behavior diverged from what your project needed and you caught it before it cost you.

```markdown
## Lessons learned
- 2026-03-19: Codex generated code that bypassed the Inventory autoload
  and mutated the player's item dict directly in a pickup script.
  Reverted. Rule added to Architecture above.
- 2026-04-02: Codex assumed save files were JSON. They are Godot resource
  files (.tres). Specified in Stack above.
- 2026-04-15: Codex iterated on a failing GUT test by inserting try/except
  to pass the test. The exception was hiding a real bug in the AI behavior
  tree. New rule: do not add exception handling to make tests pass; fix
  the underlying bug.
```

| Include in AGENTS.md | Exclude from AGENTS.md |
|---|---|
| Exact bash commands (run, test, lint, export) for this project | Anything Codex can read off `project.godot`, `package.json`, or the repo itself |
| Code-style deviations from the language default | Standard conventions Codex already knows (e.g. GDScript uses snake_case) |
| Test runner, framework, and how to invoke it | The current sprint or whatever feature you happen to be on this week |
| Architectural decisions and load-bearing invariants | Secrets, API keys, credentials — those go in an uncommitted `.env` |
| Quirks: filesystem oddities, OS-specific paths, "do not propose X" rules | Personal preferences that aren't project-specific (those belong in `~/.codex/AGENTS.md`) |
| Lessons learned — dated entries for past misses and the rule that prevents them | Constantly changing state, status updates, or session-level context |

---

## The things that do not belong in it

A common failure mode is the AGENTS.md that contains too much. It fills with entries until Codex stops reading the whole file, and then the entries that actually matter stop getting followed.

Things that do not belong:

Things Codex can figure out from the code. If the project uses GUT and the `project.godot` file already declares the GUT plugin, Codex does not need a line about GUT in AGENTS.md. It reads the project file. Save AGENTS.md for what Codex *cannot* infer.

Standard conventions. Codex knows GDScript uses snake_case by default. The AGENTS.md is for the deviations.

Constantly changing state. "The current sprint is on the auth feature" does not belong in AGENTS.md. Sprints change. Put it in your prompt for the session.

Secrets, credentials, API keys. AGENTS.md goes in git. Anything sensitive belongs in a `.env` file that is not committed.

Personal preferences unrelated to the project. Your preference for vertical-bar separators in markdown tables belongs in your personal `~/.codex/AGENTS.md`, not the project file.

The discipline for any candidate entry: *does Codex need to know this every session in this project?* Yes: AGENTS.md. No: prompt or omit.

---

## The distinction worth being precise about

AGENTS.md can blur into two other things you might be reaching for, and the blur is worth untangling.

**AGENTS.md is persistent project knowledge.** Rules, conventions, lessons learned. True every session. Stable. Versioned in git. Slowly accumulating over the life of the project.

**The task queue is session-specific work.** Codex's task queue holds the things you want Codex to do *this session* — the parallel tasks running in background containers, the specific file to modify, the test to make pass. Items in the queue are transient; they complete or get reassessed. The queue is the per-session agenda.

**Best-of-N selection criteria are judgment-layer notes.** When you generate multiple Codex responses and choose among them, the criteria by which you choose are neither AGENTS.md content (not every-session rules) nor task-queue content (not things to execute). They are notes you make in your build log: "I chose the simpler approach because the team can debug it without me; I rejected the more efficient approach because efficiency is not the bottleneck." These belong in the build log, not in AGENTS.md.

A simple test: if you are about to add something to AGENTS.md and your second thought is "but this is just for this week" — it does not belong there.

---

## How to start

Codex has a starter command. In a project directory, run `/init` in the Codex CLI and Codex will read your codebase and produce a first-draft AGENTS.md. Run it on day one of any project that does not already have one.

The starter will be imperfect. It will guess at conventions not yet visible. It will miss things you know that are not in the code. That is expected. Treat `/init` as the rough draft. Your refinement is the work.

A typical refinement pass: run `/init`, read what Codex proposed, delete anything Codex obviously already knows from the code, add the architectural decisions that are not visible in the code, add the environment quirks, start the lessons-learned section empty. Save. Commit to git. Total time the first pass: twenty minutes. Less for subsequent projects.

This is the upstream investment. It pays back every session for the life of the project.

---

## Maintenance over time

The lessons-learned section is the most valuable part of the file, and it only stays valuable if you update it after significant sessions.

The discipline: at the end of every session where something went wrong — or where you caught something before it went wrong — take two minutes to add an entry. Date, mistake or near-miss, fix. The bar for "significant" is low. Codex generated something the plan-review caught? Entry. You noticed a default Codex assumed that you needed to override? Entry. The dangerous middle from Chapter 9 surfaced in a way you had not anticipated? Entry.

The two minutes are an upstream investment. By the end of the semester, the lessons-learned section is the most useful reference for the project — a record of every divergence between Codex's defaults and your project's actual needs.

One more maintenance discipline: if an entry stops getting followed, it is either buried in a file that is too long, or it is worded in a way that Codex is pattern-matching around rather than applying. Fix it. Rewrite it. Move it higher in the file. AGENTS.md is a living document; it is not set-and-forget.

For full-strength reference implementations of the AGENTS.md form — what the file looks like when the discipline is taken all the way — see the appendix (`chapters/98-appendix-walker-and-zelda.md`). Walker and Zelda are two production agent prompts Seth and I co-built, public at humanitarians.ai/tools. Walker is a senior Unity architect for legacy-codebase refactoring; Zelda is a senior game-design consultant with a 34-command library. Both are structured exactly as this chapter describes: stack, style, architectural decisions, environment, lessons learned — extended into phase gates, never-rules, and explicit refusals. They are AGENTS.md at full strength. Read them when you want to see what the form scales to.

---

## The Knuth connection

There is a deeper principle behind AGENTS.md that is worth naming, because it changes how you think about what you are writing.

Donald Knuth introduced literate programming in 1984 with the argument that a program should be written for humans first, with the code as a side effect. His formulation: *instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want a computer to do.*[^1] The human-readable explanation is the primary artifact. The executable form is the by-product.

AGENTS.md is literate programming applied to AI collaboration. The file is read by three audiences simultaneously: by you, when you refer to it mid-project; by future collaborators, when they join the project and need to understand its conventions; and by Codex, every session. Writing for all three readers at once produces a different file than writing for Codex alone. It forces you to articulate the reasoning behind the architectural decisions, not just the decisions themselves. The entry that says "all requests go through the central client; no direct HTTP calls in feature modules" is better than an entry that just says "no direct HTTP calls" because the reasoning is in the entry — a future collaborator (or a future you) can understand why, not just what.

The discipline of writing for three readers at once is what makes AGENTS.md more than a workaround for Codex's statelessness. It is a clarity exercise. Every entry you write for Codex forces you to make an implicit convention explicit. The artifact that results is useful for the project even if Codex's statelessness problem were solved tomorrow.

![Two panels comparing the same AGENTS.md rule. Left panel — LITERATE — PROSE + RULE: three prose lines explaining why the rule exists, followed by the rule itself, followed by three small reader cards for YOU, COLLABORATORS, and CODEX. Right panel — BARE — RULE ALONE: the same rule with no prose, followed by a single reader card for CODEX. The literate panel is highlighted in red and footnoted "The why is the entry's load-bearing part"; the bare panel notes that future-you forgets the reason.](images/07-agents-md-fig-02.png)

*Figure 7.2 — Knuth's literate programming applied to AGENTS.md. Writing for three readers at once is what makes the artifact more than a workaround for statelessness.*

---

## What would change my mind

The chapter's central operational claim is that AGENTS.md materially improves Codex output quality when maintained properly. If a controlled comparison — same project, same prompts, with and without AGENTS.md — showed no measurable difference in output quality or session efficiency, the artifact becomes useful for documentation purposes but the overhead of maintaining it weakens. The chapter would still recommend it for the clarity-exercise reason; the urgency of maintaining the lessons-learned section drops.

OpenAI's own documentation, the practitioner literature, and the evidence from Seth's and Nicholas's builds all converge on the claim that AGENTS.md helps materially. A clean negative result would be surprising. It is not impossible.

---

## Still puzzling

When AGENTS.md hurts more than it helps. Anecdotally, bloated files cause Codex to ignore instructions. The threshold is fuzzy. Two hundred lines is the working rule of thumb; the actual cliff is undocumented.

`AGENTS.override.md` and managed-policy locations. Some enterprise Codex deployments enforce district-level AGENTS.md from a managed-policy location, with individual `AGENTS.override.md` files taking precedence. This is out of scope for the primary student reader.

AGENTS.md versus Skills. Codex Skills (2025+ feature) are reusable workflow templates, adjacent to AGENTS.md but not the same thing. The book uses AGENTS.md as the spine. Skills appear in the Claude Code sister books under a different name. Whether Skills become central to student workflows in a near-term edition is open.

---

## AI Wayback Machine

🕰️ **Donald Knuth** (born 1938) — computer scientist who created TeX and articulated the principle of **literate programming**: that a program should be written for humans first, with the code as a side effect, so that the human reader and the machine reader are served by the same artifact.[^1] In Knuth's 1984 essay: *"Instead of imagining that our main task is to instruct a computer what to do, let us concentrate rather on explaining to human beings what we want a computer to do."* AGENTS.md is literate programming applied to AI collaboration. The artifact you write — the five-element format, the lessons-learned entries, the never-rules — is read by you, by your future collaborators, and by Codex, all from the same file. Writing for all three readers at once is what makes the discipline worth the overhead. Knuth's argument was that the human-readable explanation is the primary artifact and the executable code is the by-product. The form scales: the project context you write for Codex is the primary artifact, and Codex's improved output is the by-product.

---

## Bridge

You have AGENTS.md. Codex knows your project at every session. Chapter 8 teaches the discipline upstream of the first prompt: problem formulation.

---

## Exercises

**Warm-up**

1. *(Targets: five-element format)* Before writing anything: open your current project directory (or the most recent one you have worked in). List what you would put in each of the five AGENTS.md categories right now — stack, style deviations, architectural decisions, environment quirks, lessons learned. One line per item is enough. Notice which category is hardest to fill. That is the category most likely to bite you.

2. *(Targets: include/exclude discipline)* Write five candidate AGENTS.md entries for a project you are working on. Then apply the discipline: *does Codex need to know this every session in this project?* Classify each entry as AGENTS.md, session prompt, or omit. Keep the classification; you will use it in Exercise 3.

**Application**

3. *(Targets: /init + refinement pass)* Run `/init` on a project that does not already have an AGENTS.md. Read what Codex proposed. Apply the refinement pass: delete what Codex already knows from the code, add the architectural decisions not visible in the code, add environment quirks, start the lessons-learned section empty. Commit the result to git. Time the whole process.

4. *(Targets: AGENTS.md vs. task queue vs. build log)* Take three things you were planning to tell Codex at the start of your next session. Classify each one: persistent project knowledge (AGENTS.md), session-specific work (task queue), or judgment-layer note (build log). Write one sentence explaining the classification for each.

**Synthesis**

5. *(Targets: Knuth's three-audience principle)* Take the AGENTS.md you produced in Exercise 3. Read it as if you are a new collaborator joining the project who has never seen the codebase. What does the file explain? What does it assume? What would you need to know that is not there? Rewrite one entry so that it serves all three readers — you, a future collaborator, and Codex — rather than Codex alone.

**Challenge**

6. *(Targets: maintenance discipline + lessons-learned value)* Run one full Codex session on your project with AGENTS.md in place. At the end, add at least one lessons-learned entry — either a mistake Codex made that the plan-review caught, or a default Codex assumed that you had to override. Then: run the same task again without the AGENTS.md present (rename it temporarily). Document what changed. This is your controlled comparison.

---

[^1]: Knuth, D. E. "Literate Programming." *The Computer Journal* 27, no. 2 (1984): 97–111.

---
