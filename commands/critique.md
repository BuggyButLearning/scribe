---
description: Score a draft 1-10 against an audience-aware rubric. Reports per-dimension scores, top issues with line refs, what's working, and a concrete path to 10/10. Use after /scribe:draft or /scribe:refine to evaluate quality before shipping.
argument-hint: "[optional: format override or focus area]"
allowed-tools: [Read, Glob, AskUserQuestion]
---

<objective>
Score the current draft on a 1-10 scale, identify what's holding it back, and lay out the concrete path to a 10/10.

**When to use:** After `/scribe:draft` or `/scribe:refine`, when the user wants an honest quality assessment before shipping. Or any time the user has a draft they want graded.
</objective>

<resources>
Before scoring, read these bundled resource files with the Read tool. `${CLAUDE_PLUGIN_ROOT}` is substituted to this plugin's install directory:
- `${CLAUDE_PLUGIN_ROOT}/resources/ai-tells.md` — AI-writing tells to scan for
- `${CLAUDE_PLUGIN_ROOT}/resources/voice-rubric.md` — the 1-10 grading rubric, format-aware
</resources>

<context>
$ARGUMENTS
</context>

<process>

## Step 1. Load draft + context + pick output mode

Look for `.scribe-context.md` (or `.scribe-context-*.md`) in CWD.

- **Found:** Read it. Use the latest version in `## Revisions`, or the original `## Draft v1` if no revisions yet. Pull format, audience, identity, goal from the context.
- **Not found:** Ask user: "No scribe context here. Paste the draft to critique, plus: format (email-to-boss / article / op-ed / cold outreach / letter / etc.) and audience."

If voice slug referenced: read `~/.claude/scribe/voices/<slug>.md` to compare draft against.

**Output mode selection:**

- **Score mode (default).** Scoring header + Top 3 issues + Path to 10 + Next step. Use this for single-piece drafts (one article, one email, one letter) where the user wants a quality read.
- **Rewrite mode (side-by-side tables).** Triggered when any of:
  1. The source is a multi-section artifact: UI copy (start page + result page), question banks, label sets, growth tip lists. Anything with naturally tabular structure where the user will apply edits row by row.
  2. The user explicitly asks for "before / after" or "side by side" rewrites.
  In rewrite mode you produce the same scoring header but follow it with a `## Side-by-side rewrites` section instead of the Top-3 / Path-to-10 narrative.

If the source looks tabular but the user's intent is ambiguous, ask once via `AskUserQuestion` which mode they want; default the recommended option to rewrite mode for UI / copy artifacts.

## Step 2. Pick rubric

From `voice-rubric.md`, pick the format-specific block matching the piece. If format is ambiguous or `$ARGUMENTS` overrides, ask user via `AskUserQuestion` with the closest 2-4 options.

Universal dimensions always graded:
1. Clarity (10%)
2. Voice match (10%)
3. Audience fit (10%)
4. Structure (10%)
5. Human-ness / anti-AI (10%)

Format-specific dimensions: load from the matching rubric block (totals 50%).

## Step 3. Score

For each dimension:
- Read the dimension's anchor descriptions in `voice-rubric.md`.
- Pick a 1-10 score honestly. Cite specific paragraphs/sentences as evidence.
- Multiply by weight.

Sum to overall (1-10, one decimal).

## Step 4. Anti-AI scan

Cross-check the draft against `ai-tells.md`:
- Vocabulary blacklist. Note every hit
- Phrase blacklist. Note every hit
- Structural tells. Note any forced trios, mechanical bold labels, uniform paragraphs, em-dash overuse
- Tone tells. Sycophancy, over-hedging, fake warmth

This feeds the Human-ness score and the top-3 issues list.

## Step 5. Write report

Pick the output template based on mode (see Step 1).

### Score mode (default). Output structure:

```markdown
# Critique. [filename or piece title]

**Overall: X.X / 10**. [Ship / Polish / Refine / Rework / Reframe]

Format: [identified format]
Audience: [from context]

## Scores

| Dimension | Weight | Score | Weighted |
|---|---|---|---|
| Clarity | 10% | X/10 | X.X |
| Voice match | 10% | X/10 | X.X |
| Audience fit | 10% | X/10 | X.X |
| Structure | 10% | X/10 | X.X |
| Human-ness | 10% | X/10 | X.X |
| [format-specific dim 1] | X% | X/10 | X.X |
| [format-specific dim 2] | X% | X/10 | X.X |
| [format-specific dim 3] | X% | X/10 | X.X |
| [format-specific dim 4] | X% | X/10 | X.X |
| **Total** |  |  | **X.X** |

## Top 3 issues

1. **[Dimension]. [issue title]**
   - Where: paragraph X, "[exact quoted sentence]"
   - Problem: [specific]
   - Suggested fix: [concrete edit, not vague advice]

2. ...
3. ...

## What's working (preserve in next pass)

- [Specific paragraph/sentence]. Why it works
- [Another]. Why
- [Another if relevant]. Why

## Path to 10/10

For every dimension scoring below 10:

- **Clarity (X/10 → 10):** [exact paragraph/sentence] → [concrete edit]
- **Voice match (X/10 → 10):** [exact passage] → [concrete edit grounded in voice profile]
- **[Each remaining dim]:** ...

### Top 3 highest-leverage moves

1. **[Move]**. Current: [state], target: [state], edit: [concrete change]. Estimated lift: +X.X points.
2. ...
3. ...

### Stretch moves (format-specific)

- [One or two moves specific to this format that would make the piece memorable, not just correct. Examples in voice-rubric.md.]

### Ceiling note

[If applicable: "This format (weekly status update to a peer) realistically caps around X.X. A 10 would require [specific thing that may not be worth the effort for this piece]."]

## Next step

- If score ≥ 9.5: **Ship.**
- If 8.5-9.4: Optional `/scribe:refine` targeting top issue.
- If 7.0-8.4: `/scribe:refine` recommended. Focus on top 3 moves.
- If 5.0-6.9: Significant rework via `/scribe:refine`.
- If < 5.0: Reframe; consider `/scribe:draft` again with different approach.
```

### Rewrite mode. Output structure:

Same scoring header at the top so the user can see where the piece stands. Replace the Top 3 issues / Path to 10 / Stretch sections with a `## Side-by-side rewrites` section organised by source artifact section. End with the same Next step block.

```markdown
# Critique. [filename or piece title] (rewrite mode)

**Overall: X.X / 10**. [Ship / Polish / Refine / Rework / Reframe]

Format: [identified format]
Audience: [from context]

## Scores

| Dimension | Weight | Score | Weighted |
|---|---|---|---|
| ... (same table as score mode) ... |
| **Total** |  |  | **X.X** |

## Voice rules applied in this rewrite

State the rules up front so the user can audit them. Examples:
1. Em dashes removed everywhere. Replace with periods, commas, or parens.
2. Specific banned words (carry from `.scribe-context.md` or call out by name).
3. Tighten only. No restructuring unless the line is doing two jobs in one.
4. KEEP marker permitted for rows that already serve their job.

## Side-by-side rewrites

For each section / table / group in the source, produce one four-column table:

### [Section name or source identifier]

**Stem / context (if any). Verbatim.**

| # / id | Original | Suggested | Why |
|---|---|---|---|
| 1 | [verbatim source string] | [rewritten string OR `KEEP`] | one-line reason |
| 2 | ... | ... | ... |

Rules for the table:
- **Original** column carries the source string verbatim, including any em dashes the rewrite is proposing to remove. This is the only place em dashes are allowed to appear in the report.
- **Suggested** column is em-dash-free and follows every voice rule listed above. Use `KEEP` (literal, capitalised) when no change is proposed.
- **Why** column is one line. Concrete. Not vague. "em dash to period" beats "improve flow".
- Default heavily toward `KEEP`. A change has to earn its row.

Repeat the table per logical section. Group section tables by source file when crossing files.

## Net summary

After all tables, a short summary:
- Total rows audited.
- Changes proposed (counted by category: em-dash strips, banned-word swaps, tightenings).
- KEEP count.

## Apply checklist

Bulleted steps the user (or a follow-up session) can execute literally to apply the rewrites:
- File paths to edit.
- The exact way to verify (tests to run, dev server to launch, paths to spot-check).
- Anything explicitly out of scope.

## Next step

(Same score-band recommendation block as score mode. Plus: "Approve the doc to move on to apply phase.")
```

## Step 6. Honesty rules

- Score honestly. A 7 is a 7. Don't grade-inflate to be encouraging.
- Don't grade-deflate either. If the draft is genuinely good, say 9.
- In score mode: every score below 10 must have a `Path to 10` entry. No exceptions.
- In rewrite mode: every Original row must have a Suggested cell. `KEEP` counts as a Suggested cell.
- Every concrete edit (path-to-10 or Suggested cell) must be specific: exact location + exact replacement. No vague advice like "tighten the prose" or "vary sentences more".
- Top 3 moves (score mode) must be ranked by score-impact-per-effort. Biggest gain for least work first.
- The ceiling note is for honesty, not a cop-out. Only invoke it if the format genuinely caps the score.
- The em-dash rule for the report itself: zero em dashes outside of `Original` cells. Verbatim quotes are the only place an em dash may appear in the output.

</process>

<success_criteria>
- [ ] Draft + context loaded (or user provided both)
- [ ] Output mode chosen (score vs rewrite) per Step 1 rules
- [ ] Format-specific rubric picked
- [ ] All 9 dimensions scored with evidence
- [ ] Overall score calculated, one decimal

Score mode only:
- [ ] Top 3 issues with line refs + concrete fixes
- [ ] What's working called out
- [ ] Path to 10 for every below-10 dimension
- [ ] Top 3 highest-leverage moves ranked
- [ ] Stretch moves listed
- [ ] Ceiling note included if format caps realistic score

Rewrite mode only:
- [ ] Voice rules listed before the tables
- [ ] One four-column table per source section
- [ ] Every Original row has a Suggested cell (KEEP allowed)
- [ ] Zero em dashes outside Original cells
- [ ] Net summary at the end (rows audited, changes, KEEP count)
- [ ] Apply checklist with file paths and verification steps

Both modes:
- [ ] Next-step recommendation matches the score band
</success_criteria>
