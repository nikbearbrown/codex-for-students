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

![Both are technically correct. Only one has an author.](images/11-brutalist-creative-builds-fig-01.png)
*Figure 11.1 — Illustration *

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

For Zebonastic — Seth's dark-neon Next.js platform where developers and educators create game templates, with a weekly article on horror-game psychology and adrenaline mechanics:

```markdown
# DESIGN.md — Zebonastic

## Color system (six variables, no others)
--background:  #0a0a12   /* near-black, deep blue-black */
--ink:         #e8e8f0   /* off-white, body */
--accent:      #ff2e88   /* hot neon pink, links and emphasis */
--muted:       #6b6b85   /* dim violet-gray, secondary text */
--code-bg:     #11111c   /* slightly lighter than background */
--code-fg:     #b5ffd5   /* phosphor green, code */

## Typography (two faces, no others)
Display + body: Inter, system-ui, sans-serif. 17px body.
Code:           JetBrains Mono, monospace. 14px.

No serif. No script. No display face other than Inter.

## Interaction vocabulary
ALLOWED:
- Hover on links: the underline glows from --muted to --accent over 120ms.
- Click on template card: navigate to template page.
- Keyboard navigation: standard tab order; visible focus ring in --accent.

NEVER:
- Animations or transitions over 200ms.
- Hover effects that change layout.
- Hidden content that requires hover to reveal.
- Auto-playing media of any kind.

## Accessibility
- All text at least 16px.
- Color contrast meets WCAG AA at minimum on the dark background.
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

For Zebonastic:

```markdown
# PROJECT.md — Zebonastic

## Layer 1: Intent (Human Layer — never overwritten by Codex)

What this project is:
A dark-neon platform where developers and educators publish game templates
and short articles on horror-game psychology and adrenaline mechanics.
It is not a portfolio. It is not a marketplace. It is the place where the
craft of building scary games gets taken apart in public, weekly.

What the visitor should understand after using it:
- That horror in games is a designed effect, not a vibe — and there is
  a literature for it.
- That the templates are working starting points, not finished games.
- That the writing is for builders, not consumers — second-person,
  specifics over genre talk.

What questions it answers:
- What is a "scare event" and how do you build one? (Articles.)
- How do other people structure their horror prototypes? (Templates.)
- What is this week's piece? (Front page, dated, one-click.)

What questions it refuses to answer:
- "Top 10 horror games of all time?" (Not a listicle site.)
- "Buy my course?" (Nothing is monetized on Zebonastic.)
- "Will this article scare me?" (Wrong reader.)

The tone:
Matter-of-fact. No marketing voice. No "passionate about..." or
"unleash your creativity." Direct second-person where the builder is
addressed. Specifics over generic claims.

## Layer 2: Technical State (Codex Layer)

What is built:
- Next.js scaffold with the article and template routes.
- Tailwind config with the six variables defined.

What is pending:
- The MDX pipeline for weekly articles.
- The template-card index.
- The dark-mode-only enforcement (no light-mode toggle).

Generation log: [will populate during build]

Open technical questions:
- MDX rendered at build time with `next-mdx-remote`, or as static MDX routes?
```

The Intent Layer is what makes the project yours. It is 200 words of specificity that excludes the entire space of most-probable defaults. Codex, reading it, cannot produce the voiceless output from the chapter opening — the output would directly contradict the specified intent.

The Intent Layer's job is to protect a register that already exists. Seth's actual writing on Zebonastic — the weekly horror-game-psychology articles the platform is built around — opens in this register:

> Here is the foundational fact: your brain cannot distinguish between a threat that will kill you and a threat that will not. The amygdala — the almond-shaped structure buried in your temporal lobe — fires identically whether you see a bear charging at you in a forest or a sprite of a creature lunging at you on a screen. Cortisol spikes. Heart rate climbs. Muscles tense. This is not a metaphor. It is a measurable physiological response documented in fMRI studies, most famously Mathiak & Weber's 2006 work showing that first-person shooter gameplay reliably activates regions associated with real threat response — including the amygdala and anterior cingulate cortex. The brain's threat-detection architecture predates our ability to distinguish fiction from reality by millions of years. Horror games exploit a firmware bug in Homo sapiens.

That register — *the amygdala fires identically*; *firmware bug in Homo sapiens*; the cited 2006 fMRI study; the willingness to use clinical neuroscience vocabulary inside a piece about video games — is the thing the Intent Layer exists to preserve. Without the layer, Codex's most-probable output for "an article about horror games" samples the average of every horror-game article ever written: introductory, atmospheric, oriented to the casual reader. With the layer, Codex's output stays in Seth's register: rigorous, specific, technically grounded, willing to put neurochemistry and game mechanics in the same sentence. The Intent Layer is not instructing Codex how to write. It is naming the register that already exists and must not be averaged away.

The phase gate: **Code Mode does not begin until both layers of PROJECT.md are populated.** Not as a suggestion. As a gate. The classmates' work was voiceless because there was no Intent Layer to start from. Codex filled in defaults for everything the Intent Layer would have specified.

![The three-file system as three nested layers](images/11-brutalist-creative-builds-fig-02.png)
*Figure 11.2 — The three-file system as three nested layers*

| File | What it holds | Who writes it | What happens if it's missing |
|---|---|---|---|
| AGENTS.md | Technical constitution — commands, stack, conventions, invariants, lessons learned | The builder (human), updated after each session | Codex re-infers conventions every session; the same misalignment is rediscovered again and again |
| DESIGN.md | Visual constitution — the six colors, type ramp, spacing scale, allowed and forbidden interactions | The builder (human), once and rarely revised | The build drifts to the most-probable visual default — Bootstrap-shaped, gradient-shaped, voiceless |
| PROJECT.md | Project state in two layers — Intent Layer (human-only) and Technical State (Codex-updatable) | Intent Layer: human, always. Technical State: collaborative | Codex fills the intent gap with the average of "an article platform" and the voice is averaged away |

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

![Same data, same Codex, different files. The 30-minute investment is visible in the output.](images/11-brutalist-creative-builds-fig-03.png)
*Figure 11.3 — Chart comparison *

---

## The 45-minute scope test

A forcing function that keeps creative builds appropriately sized: **if writing all three files takes more than 45 minutes, the project is too large for a first creative build.**

The Zebonastic example passes the test — the three files can be written in 30–40 minutes if the intent is clear. A more ambitious project (a multi-page interactive essay, a small game, a custom data visualization with non-standard interactions) takes longer to specify and benefits from being scoped down for the first build.

The test is not about whether you *can* write longer files. It is about whether the first creative build should be ambitious enough to *need* longer files. The answer is almost always no. Make it small. Make it complete. Ship it. Then build the next one larger. The discipline of completing a small, specified project builds the skill the larger projects will need.

---

## What the three-file system is not

**It is not for designers.** Anyone building anything with visual output is making design decisions. The choice is whether you make them explicitly — in the DESIGN.md, with your specific preferences stated — or whether you make them by omission and let Codex sample the average. The chapter argues for explicit.

**It is not overhead for small projects.** The 45-minute scope test forces appropriate sizing. For a small project, the three files will be short and will take 20 minutes to write. Write them anyway. They protect against exactly the failure mode that opened the chapter.

**It is not about distrust of Codex.** Codex's defaults are reasonable. They are the average of what has worked across its training data. The three-file system is not a claim that Codex's defaults are bad. It is a claim that your specific choices are not the average, and the files are how you specify the difference.

**It is not a substitute for having aesthetic opinions.** The DESIGN.md needs to specify six colors. If you do not have opinions about which six colors, the DESIGN.md will be vague, and Codex will sample the average anyway. The discipline works only if you have made the decisions the files are asking you to make. Making those decisions is part of the work.

---

## Reference implementations at full strength

The Brutalist three-file system specifies a creative project. The same discipline — a structured specification with allowed and forbidden moves, phase gates, refusals as features — also produces strong AI agents. The appendix (`chapters/98-appendix-walker-and-zelda.md`) reproduces Walker and Zelda, two production agent prompts Seth and I co-built, public at humanitarians.ai/tools. Zelda in particular is the closest cousin to this chapter: a senior game-design consultant with a 34-command library across five phases, a phase-gate enforcement layer, a pushback layer that refuses weak input, and a 7-failure-mode audit pass. Read as Brutalist files at full strength: every command specified, every refusal explicit, every phase gated. When a creative build's three files start to feel skeletal, the appendix shows what the form scales to.

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

2. *(Tests: file responsibilities)* For each of the following decisions, name which file it belongs in and explain why: (a) the project uses Next.js with no static-export step; (b) link hover underlines glow from muted violet-gray to hot neon pink; (c) Zebonastic is for builders, not consumers; (d) the `.next/` folder should never be modified by Codex; (e) no animations longer than 200ms.

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
