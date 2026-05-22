# Chapter 10 — Brutalist: When the Build Is Creative

> The technical barrier is now low enough that any student can produce ambitious creative work. The question is whether the creative judgment stays theirs.

---

## Learning outcomes

1. **(Understand)** Explain how the fluency trap manifests in creative AI use.
2. **(Apply)** Apply the Brutalist three-file system (AGENTS.md, DESIGN.md, PROJECT.md) to a creative project.
3. **(Analyze)** Distinguish creative judgment (irreducibly human) from technical execution (Codex's domain) in a provided creative build.

---

## Opening

Nicholas — a friend of Seth's, mentioned earlier in the book — has watched his classmates use AI in a specific way that bothered him before he had vocabulary for it.

The classmates would use Codex (and other AI tools) to generate the prose for an essay, the graphics for a slide deck, the structure of a presentation, the framing of an analysis. The output was polished. The output was technically correct. The output had no *voice*. No quirky design choice. No idiosyncratic comment. No sign that a specific person had thought about what to write. The output was the *average* AP CS student's submission, as the model had inferred from training — indistinguishable from the next classmate's submission that came from the same model with a similar prompt.

Nicholas would read the work and feel a void underneath it. The classmates were not necessarily aware of the void. The work was getting them grades. But Nicholas, who had been making things — drawings, small game prototypes, his own visual design experiments — for years, recognized something missing. The work was not theirs.

This is the **creative version** of the fluency trap from Chapter 1. The technical implementation is right. The output is polished. The *meaning* of the output — its voice, its stance, what choices about format and tone it makes — has drifted to the model's most-probable choice, which is the choice no specific person would have made on purpose.

This chapter is the discipline that keeps creative judgment yours in AI-assisted creative builds.

<!-- → [DIAGRAM: The three-file system as three nested layers. Outer: CLAUDE.md / AGENTS.md (technical constitution). Middle: DESIGN.md (visual constitution). Inner: PROJECT.md (project state — Intent Layer is human, always). Editorial style.] -->

---

## A note on attribution

This chapter teaches the **Brutalist three-file system** — AGENTS.md plus DESIGN.md plus PROJECT.md, with specific size and discipline conventions. The system is one operational choice within a broader 2025–2026 ecosystem of AI-readable project-specification files. Other entries: designmd.app's library of design systems for AI agents, VoltAgent's *awesome-design-md* collection, the SKILL.md / CLAUDE.md / AGENTS.md genre.[^1]

The Brutalist three-file system is documented at brutalist.art. **brutalist.art is the author's own design system.** That is not a third-party reference; it is the same author who wrote this book teaching their own framework. The book recommends the system because it has held up in practice across multiple projects in this series; you should know who is recommending what.

If you prefer a different approach from the broader ecosystem, the *principles* of this chapter still apply. The names of the files matter less than *the concerns the files separate*: technical decisions you do not want Codex to revisit; visual and interaction decisions Codex must not improvise; intent that is irreducibly yours.

---

## The three files

### AGENTS.md — Technical Constitution

You have an AGENTS.md from Chapter 6. For a creative project, the AGENTS.md is the same kind of artifact, extended for creative-specific technical decisions:

- The stack (vanilla HTML/CSS/JS; React; whatever the project uses).
- The file structure.
- The naming conventions.
- The never-touch list (other projects' files; shared CSS that should not be modified).
- The verification step (how you know a step is done from the technical side).

The AGENTS.md for the creative project is project-scoped — it lives at the project root and applies whenever Codex is working there.

### DESIGN.md — Visual Constitution

The DESIGN.md is the file that prevents Codex from improvising aesthetics. The opening of the chapter — where classmates produced polished but voiceless work — happened because there was no DESIGN.md. The model sampled the most-probable visual and stylistic choices from its training data.

A useful DESIGN.md is *brutally specific*. Six color variables, named, with hex values. No others. The typography is two faces — say, one monospace, one sans — with sizes specified. The interaction vocabulary lists what the system *does* and explicitly names what the system *does not do*.

For a personal portfolio website:

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

Forty lines. Six colors. Two typefaces. Three allowed interactions. Four forbidden interactions. The file fits on a single screen. **The constraint is the point.** A DESIGN.md that tries to specify everything Codex *might* do produces a file Codex ignores. A DESIGN.md that specifies the small number of decisions you actually care about — *and explicitly forbids the most-probable improvisations* — gets followed.

### PROJECT.md — Project State (with Intent Layer human, always)

PROJECT.md has two layers.

**Layer 1: Intent (Human Layer — never overwritten by Codex).** What the project is. What you want the visitor to feel or understand. What questions it answers. What it refuses to answer. The tone.

**Layer 2: Technical State (Codex layer).** What is built. What is pending. The generation log. Open technical questions.

Layer 1 is the irreducibly human part. You write it. Codex reads it but does not modify it. If Codex proposes changes to Layer 1, you reject; the proposal goes to Layer 2 as a question for you to decide.

For the personal portfolio:

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
- "Are they available for hire?" (Not on this site; reach out by email if interested.)
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

Generation log:
[will populate during build]

Open technical questions:
- Should the essays be markdown rendered to HTML at build time, or written
  as HTML directly?
```

The Intent Layer is what makes the project *yours*. The Technical State is the working surface Codex updates as the build proceeds.

The phase gate: **Code Mode does not begin until both layers of PROJECT.md are populated.** This is not a suggestion. The chapter opening — the classmates whose work was polished and voiceless — happened because there was no PROJECT.md to start from. Codex filled in defaults for everything the Intent Layer would have specified.

---

## The 45-minute scope test

A useful forcing function: **if writing all three files takes more than 45 minutes, the project is too large for a first creative build.**

The portfolio in the worked example passes the test — the three files can be written in 30–40 minutes if the Intent is clear. A more ambitious creative project (a small game, a multi-page interactive essay, a data visualization with custom interactions) takes longer to specify and benefits from being scoped down on the first build.

The test is not about whether you *can* write a longer set of files. It is about whether the *first* creative build should be ambitious enough to need a longer set. For a first build, the answer is no. Make it small. Make it complete. Ship it. Then build the next one larger.

---

## The Brutalist refusal

The system has a feature worth naming explicitly: **it says no when Codex is asked to make a creative judgment.**

When the DESIGN.md specifies six colors and no others, Codex will not propose a seventh. When the Intent Layer says the tone is matter-of-fact, Codex will not produce marketing-voice copy. When the interaction vocabulary forbids hover-only states, Codex will not generate them.

The refusal is the feature. The system is *maximally informed* (the three files give Codex enough context to be useful) and *minimally autonomous* (the three files prevent Codex from making the creative decisions that should be yours). The combination is what protects voice.

The Brutalist governing principle: **maximally informed, minimally autonomous, by design.** The three files operationalize the principle. The system refusals are the principle in action.

---

## Worked example: same chart, two builds

The task: a data visualization showing student survey responses by class period.

**Path A: without the three-file system.**

You ask Codex to "make a chart from this CSV showing responses by class period." Codex generates D3 or Chart.js code. The chart renders. It uses default colors (a green-yellow-red gradient that is the most-probable choice for survey data). The axis labels are formatted in the model's default style. The chart is technically correct.

It is also a chart that anyone with the same data and the same prompt would produce. Your chart and your classmate's chart are visually indistinguishable. The voice of the analysis (which would be expressed *partly* through the chart's specific aesthetic choices) is absent.

**Path B: with the three-file system.**

You write a DESIGN.md for the visualization: muted earth-tone palette, sans-serif throughout, no gridlines (the data carries the structure), category labels as the chart's annotations rather than as separate axes. You write a PROJECT.md Intent Layer: "this chart is for the class to see *what their classmates think*, not for me to judge the responses — the design should feel collaborative, not authoritative."

You ask Codex to generate the chart given the three files in context. The output uses the muted palette. The annotations replace gridlines. The chart looks like *your* chart. A peer who saw it could probably guess it was yours; the design choices express your specific framing.

Same Codex. Same data. The 30 minutes of three-file work are the difference between Path A and Path B.

---

## Common misconceptions

**"Three files is too much for a small creative project."** The 45-minute scope test forces appropriate sizing. If three files genuinely feel like overhead, the project is small enough that the constraint will land lightly. Write them anyway; they protect against the failure mode that opened the chapter.

**"DESIGN.md is for designers."** No. Anyone building anything with visual output makes design decisions. The choice is whether you make them *explicitly* or whether Codex makes them by sampling from the most-probable patterns in its training data. The chapter argues for explicit.

**"Codex can figure out the Intent Layer from context."** No. The Intent Layer is what makes the project *yours*. Codex's "context" is its training distribution; the Intent Layer is what you know about your audience and your project that is not in that distribution.

**"I'll iterate on the files as I build."** No. Iteration during generation is forward correction (Chapter 9 warned against this). Write the files first; refine them only if the build surfaces a genuinely new question. *Most* "I'll refine later" turns into *never refine* turns into *the project drifted*.

**"The Intent Layer can be vague."** No. Vague intent produces vague output. The Intent Layer's specificity is what makes the project teach what you wanted it to teach.

---

## Exercises

1. **(Apply)** Create a DESIGN.md for a creative project (a portfolio page, a data visualization, a small game, a personal-essay site). Name six design decisions that are fully specified and two that are escalated to you (places where you will make the decision rather than specifying in advance).

2. **(Analyze)** A provided creative build transcript shows Codex making an aesthetic judgment. Identify the moment, name which file would have prevented it, and write the entry that should be added.

3. **(Create)** Write the Intent Layer of a PROJECT.md for a creative project. Have a classmate read it. Can they tell what you are trying to make? Revise until they can.

---

## Links

- Brutalist Design System: [brutalist.art](https://brutalist.art) (author's own)
- DESIGN.md ecosystem: [designmd.app](https://designmd.app); [getdesign.md](https://getdesign.md); [github.com/VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md)

---

## What would change my mind

The chapter's strong claim is that **three concerns separated into three files produces materially better creative builds** than a single-file or no-file approach. If a controlled comparison — same project brief, with and without the three-file system — produced equivalent voice fidelity and aesthetic specificity, the chapter's prescription becomes optional rather than load-bearing.

A two-file system (combining AGENTS.md and DESIGN.md) might also work. The book commits to three because the three concerns are clearly separable in practice. Whether two are clearly separable enough is an open practitioner question.

---

## Still puzzling

- **The 200-line size limit per file.** AGENTS.md has documented Codex behavior degrading past ~200 lines. DESIGN.md and PROJECT.md have not been similarly characterized. The book extends the limit by analogy; whether it holds is empirically untested.

- **brutalist.art as authority.** The chapter cites the author's own design system. Reader trust depends on transparency: the system has held up across multiple projects in this series, but it is not third-party-validated. Other approaches in the broader ecosystem are also defensible.

- **Whether the chapter generalizes beyond visual design.** The Brutalist three-file system is framed for visual creative work. The principles — separate concerns, make decisions explicit, refuse to improvise — apply to writing, music, slide decks, anything with aesthetic dimensions. The book's working answer is that the principles generalize but the specific file structure may differ across mediums.

---

## AI Wayback Machine

🕰️ **Sol LeWitt** (1928–2007) — American conceptual artist whose *Paragraphs on Conceptual Art* (1967) argued that **the idea is the art** — that the person who holds the intent and writes the instruction is the author, regardless of who executes.[^2] LeWitt's wall drawings were instructions: a set of constraints and operations that anyone competent could execute. The execution varied; the work was the same, because the *concept* was the work. The instructions were specific enough that the executor's voice did not drown the artist's; LeWitt's voice was in the constraints.

The three-file system is LeWitt's discipline applied to AI-assisted creative work. The Intent Layer holds your intent. The DESIGN.md holds your constraints. The AGENTS.md holds the technical scaffolding. Together they specify the work completely enough that Codex's execution does not drown your authorship. The CLI is the executor; you are the author. The discipline LeWitt operationalized in 1967, for human executors, scales to AI executors precisely because the relationship is the same: the author specifies the work; the executor produces the instance.

---

## Bridge

You have the full conducting discipline — technical and creative. Chapter 11 is the planning phase of your first complete build.

---

[^1]: For the broader ecosystem, see designmd.app's library of design systems for AI agents; VoltAgent's awesome-design-md collection on GitHub; the DEV Community discussion "AGENTS.md, SKILL.md, DESIGN.md: How AI Instructions Split into Three Layers." The Brutalist three-file system is one entrant in this conversation.
[^2]: LeWitt, S. "Paragraphs on Conceptual Art." *Artforum* 5, no. 10 (1967): 79–83. See also LeWitt's "Sentences on Conceptual Art" in *0–9*, no. 5 (1969).
