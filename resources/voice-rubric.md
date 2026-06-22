# Voice Rubric. 1-10 Grading for `/scribe:critique`

Loaded by `/scribe:critique`. Pick the rubric block for the piece's format, score each dimension, weight, sum.

The skill supports two output modes: **score mode** (default, single-piece narrative drafts) and **rewrite mode** (side-by-side `Original | Suggested | Why` tables for tabular UI / copy artifacts). Scoring rules below apply to both. See the skill's Step 5 for output formatting per mode.

## How to use this file

1. Identify format from `.scribe-context.md` (or ask user if missing).
2. Score universal dimensions (always graded).
3. Score format-specific dimensions (varies).
4. Apply weights → overall 1-10.
5. For every dimension below max, write a `Path to 10` line: exact paragraph/sentence + concrete edit.
6. Pick top 3 highest-leverage moves.
7. Honest ceiling note if format caps realistic score.

---

## Universal dimensions (always graded)

Total weight 50% across all formats.

### 1. Clarity (10%)
- 10: Point lands on first read. No re-reading needed. No ambiguous referents.
- 7: Clear after one pass. Maybe one fuzzy sentence.
- 4: Reader has to work to extract the point.
- 1: Buried, contradictory, or absent.

### 2. Voice match (10%)
Compare against saved voice profile (or samples in context file).
- 10: Indistinguishable from user's own writing. Sentence rhythm, vocabulary, quirks all match.
- 7: Mostly matches; one or two passages drift toward generic.
- 4: Generic competent prose; doesn't sound like the user.
- 1: Sounds like a different person (or like AI).

### 3. Audience fit (10%)
- 10: Register, vocabulary, and assumptions about prior knowledge exactly match the audience.
- 7: Close; minor mismatch (slight over-explanation or under-explanation).
- 4: Wrong register or wrong assumed knowledge level.
- 1: Talks past audience or condescends.

### 4. Structure (10%)
- 10: Opening hooks. Middle flows. Ending lands. No section feels like filler.
- 7: Solid structure, weak opening or weak ending.
- 4: Disorganized; reader loses thread.
- 1: No structure.

### 5. Human-ness / anti-AI (10%)
Cross-check against `ai-tells.md`.
- 10: Zero AI tells. Would not trip detectors. Reads as written by a specific person.
- 7: One or two minor tells (e.g., one "leverage", one forced trio).
- 4: Multiple tells across vocabulary, structure, or tone.
- 1: Reads as obvious AI output.

---

## Format-specific dimensions

Pick ONE block below. Total weight 50%.

### Email to boss / executive

- **Brevity (15%):** Under 250 words. No padding. 10 = every sentence necessary.
- **BLUF / point upfront (15%):** Decision, ask, or status in first 2 sentences. 10 = subject line itself states the point.
- **Clear ask (15%):** What boss needs to do/decide, by when. 10 = specific action + specific deadline.
- **No over-apology / hedging (5%):** Direct, professional. 10 = takes responsibility cleanly, no grovelling.

### Email to vendor

- **Specific ask (15%):** Exact deliverable. 10 = no ambiguity in what's requested.
- **Deadline + impact (15%):** What you need, when, what happens if missed. 10 = all three present.
- **Reference data (10%):** Ticket #s, circuit IDs, contract refs, dates included accurately.
- **Tone calibration (10%):** Firm but professional. 10 = matches escalation level appropriately.

### Cold outreach email

- **Hook (15%):** First sentence earns the second. 10 = specific to recipient, not template.
- **Relevance to recipient (15%):** Why them, why now. 10 = clear research, not generic.
- **Low-friction CTA (10%):** One small ask. 10 = under 15 minutes of recipient time to act.
- **Brevity (10%):** Under 150 words. 10 = scannable in 8 seconds.

### Internal team update / status email

- **Scannable structure (15%):** Bullets where they help, prose where it flows. 10 = reader gets gist in 30 seconds.
- **Owners + dates (15%):** Every action item has name + date. 10 = no orphan tasks.
- **Risk surfacing (10%):** Bad news stated plainly, not buried.
- **Right level of detail (10%):** Enough context, not too much.

### Difficult / sensitive communication (delivering bad news)

- **Honesty without excess apology (15%):** Owns the issue. 10 = states facts, takes responsibility, no defensive padding.
- **Impact stated for recipient (15%):** How it affects them, not just you.
- **Resolution path (15%):** What's being done, when. 10 = concrete plan + timeline.
- **Tone (5%):** Calm, direct, no passive voice hiding agency.

### LinkedIn post / professional social

- **Hook in first 2 lines (15%):** Before the "see more" cut. 10 = stops scrolling.
- **Opinion strength (15%):** Takes a position, not balanced mush.
- **Scannability (10%):** Line breaks, short paragraphs. 10 = mobile-readable.
- **Takeaway (10%):** One thing reader leaves with. 10 = quotable.

### Blog post / article / long-form

- **Hook (10%):** Opens with a specific thing, not throat-clearing.
- **Argument strength (15%):** Each section earns its place. 10 = nothing cuttable without loss.
- **Evidence / specificity (15%):** Concrete examples, numbers, names. Not abstraction. 10 = every claim grounded.
- **Ending (10%):** Lands. Not "in conclusion" filler. 10 = leaves reader with something to do or think.

### Op-ed / persuasive piece

- **Argument strength (15%):** Claim is sharp; reasoning holds.
- **Evidence credibility (10%):** Sources, examples, numbers withstand pushback.
- **Counterargument handling (15%):** Strongest opposing view addressed honestly. 10 = steelmans the other side.
- **Memorable line (10%):** At least one quotable sentence. 10 = the line travels.

### How-to / explainer

- **Accuracy (15%):** Facts verifiable. 10 = nothing wrong.
- **Step clarity (15%):** Each step actionable in isolation.
- **Prereqs explicit (10%):** Reader knows what they need before starting.
- **Examples concrete (10%):** Real values, real commands, real outputs. Not placeholders.

### Personal letter / family / community

- **Sincerity (20%):** Sounds like the person, not the polished version. 10 = unguarded but appropriate.
- **Specific not generic (15%):** Names, shared moments, concrete details. 10 = could only be from this person to this person.
- **Right emotional register (15%):** Matches relationship. 10 = neither over- nor under-shoots.

### Memo / report / internal doc

- **Executive summary present (10%):** TL;DR at top.
- **Structure / navigability (15%):** Headings, bullets where useful. 10 = reader can jump to section they need.
- **Recommendation strength (15%):** If it's a decision doc, the recommendation is clear and reasoned.
- **Accuracy / sourcing (10%):** Claims sourced where non-obvious.

### Other / unspecified

Default mix:
- **Specificity (15%):** Concrete over abstract.
- **Strong opening (10%):** First sentence earns the second.
- **Strong ending (10%):** Lands, doesn't trail off.
- **Coherence (15%):** Sticks to one purpose.

---

## Path to 10. Output guidance

For every dimension scoring below 10, produce one line:
```
[Dimension] (X/Y → 10): [exact paragraph or sentence] → [concrete edit]
```

Example:
```
Voice match (6/10 → 10): para 3 "leverages best-in-class methodology" → user samples never use "leverage" or "best-in-class". Rewrite: "uses the same approach we used on the Acme project".
```

Then pick **top 3 highest-leverage moves**. Biggest score gain for least effort. Format:
```
1. [Move]. Current: [state], target: [state], edit: [concrete change]. Estimated lift: +X points.
2. ...
3. ...
```

Then list **stretch moves** specific to the format:
- Op-ed: "Needs one quotable line. Try compressing para 4's argument into a single 15-word sentence."
- LinkedIn: "Add a concrete number in the hook. 'I rebuilt our deploy pipeline' → 'I cut our deploy from 47 minutes to 4'."
- Email to boss: "Move the ask into the subject line. Subject becomes the decision itself."
- Cold outreach: "Add one personalized line referencing recipient's recent work."
- Article: "Add one strong counterargument and address it. Strengthens credibility."
- Personal letter: "Replace one general statement with a specific shared memory."

Then **ceiling note** if applicable:
```
Ceiling: This format (weekly status update to a peer) realistically caps around 8.5. A 10 would require this piece to do something status updates don't normally do, e.g., shift a decision the team has been avoiding. Worth aiming for, but not required to ship.
```

---

## Scoring math

Per dimension: score 1-10. Weighted by % above. Sum = overall 1-10 (or 1-100, scaled).

Round to one decimal. Show the math in the report so the user sees where points came from and where they went.

## Interpretation

| Score | Status |
|---|---|
| 9.5-10 | Ship as-is |
| 8.5-9.4 | Minor polish. Optional refine pass |
| 7.0-8.4 | One more `/scribe:refine` recommended |
| 5.0-6.9 | Significant rework. Refine targeting top 3 moves |
| < 5.0 | Reframe; consider new draft with different approach |
