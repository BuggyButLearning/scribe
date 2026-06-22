---
description: Interview the user grill-me style, then write a first draft of an article, email, letter, post, or other piece in their voice. Use when user wants to start a new piece of writing.
argument-hint: "[optional: short topic description]"
allowed-tools: [Read, Write, Edit, Glob, AskUserQuestion]
---

<objective>
Interview the user and produce a first-draft piece of writing that sounds like *they* wrote it.

**When to use:** Starting any new article, email, letter, post, memo, op-ed, how-to, or other piece of writing. Not for editing existing content (use `/scribe:refine` for that).
</objective>

<resources>
Before interviewing or drafting, read these bundled resource files with the Read tool. `${CLAUDE_PLUGIN_ROOT}` is substituted to this plugin's install directory:
- `${CLAUDE_PLUGIN_ROOT}/resources/ai-tells.md` — AI-writing tells to strip
- `${CLAUDE_PLUGIN_ROOT}/resources/interview-questions.md` — the grill-me interview question bank
</resources>

<voice_catalog>
Saved voice profiles persist in your home directory (not the plugin cache, so they survive plugin updates) at `~/.claude/scribe/voices/`. Read `~/.claude/scribe/voices/index.md` with the Read tool if it exists to list saved voices. If the file or directory does not exist yet, treat the catalog as empty — it gets created the first time the user saves a voice.
</voice_catalog>

<context>
$ARGUMENTS
</context>

<process>

## Step 1 — Voice profile

Read `~/.claude/scribe/voices/index.md` if it exists (per voice_catalog above). Branch:

- **Catalog has voices:** List them by slug + description via `AskUserQuestion`. Options: use one of them, create a new voice, or skip voice profile (write neutral).
- **Catalog empty:** Ask the user once: "Got writing samples you want me to match? Paste 1-3 examples, or give me a file path. Or skip if you want me to write neutral."

If user picks an existing voice: read `~/.claude/scribe/voices/<slug>.md` and load its patterns.

If user provides samples: analyze them (sentence rhythm, vocabulary level, paragraph length, punctuation habits, recurring phrases, transitions). Hold the analysis in memory. Do NOT save yet — save at end after user confirms.

If user skips: use the default voice rules in `ai-tells.md` (natural, direct, human, no AI tells).

## Step 2 — Grill the user

Use the question bank in `interview-questions.md`. Rules:

- Ask one or two questions per turn. Never a wall.
- Skip anything already answered in `$ARGUMENTS` or earlier replies.
- Prefer `AskUserQuestion` tool when 2-4 options make sense; ask in chat for open-text.
- Resolve ambiguity before moving on. If user says "my team", ask team size + role.
- Stop interviewing once you can write a competent first pass.

Cover at minimum:
1. Identity — who the user is in this piece (role, stake, relationship to topic)
2. Audience — who reads it, what they know, formality level
3. Format — email type / article type / letter / post / memo / etc.
4. Length target (or accept a sensible default for the format)
5. Topic + goal — what it's about, what should reader do/believe/feel after
6. Approach — argumentative / explanatory / story / warning / persuasive / informative
7. **Outline option** — ask explicitly: "Want to give me a bullet outline of the points first, or just describe the situation and I structure it?"
   - If outline: user pastes bullets. Ask one clarifying question per ambiguous bullet.
   - If describe-and-structure: skill picks the structure.
8. Constraints — must-include facts/names/dates, must-avoid words/topics, deadline.

## Step 3 — Write the draft

Apply these rules every time:

- Match voice profile sentence rhythm, vocabulary level, paragraph length, punctuation habits.
- Vary sentence length (Gary Provost rule — fragments, short, medium, long).
- Preserve all user-provided facts, names, dates, technical details. Do NOT invent facts.
- Grammar fixes only when needed for clarity. Keep user's natural quirks if samples show them.
- Strip every AI tell in `ai-tells.md`. Cross-check before finalizing.
- Active voice default. Contractions natural. Specific over abstract.
- Format conventions:
  - Email: subject line + body. No fake warmth, no "I hope this finds you well", no "thank you in advance".
  - Article: hook in first sentence. No "in today's landscape". No "in conclusion".
  - LinkedIn: hook in first 2 lines (pre-cutoff). Short paragraphs.
  - Op-ed: take a position, address strongest counterargument.
  - Personal letter: specific shared details, no performative phrasing.
- **Final anti-AI pass.** Before output, ask yourself: "What about this still sounds AI?" Name 1-2 remaining tells. Revise to remove them.

## Step 4 — Write context file

Create `.scribe-context.md` in CWD with this structure:

```markdown
# Scribe Context

## Voice
- Profile used: [slug or "new (unsaved)" or "neutral"]
- Samples: [brief note if new]

## Identity
[who the user is in this piece]

## Audience
[who reads, what they know, formality]

## Format
[email-to-boss / article / etc.] — target length: [N words]

## Topic + goal
[topic, then goal in concrete reader-action terms]

## Approach
[argumentative / explanatory / etc.]

## Outline
[if user gave one, paste verbatim; else "none — drafted from description"]

## Must-include
- [item]

## Must-avoid
- [item]

## Deadline / notes
[anything else]

---

## Draft v1
[paste the full draft here]

## Revisions
<!-- /scribe:refine appends v2, v3, etc. below -->
```

If `.scribe-context.md` already exists in CWD, ask user: "There's already a scribe context here for `<topic>`. Overwrite, or use a different filename (.scribe-context-<slug>.md)?"

## Step 5 — Save voice (if new)

If user provided new samples this session, ask: "Want to save this voice for future pieces?"

- If yes: ask for slug (or suggest one based on context — e.g., `email-vendor-firm`, `linkedin-personal`, `dad-to-school`). Ask for one-line description.
- Create the `~/.claude/scribe/voices/` directory if it does not exist yet, then write `~/.claude/scribe/voices/<slug>.md` with this structure:

```markdown
---
name: <slug>
description: <one-line description>
created: <YYYY-MM-DD>
samples_provided: <count>
---

## Sentence patterns
- Average length: ~N words; range A-B
- Fragments: [frequent / rare / never]
- Rhythm notes: [describe]

## Vocabulary level
- [formal / casual / mixed]
- Jargon comfort: [high / medium / low]
- Signature words/phrases: [list any]

## Habits
- Contractions: [yes / no / mixed]
- Em-dashes: [frequent / rare]
- Paragraph openers: [common patterns]
- Punctuation tics: [list]

## Avoid (per user)
- [list]

## Source samples
[paste 1-2 short representative excerpts for future reference]
```

- If `~/.claude/scribe/voices/index.md` does not exist yet, create it with a `# Saved Voice Profiles` heading first. Then append:
  ```
  - [<slug>](<slug>.md) — <description>
  ```

## Step 6 — Output

Show the draft in chat. End with:

> Saved to `.scribe-context.md`. Next: run `/scribe:refine` to interview and produce v2, or `/scribe:critique` to score it 1-10.

</process>

<success_criteria>
- [ ] Voice profile loaded, created, or explicitly skipped
- [ ] At least format, audience, topic, and goal known before drafting
- [ ] Outline option explicitly offered
- [ ] Draft written, AI tells stripped, voice matched
- [ ] `.scribe-context.md` written to CWD with full context + draft
- [ ] New voice (if any) saved to catalog after user confirmation
- [ ] Suggested next step in output
</success_criteria>
