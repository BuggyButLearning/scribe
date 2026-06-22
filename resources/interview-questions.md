# Interview Question Bank — Grill-Me Style

Loaded by `/scribe:draft` and `/scribe:refine`. Use `AskUserQuestion` tool. Ask one or two questions per turn. Only ask what's missing. Skip what context already provides.

## Grilling principle

Like `/grill-me`: short, targeted, conversational. Don't pile every question into one wall. Resolve one branch before opening the next. If user answer raises a follow-up, ask it before moving on. Never let an ambiguity slide.

---

## `/scribe:draft` — questions in priority order

### Branch A: Voice (always first)

1. **Saved voices check.** Read `~/.claude/scribe/voices/index.md` if it exists.
   - If voices exist: list them. Ask: "Use one of these, create new, or skip voice profile?"
   - If none: ask: "Got writing samples you want me to match? Paste 1-3, or give me a file path. Or skip if you want me to write neutral."
2. **If creating new voice:** after samples received, ask for 1-3 words to label it (e.g., "professional-engineer", "dad-to-school", "linkedin-casual"). Defer saving until end of draft.

### Branch B: Identity + audience

3. **Who is the user, in this piece?**
   - "Who are you, writing this? Role, relationship to subject, stake."
   - Example answers: "Senior network engineer writing to broadcast VP about a real circuit risk." / "Dad writing to school principal about my kid's bullying issue." / "Founder writing to potential investor."
4. **Audience.**
   - "Who reads this? What do they already know about the topic? How formal?"
   - Probe: if user says "my team", ask team size + tech level. If "the public", ask which subset.

### Branch C: Format

5. **Format pick.** Options:
   - Email (sub-type: boss, vendor, cold outreach, internal update, difficult news)
   - Letter (personal, formal, complaint)
   - Article (blog, LinkedIn, op-ed, how-to, explainer)
   - Memo / report / internal doc
   - Social post
   - Other (user describes)
6. **Length target.** Rough word count or read-time. Some defaults if user says "you pick":
   - Email to boss: <250 words
   - Cold outreach: <150 words
   - LinkedIn post: 150-300 words
   - Article: 600-1500 words
   - Op-ed: 700-1000 words

### Branch D: Substance

7. **What's it about?** Topic in plain words.
8. **Goal.** "What should the reader do, believe, or feel after reading?" Force concrete answer — not "be informed".
9. **Approach.**
   - Argumentative (taking a side)
   - Explanatory (here's how X works)
   - Storytelling (here's what happened)
   - Warning / alarm-raising
   - Persuasive (do this thing)
   - Informative / status

### Branch E: Outline option

10. **Outline first?** Ask: "Want to give me a bullet outline of the points to hit? Or describe the situation and let me structure it?"
    - If outline path: user pastes bullets. Skill asks clarifying question per ambiguous bullet (e.g., "bullet 3 says 'mention the Q3 incident' — what should reader know about it?"). Then drafts.
    - If no outline: skill drafts from interview answers directly.

### Branch F: Constraints

11. **Must-include.** Facts, names, dates, quotes, technical details that cannot be lost or invented.
12. **Must-avoid.** Words, topics, names, framings the user does not want.
13. **Deadline / urgency.** Affects tone (urgent = more direct).
14. **Any other context.** Open-ended catch-all. Last question.

---

## `/scribe:refine` — questions in priority order

### Branch A: Open read

1. **What landed? What felt off?**
   - Open question. Let user point.
   - Then probe their answer specifically before moving on.

### Branch B: Targeted probes (only ask if user didn't flag)

2. **Opener.** "Does the first sentence pull you in? Or feels like throat-clearing?"
3. **Ending.** "Does it land? Or trail off?"
4. **Tone.** "Right register for the audience? Too formal? Too casual?"
5. **Voice.** "Any sentence that doesn't sound like you?" (then match that sentence specifically against voice profile)
6. **Length.** "Too long? Too short? Right?"
7. **Audience fit.** "Would the reader follow this without context you didn't include?"

### Branch C: Substantive changes

8. **New facts.** "Anything to add — fact, example, quote, anecdote?"
9. **Things to cut.** "Anything to remove — section that's not pulling weight?"
10. **Reframe.** "Want me to rewrite a section from scratch vs. nudge what's there?"
11. **Pivot.** "Did your goal for this piece change while reading it?"

### Branch D: Output preference

12. **Diff or full?** "Want me to show full v2, or just the changes?"

---

## Asking style

- Use `AskUserQuestion` tool when 2-4 multiple-choice options make sense.
- For open-text answers (samples, topic, must-includes), ask in chat.
- One or two questions per turn maximum. No walls.
- If user gives short answer, accept and move on. Don't grill.
- If user gives ambiguous answer, ask one follow-up before next branch.
- Skip any question the user has already answered in their initial request.

## When to stop interviewing

Stop and draft when:
- Voice is set (or skipped)
- Identity + audience clear
- Format known
- Topic + goal clear
- Outline decision made
- At least one round of must-include / must-avoid

Don't keep asking once you can write a competent first pass. The refine step exists to catch what the interview missed.
