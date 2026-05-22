# Chapter 11 — Brutalist: When the Build Is Creative

*The output is polished. The voice is the model's. Those are not the same problem.*

---

Nicholas could tell.

He would read a classmate's essay, look at a classmate's slide deck, review a classmate's project page, and feel something missing underneath the surface. The work was technically correct. The output was polished. There were no obvious errors. And yet it read like no one in particular had made it — like the most-probable version of what that assignment would look like, averaged across everyone who had ever submitted something similar.

He recognized it because he had been making things for years — drawings, small game prototypes, visual design experiments — and he knew what it felt like when something had a specific person's choices in it versus when it had been assembled from the most available parts. The classmates' work had been assembled. It was getting them grades. It had no voice.

Nicholas did not have vocabulary for this yet. He does now. The classmates had fallen into the **creative version of the fluency trap**. Their prompts had been clear. Codex had built exactly what the prompts described. The prompts had not specified voice, aesthetic stance, what choices to make about format and tone — those had been left open, and Codex had filled them with the most-probable interpretation, which is the interpretation no specific person would have made on purpose.

The technical implementation was fine. What was missing was the author.

---

## What the creative fluency trap is

In Chapter 2, the fluency trap was about learning: reading Codex's output felt like understanding, even when the cognitive events that produce understanding had not occurred. The mechanism was borrowing capability instead of building it.

The creative fluency trap is the same mechanism in a different domain. Using Codex's output feels like making something, even when the aesthetic decisions that constitute making something have not occurred. The mechanism is delegating judgment instead of exercising it.

The classmates' work was not plagiarized. They had written the prompts. They had reviewed the output. They had submitted the result. What they had not done was decide, for themselves, what the thing should look like, feel like, what choices it should make, what it should refuse. Those decisions had defaulted to Codex's most-probable choices — which means they had defaulted to no one's choices, because the most-probable choice is an average over everyone who ever made something similar.

Voice is what remains when you subtract the average. The classmates' work had subtracted the average and left nothing.

<!-- → [IMAGE: two-panel illustration — left panel: a polished portfolio page with generic sans-serif font, blue accent color, standard card grid, labeled "most-probable output"; right panel: same content with specific serif face, muted earth palette, annotation-style labels, labeled "specified output." Caption: "Both are technically correct. Only one has an author."] -->

---

## The three files

The discipline that prevents this is a specific separation of concerns, encoded in three files. Each file holds a different kind of decision. Together they make the project specific enough that Codex cannot default to the average — because the average is already excluded by the files.

### AGENTS.md — Technical Constitution

You have an AGENTS.md from Chapter 6. For a creative project, it extends naturally: the stack, the file structure, the naming conventions, the never-touch list, the verification step. Technical decisions you have made once and do not want Codex to revisit.

This is the same AGENTS.md discipline applied to a new domain. The creative project's AGENTS.md is not different in kind from a data-pipeline project's AGENTS.md. It specifies the technical environment so Codex is not inventing it.

### DESIGN.md — Visual Constitution

The DESIGN.md is the file that prevents Codex from improvising aesthetics.

The classmates' work was voiceless partly because there was no DESIGN.md. Codex sampled the most-probable visual and stylistic choices from its training data: the default color palette for survey charts, the default font pairing for portfolios, the default hover behavior for cards. None of those choices were wrong. None of them were anyone's.

A DESIGN.md prevents this by being *brutally specific*. Not "use a professional color palette" — that is an invitation to sample the average. Six named color variables, with hex values. Two typefaces, named, with sizes. An explicit interaction vocabulary listing what the system does *and* what it explicitly never does.

For a personal portfolio:

```markdown
# DESIGN.md — Personal Portfolio

## Color system (six variables, no others)
--background:  #f8f5f0   /* warm white */
--ink:         #1f1f1f   /* near-black, body */
--accent:      #b54232   /* deep red, links and emphasis */
--muted:       #6b6b6b   /* gray, secondary text */
--code-bg:     #1f1f1f   /* dark background for code blocks */
--code-fg:     #f8f5f0   /* light foreground for code blocks */

## Typography (two faces, no others)
Display + body: Iowan Old Style, Charter, serif. 18px body.
Code:           SF Mono, Menlo, monospace. 15px.

No sans-serif. No script. No display face other than the serif above.

## Interaction vocabulary
ALLOWED:
- Hover on links: underline darkens; no other change.
- Click on project card: navigate to project page.
- Keyboard navigation: standard tab order; visible focus ring.

NEVER:
- Animations or transitions over 200ms.
- Hover effects that change layout.
- Hidden content that requires hover to reveal.
- Auto-playing media of any kind.

## Accessibility
- All text at least 16px.
- Color contrast meets WCAG AA at minimum.
- All interactive elements have keyboard-accessible focus.
- Images have alt text or are explicitly marked decorative.
```

Forty lines. Six colors. Two typefaces. Three allowed interactions. Four forbidden interactions.

The constraint is the point. A DESIGN.md that tries to specify everything Codex *might* do produces a file too long to follow. A DESIGN.md that specifies the small number of decisions you actually care about — and explicitly forbids the most-probable improvisations — gets followed. The forbidden list is as important as the allowed list. You are not only saying what the project looks like. You are saying what the project refuses to look like.

### PROJECT.md — Project State (with Intent Layer human, always)

PROJECT.md has two layers, and the distinction between them is the heart of the discipline.

**Layer 1: Intent (Human Layer — never overwritten by Codex).** What the project is. What you want the visitor to feel or understand. What questions it answers. What it refuses to answer. The tone.

**Layer 2: Technical State (Codex layer).** What is built. What is pending. The generation log. Open technical questions.

Layer 1 is what you write and Codex reads but does not modify. If Codex proposes a change to Layer 1, you reject it; the proposal goes to Layer 2 as a question for you to decide. The Intent Layer is the author's domain. The Technical State is the executor's working surface.

For the portfolio:

```markdown
# PROJECT.md — Personal Portfolio

## Layer 1: Intent (Human Layer — never overwritten by Codex)

What this project is:
A personal portfolio that shows the work I have done and signals — through
its design choices — that I think about what I make. It is not a resume.
It is not a job application. It is the artifact a curious person reads when
they want to know what kind of person I am as a builder.

What the visitor should understand after using it:
- What kinds of problems I am drawn to (specific examples, not categories).
- That I have an aesthetic — that the visual choices were deliberate.
- That I write about what I make, not just produce artifacts.

What questions it answers:
- What has this person built? (Project cards with brief descriptions.)
- What is this person interested in? (The selection itself is the answer.)
- How does this person write? (One essay per project, linked from the card.)

What questions it refuses to answer:
- "Are they available for hire?" (Not on this site; reach out by email.)
- "How many GitHub stars?" (Vanity metrics are not the point.)
- "What is their resume?" (Different artifact.)

The tone:
Matter-of-fact. No marketing voice. No "passionate about..." or
"results-driven..." Direct second-person where the visitor is addressed.
Specifics over generic claims.

## Layer 2: Technical State (Codex Layer)

What is built:
- HTML scaffold with project cards.
- CSS with the six variables defined.

What is pending:
- The essays linked from each project card.
- The contact section.
- The dark-mode toggle.

Generation log: [will populate during build]

Open technical questions:
- Markdown rendered to HTML at build time, or written as HTML directly?
```

The Intent Layer is what makes the project yours. It is 200 words of specificity that excludes the entire space of most-probable defaults. Codex, reading it, cannot produce the voiceless output from the chapter opening — the output would directly contradict the specified intent.

The phase gate: **Code Mode does not begin until both layers of PROJECT.md are populated.** Not as a suggestion. As a gate. The classmates' work was voiceless because there was no Intent Layer to start from. Codex filled in defaults for everything the Intent Layer would have specified.

<!-- → [DIAGRAM: The three-file system as three nested layers. Outer: CLAUDE.md / AGENTS.md (technical constitution). Middle: DESIGN.md (visual constitution). Inner: PROJECT.md (project state — Intent Layer is human, always). Editorial style.] -->

<!-- → [TABLE: three-file responsibility matrix — three rows (AGENTS.md, DESIGN.md, PROJECT.md), four columns: file name, what it holds, who writes it, what happens if it's missing. Student should be able to explain the purpose of each file and the failure mode its absence produces.] -->

---

## Why the refusals matter

There is a feature worth naming explicitly. The system says no.

When the DESIGN.md specifies six colors and no others, Codex does not propose a seventh. When the Intent Layer says the tone is matter-of-fact, Codex does not produce marketing-voice copy. When the interaction vocabulary forbids hover-only states, Codex does not generate them.

The refusals are the feature. Not constraints reluctantly accepted, but the mechanism by which voice is preserved. Every refusal is a place where the most-probable choice would have appeared and was excluded. The project's aesthetic is defined as much by what it refuses to do as by what it does.

The governing principle: **maximally informed, minimally autonomous, by design.** The three files make Codex maximally informed — it has the technical environment, the visual decisions, the intent, the scope. The explicit exclusions make it minimally autonomous — there is no open question for it to answer by sampling the average. The combination is what protects authorship.

Codex's defaults are not wrong. They are the average of what has worked. But the average is the opposite of voice. The discipline is how you exclude the average.

---

## The same chart, two builds

The task: a data visualization showing student survey responses by class period.

**Without the three-file system.**

You ask Codex to build a chart from the CSV. Codex generates the chart. It uses a green-yellow-red gradient — the most-probable choice for survey data, common in every charting library's default theme. The axis labels are formatted in the default style. The chart is technically correct.

It is also the chart your classmate would produce with the same data and a similar prompt. The voice of the analysis — which would be expressed partly through the chart's specific aesthetic choices — is absent. The chart communicates the data. It does not communicate *your* framing of the data.

**With the three-file system.**

You write a DESIGN.md for the visualization: a muted earth-tone palette, no gridlines (the data carries the structure), category labels as annotations rather than separate axes. You write a PROJECT.md Intent Layer: *"this chart is for the class to see what their classmates think — the design should feel collaborative, not authoritative. No evaluation language. No green/red. The reader is a peer, not a subject."*

You ask Codex to generate the chart given the three files in context. The output uses the muted palette. Annotations replace gridlines. The chart looks like your chart. A peer who saw it without attribution would probably recognize it as yours.

Same Codex. Same data. Same 45-minute build. The 30 minutes of three-file work are the difference between a technically correct chart and a chart with a specific person's thinking in it.

<!-- → [IMAGE: side-by-side chart comparison — left: survey data rendered with default green-yellow-red gradient, standard gridlines, axis labels; right: same data rendered with muted earth-tone palette, annotation labels, no gridlines. Caption: "Same data, same Codex, different files. The 30-minute investment is visible in the output."] -->

---

## The 45-minute scope test

A forcing function that keeps creative builds appropriately sized: **if writing all three files takes more than 45 minutes, the project is too large for a first creative build.**

The portfolio in the worked example passes the test — the three files can be written in 30–40 minutes if the intent is clear. A more ambitious project (a multi-page interactive essay, a small game, a custom data visualization with non-standard interactions) takes longer to specify and benefits from being scoped down for the first build.

The test is not about whether you *can* write longer files. It is about whether the first creative build should be ambitious enough to *need* longer files. The answer is almost always no. Make it small. Make it complete. Ship it. Then build the next one larger. The discipline of completing a small, specified project builds the skill the larger projects will need.

---

## What the three-file system is not

**It is not for designers.** Anyone building anything with visual output is making design decisions. The choice is whether you make them explicitly — in the DESIGN.md, with your specific preferences stated — or whether you make them by omission and let Codex sample the average. The chapter argues for explicit.

**It is not overhead for small projects.** The 45-minute scope test forces appropriate sizing. For a small project, the three files will be short and will take 20 minutes to write. Write them anyway. They protect against exactly the failure mode that opened the chapter.

**It is not about distrust of Codex.** Codex's defaults are reasonable. They are the average of what has worked across its training data. The three-file system is not a claim that Codex's defaults are bad. It is a claim that your specific choices are not the average, and the files are how you specify the difference.

**It is not a substitute for having aesthetic opinions.** The DESIGN.md needs to specify six colors. If you do not have opinions about which six colors, the DESIGN.md will be vague, and Codex will sample the average anyway. The discipline works only if you have made the decisions the files are asking you to make. Making those decisions is part of the work.

---

## A note on attribution

The Brutalist three-file system — AGENTS.md plus DESIGN.md plus PROJECT.md, with the specific size and discipline conventions described in this chapter — is documented at brutalist.art. **brutalist.art is the author's own design system.** That is not a third-party reference; it is the same person who wrote this book recommending their own framework. You should know who is recommending what.

The system exists within a broader 2025–2026 ecosystem of AI-readable project-specification files: designmd.app's library of design systems for AI agents, VoltAgent's *awesome-design-md* collection, the SKILL.md / CLAUDE.md / AGENTS.md genre. The Brutalist three-file system is one entrant in that conversation.[^1]

If you prefer a different approach from the broader ecosystem, the principles of this chapter still apply. The names of the files matter less than the concerns the files separate: technical decisions you do not want Codex to revisit; visual and interaction decisions Codex must not improvise; intent that is irreducibly yours.

---

## What would change my mind

The chapter's strong claim: three concerns separated into three files produces materially better creative builds — specifically, builds with more recognizable voice and more specific aesthetic choices — than a single-file or no-file approach.

What would soften that claim: a controlled comparison — same project brief, with and without the three-file system — that produced equivalent voice fidelity and aesthetic specificity. If the three files do not measurably distinguish the output from what Codex would produce on a clear single prompt, the system becomes optional rather than load-bearing.

A two-file system — combining AGENTS.md and DESIGN.md — might also work. The book commits to three because the technical and aesthetic concerns are genuinely separable in practice and produce cleaner outputs when kept separate. Whether two are clearly separable enough is an open practitioner question.

---

## What is still puzzling

**The 200-line size limit per file.** AGENTS.md has documented Codex behavior degrading past roughly 200 lines. DESIGN.md and PROJECT.md have not been similarly characterized. The book extends the limit by analogy; whether the degradation threshold holds for all three files is empirically untested.

**Whether the system generalizes beyond visual design.** The Brutalist three-file system is framed for visual creative work — portfolios, charts, interactive essays, small games. The principles — separate concerns, make decisions explicit, refuse to improvise — apply to writing, music, slide decks, anything with aesthetic dimensions. But the specific file structure may differ across mediums. A DESIGN.md for a prose essay is not the same artifact as a DESIGN.md for a website. The generalization is plausible; it is not yet documented.

**The Intent Layer as a skill.** Writing a specific, exclusionary Intent Layer is not easy the first time. Most first attempts are either too vague ("I want it to feel professional") or too prescriptive in the wrong direction (specifying visual details that belong in DESIGN.md). The skill of writing a genuinely useful Intent Layer is built through iteration. The book does not fully account for how much iteration a new practitioner will need before the Intent Layer does its job.

---

## AI Wayback Machine

🕰️ **Sol LeWitt** (1928–2007) — American conceptual artist whose *Paragraphs on Conceptual Art* (1967) argued that the idea is the art — that the person who holds the intent and writes the instruction is the author, regardless of who executes.[^2] LeWitt's wall drawings were instructions: a set of constraints and operations that any competent person could execute. The execution varied in small ways; the work was the same, because the concept was the work. The instructions were specific enough that the executor's voice did not drown the artist's; LeWitt's choices were in the constraints.

The three-file system is LeWitt's discipline applied to AI-assisted creative work. The Intent Layer holds your intent. The DESIGN.md holds your constraints. The AGENTS.md holds the technical scaffolding. Together they specify the work completely enough that Codex's execution does not overwrite your authorship. The CLI is the executor; you are the author. The discipline LeWitt operationalized in 1967, for human executors, scales to AI executors precisely because the relationship is the same. The author specifies the work; the executor produces the instance. What LeWitt understood was that the specification *is* the creative act. Execution is how it becomes visible.

The classmates who produced voiceless work had not understood this. They had outsourced the specification along with the execution. The three-file system is what it looks like to keep the specification yours.

---

## Bridge

You have the full conducting discipline — technical and creative. The next chapter is the planning phase of your first complete build: bringing the AGENTS.md, DESIGN.md, PROJECT.md, the gate, and the handoff conditions together into a single coherent session.

---

## Exercises

**Warm-up**

1. *(Tests: what the creative fluency trap is)* Explain in your own words why the classmates' work at the chapter opening was voiceless — not in terms of what they did wrong, but in terms of what cognitive event did not occur. Connect your answer to the mechanism described in Chapter 2.

2. *(Tests: file responsibilities)* For each of the following decisions, name which file it belongs in and explain why: (a) the project uses vanilla HTML/CSS with no build step; (b) links use a deep red accent on hover; (c) the portfolio is for a curious peer, not a recruiter; (d) the `dist/` folder should never be modified by Codex; (e) no animations longer than 200ms.

3. *(Tests: the forbidden list)* A DESIGN.md includes an ALLOWED section but no NEVER section. What is the likely consequence for the build? What specific kind of output would the missing NEVER section have prevented?

**Application**

4. *(Tests: writing a DESIGN.md)* Write a DESIGN.md for one of the following: (a) a personal essay site with a single long-form piece; (b) a data visualization of your school's club enrollment by year; (c) a small browser game with a minimal UI. Your DESIGN.md must specify exactly six color variables, two typefaces, at least three ALLOWED interactions, and at least three NEVER interactions. No vague terms — every entry must be specific enough that Codex could enforce it without asking you a follow-up question.

5. *(Tests: writing an Intent Layer)* Write the Intent Layer of a PROJECT.md for a creative project of your choice. It must answer all four questions: what the project is, what the visitor should understand, what questions it answers, and what questions it refuses to answer. Have a classmate read it without seeing the project itself. Can they describe what the project does and does not do? Revise until they can.

6. *(Tests: identifying a creative fluency failure)* You are given a transcript of a creative build session in which Codex made three aesthetic decisions without being asked to. For each decision: identify which file would have prevented it, and write the entry that should have been in that file.

**Synthesis**

7. *(Tests: maximally informed, minimally autonomous)* The chapter's governing principle is "maximally informed, minimally autonomous, by design." Explain what each term means operationally — not as an abstract principle, but in terms of specific entries in the three files. Then identify one place in each file where a vague or absent entry would shift Codex from minimally autonomous toward more autonomous, and explain the consequence.

8. *(Tests: three-file system vs. no-file system)* A classmate argues: "I wrote a very detailed single prompt — three paragraphs describing the design, the tone, and the technical setup. That's equivalent to three files." Evaluate this claim. What does the single-prompt approach preserve from the three-file discipline? What does it lose? Be specific about which failure modes the three-file system catches that the detailed prompt does not.

**Challenge**

9. *(Open-ended)* The chapter frames the three-file system for visual creative work — portfolios, charts, interactive essays. Adapt the system for a non-visual creative project: a long-form written piece, a piece of music generated with AI assistance, or an AI-assisted slide deck. Identify which concerns from the original three files still apply, which need to be renamed or restructured, and what new concern — if any — the non-visual medium introduces that the original system does not address.

---

[^1]: For the broader ecosystem, see designmd.app's library of design systems for AI agents; VoltAgent's awesome-design-md collection on GitHub; the DEV Community discussion "AGENTS.md, SKILL.md, DESIGN.md: How AI Instructions Split into Three Layers." The Brutalist three-file system is one entrant in this conversation.
[^2]: LeWitt, S. "Paragraphs on Conceptual Art." *Artforum* 5, no. 10 (1967): 79–83. See also LeWitt's "Sentences on Conceptual Art" in *0–9*, no. 5 (1969).
