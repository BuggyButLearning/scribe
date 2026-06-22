---
description: Interview the user about a draft they've already written (via /scribe:draft or otherwise), then produce a refined v2. Loops for v3, v4 as needed.
argument-hint: "[optional: what to focus on]"
allowed-tools: [Read, Write, Edit, Glob, AskUserQuestion]
---

<objective>
Refine an existing draft based on a targeted interview with the user about what's working and what isn't.

**When to use:** After `/scribe:draft` produced v1, or when user has a draft from elsewhere they want polished in their voice. Not for first drafts (use `/scribe:draft`).
</objective>

<resources>
Before interviewing or revising, read these bundled resource files with the Read tool. `${CLAUDE_PLUGIN_ROOT}` is substituted to this plugin's install directory:
- `${CLAUDE_PLUGIN_ROOT}/resources/ai-tells.md` — AI-writing tells to strip
- `${CLAUDE_PLUGIN_ROOT}/resources/interview-questions.md` — the refine question bank
</resources>

<context>
$ARGUMENTS
</context>

<process>

## Step 1 — Load context

Look for `.scribe-context.md` in CWD (or any `.scribe-context-*.md` variant).

- **Found:** Read it. Identify current latest version (v1 / v2 / etc.) in the `Revisions` section, or the original draft if no revisions yet.
- **Not found:** Ask user: "No scribe context here. Paste the draft you want refined, plus brief context: who you are in this piece, audience, format, and what you want me to focus on. Or run `/scribe:draft` first to build context."

If voice slug is referenced in the context file, read `~/.claude/scribe/voices/<slug>.md`.

## Step 2 — Interview the user

Use the refine question bank in `interview-questions.md`. Rules:

- Start open: "Read it again. What landed? What felt off?" (in chat, not multiple-choice)
- Probe the user's flags before opening new branches. If user says "the ending feels weak", ask one follow-up about the ending before moving on.
- Then ask targeted probes for anything the user didn't address — but only ones likely to matter for this piece. Don't ask about LinkedIn-style hooks if it's an email to a vendor.

Cover at minimum:
1. What landed / what didn't (user's own read)
2. Opener — strong enough? Or throat-clearing?
3. Ending — lands? Or trails off?
4. Tone / register — right for audience?
5. Voice — any sentence that doesn't sound like the user?
6. Length — too long, too short, right?
7. New facts to add (quotes, examples, anecdotes, data)
8. Things to cut (sections not pulling weight)
9. Reframe vs. nudge — full rewrite of a section, or small fixes?
10. Goal pivot — did user's goal change while reading the draft?

Stop interviewing once you have enough to write a meaningfully different v2.

## Step 3 — Write v2

Apply:

- Address every concrete change the user raised. Don't quietly skip anything.
- Preserve voice profile (sentence rhythm, vocabulary, quirks).
- Preserve facts, names, dates from context file. Do NOT invent new ones unless the user provided them in this interview.
- Strip any AI tells that crept in (`ai-tells.md`). Cross-check before finalizing.
- Keep what the user said worked. Don't quietly rewrite passages the user praised.
- Vary sentence length (Gary Provost rule).
- **Final anti-AI pass.** "What about this still sounds AI?" Name remaining tells. Revise.

## Step 4 — Append to context file

In `.scribe-context.md`, under `## Revisions`, append:

```markdown
### v<N>
**Interview notes (what user asked for):**
- [bullet per change]

**Draft v<N>:**
[full v2/v3/etc. text]
```

Increment version number based on what's already in the file.

## Step 5 — Output

Ask early in the process: "Want me to show v<N> in full, or just a diff against v<N-1>?" Default: full.

Show the chosen version. End with:

> v<N> saved to `.scribe-context.md`. Next: run `/scribe:refine` again for v<N+1>, or `/scribe:critique` to score it 1-10.

## Looping

User can run `/scribe:refine` multiple times. Each run reads the latest version, interviews, appends a new revision. No cap.

</process>

<success_criteria>
- [ ] Context file loaded (or user provided draft + context)
- [ ] Open question asked before targeted probes
- [ ] Every change the user requested is addressed in v2
- [ ] No invented facts
- [ ] Voice preserved (cross-checked against profile)
- [ ] AI tells stripped
- [ ] New revision appended to context file with interview notes
- [ ] Suggested next step in output
</success_criteria>
