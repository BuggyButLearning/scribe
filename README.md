<p align="center">
  <img src="assets/scribe-banner.png" alt="Scribe — write like you, not like AI" width="100%">
</p>

# Scribe

A Claude Code plugin for writing articles, emails, letters, posts, and other communications that **sound like you wrote them** — not like AI.

Three slash commands, a per-piece context file, an audience-aware grading rubric, and a curated list of AI-writing tells to strip from every draft.

```
/scribe:draft     → interview + first draft
/scribe:refine    → interview + v2 (loops for v3, v4...)
/scribe:critique  → 1-10 score + path to 10/10
```

---

## Why this exists

Most AI writing has a fingerprint. Forced trios. "It's worth noting." "In conclusion." Mechanical bold labels. Em-dash overuse. Sycophantic openers. Reflexive both-sides framing. Detectors flag it; human readers can smell it even when they can't name it.

Scribe takes a different approach: it interviews you the way a ghostwriter would, then writes in **your** voice based on your own samples — with grammar fixes only when clarity needs them. Then it grades the result honestly and tells you exactly what to change to make it better.

## What it does

### `/scribe:draft` — interview + first draft

Asks you, one or two questions at a time:

1. Which saved voice to use (or create a new one from samples you paste)
2. Who you are in this piece — role, stake, relationship to topic
3. Who reads it — audience, formality, prior knowledge
4. Format — email-to-boss / vendor / cold outreach / LinkedIn / op-ed / how-to / letter / memo / other
5. Topic + concrete goal (what the reader should do, believe, or feel)
6. Approach — argumentative / explanatory / story / warning / persuasive / informative
7. **Outline option** — give bullet points first, or describe and let it structure
8. Constraints — must-include facts, must-avoid words/topics, deadline

Writes the draft. Saves everything to `.scribe-context.md` in the current directory. Offers to save your voice to the catalog for future use.

### `/scribe:refine` — second-pass interview

Reads the context file. Asks:

- Read it again. What landed? What felt off?
- Opener strong, or throat-clearing?
- Ending lands, or trails?
- Any sentence that doesn't sound like you?
- Anything to add? Cut? Reframe?
- Did your goal shift while reading?

Produces v2. Appends to context file. Loops indefinitely for v3, v4.

### `/scribe:critique` — 1-10 grade

Picks the rubric matching your format (LinkedIn post is weighted differently than an escalation email or an op-ed).

Outputs:

- **Overall: X.X / 10**, with weighted dimension scores
- **Top 3 issues**, each with exact location + concrete fix
- **What's working** (so the next refine doesn't quietly delete it)
- **Path to 10/10** — for every below-10 dimension, an exact paragraph + exact edit
- **Top 3 highest-leverage moves** ranked by score-impact-per-effort
- **Stretch moves** specific to the format (op-eds need one quotable line; LinkedIn needs a number in the hook; etc.)
- **Ceiling note** when the format honestly caps the achievable score

No grade inflation. No vague advice. Every below-10 score comes with a concrete edit.

---

## Installation

Scribe is a Claude Code plugin. This repo is both the plugin and its own marketplace, so installing it is two commands inside Claude Code:

```
/plugin marketplace add BuggyButLearning/scribe
/plugin install scribe@buggybutlearning
```

The first command registers this repo as a marketplace (named `buggybutlearning`); the second installs the `scribe` plugin from it. Reload when prompted.

Verify: type `/scribe:` in any project and you should see `draft`, `refine`, and `critique`.

Works on every project automatically. No per-project setup.

### Updating

```
/plugin marketplace update buggybutlearning
```

### Where your data lives

- **Saved voice profiles** are written to `~/.claude/scribe/voices/` — your home directory, not the plugin cache — so they survive plugin updates and are shared across every project.
- **The per-piece context file** (`.scribe-context.md`) is written to whatever directory you run the commands in, so each piece stays with its project.

---

## Usage

### First piece

```
cd <wherever you want the context file saved>
/scribe:draft
```

Scribe asks for samples (paste them, or give a file path). Answers a few questions. Writes the draft. Offers to save your voice for next time.

### Refining

```
/scribe:refine
```

Reads `.scribe-context.md`. Asks what worked and what didn't. Writes v2. Repeat as many times as you want.

### Grading

```
/scribe:critique
```

Reads the latest version. Scores it. Tells you exactly what would push it to 10.

### Picking up later

The context file lives in your project folder. Close the session, come back tomorrow, run `/scribe:refine` — it picks up where you left off.

### Multiple pieces in one project

Use `.scribe-context-<slug>.md` to keep contexts separate. Scribe will prompt if it finds an existing context.

---

## Voice profiles

Saved profiles live at `~/.claude/scribe/voices/<slug>.md`. Each one captures:

- Sentence-length patterns and rhythm
- Vocabulary level, signature words, jargon comfort
- Habits (contractions, em-dashes, paragraph openers, punctuation tics)
- Things to avoid (your preferences)
- Source sample excerpts for future reference

Build a library: `professional-engineer`, `dad-to-school`, `linkedin-personal`, `cold-outreach-warm`, whatever fits how you write across contexts. `/scribe:draft` lists them and lets you pick per piece.

---

## What gets stripped

A short slice of what's in `resources/ai-tells.md`:

- Vocab: *delve, leverage, robust, pivotal, comprehensive, seamless, tapestry, navigate (figurative), nuanced, intricate, stands as, serves as, underscores...*
- Phrases: *"I hope this email finds you well," "just checking in," "in today's landscape," "in conclusion," "let's dive in," "it's worth noting," "from X to Y..."*
- Structural: forced three-item lists, mechanical bold labels, uniform paragraph length, em-dash overuse, smooth transitions on every paragraph, reflexive both-sides framing
- Tone: sycophancy, fake warmth, over-apology, knowledge-cutoff disclaimers, closing pep talks

Every draft and revision runs a final "what about this still sounds AI?" pass.

---

## The rubric

`resources/voice-rubric.md` covers:

**Universal dimensions (always graded, 50% total):**
- Clarity, Voice match, Audience fit, Structure, Human-ness

**Format-specific dimensions (50% total):** one block per format —
- Email to boss · Email to vendor · Cold outreach · Internal team update · Difficult/sensitive communication · LinkedIn post · Blog post / article · Op-ed · How-to / explainer · Personal letter · Memo / report

Each dimension has anchor descriptions for scores 1, 4, 7, 10 so the grading stays honest and repeatable across pieces.

---

## File layout

```
scribe/                          ← this repo = the plugin (and its own marketplace)
├── .claude-plugin/
│   ├── plugin.json              plugin manifest (name: scribe)
│   └── marketplace.json         marketplace manifest (name: buggybutlearning)
├── commands/
│   ├── draft.md                 /scribe:draft
│   ├── refine.md                /scribe:refine
│   └── critique.md              /scribe:critique
├── resources/
│   ├── ai-tells.md              AI-writing tells to strip
│   ├── voice-rubric.md          1-10 grading rubric, format-aware
│   └── interview-questions.md   grill-me question banks for draft + refine
├── voices/
│   └── index.md                 seed catalog (your saved voices live in ~/.claude/)
├── assets/
│   ├── scribe-banner.png        the title image
│   └── scribe-banner.spec.json  the JSON spec it was generated from
├── README.md
└── LICENSE

# Created on your machine as you use it:
~/.claude/scribe/voices/         your saved voice profiles (persist across updates)
~/.claude/scribe/voices/<slug>.md   one file per saved voice
.scribe-context.md               per-piece context + all revisions (in your working dir)
```

The command files reference their bundled resources with `${CLAUDE_PLUGIN_ROOT}` so they resolve wherever the plugin is installed.

---

## Workflow loop

```
/scribe:draft
   ↓
context file + v1
   ↓
/scribe:refine  ←┐
   ↓             │
v2, v3, v4...    │ loop until you're happy
   ↓             │
/scribe:critique │
   ↓             │
score + path to 10
   ↓             │
if < 8: refine ─┘
if ≥ 8: ship
```

---

## Design principles

- **Sound like a person, not an AI.** Voice match is weighted equally to clarity. Detector-tripping fingerprints are explicitly enumerated and removed.
- **Grammar fixes only when clarity needs them.** If your samples end sentences with prepositions or open paragraphs with "And," scribe keeps that.
- **Interview, don't assume.** Five-question minimum before drafting. Skip nothing important. Don't pile every question into one wall.
- **Honest grading.** Every score below 10 comes with an exact edit. No vague advice. Ceiling notes when format genuinely caps the achievable score.
- **One piece, one context file.** Survives session breaks. Refine days later without re-explaining anything.
- **Composable with itself.** Three commands, one workflow. Same pattern as `/paul` — each command suggests the next.

---

## License

MIT — do what you want. Pull requests welcome on the rubric, tells list, and format-specific dimensions.
