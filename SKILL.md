---
name: content-repurposer
description: "A Claude skill for repurposing long-form content (video scripts, transcripts, articles) into platform-native derivatives for LinkedIn, X, Medium, and Substack. Treats repurposing as an editorial judgment problem, not a formatting exercise. Includes a context independence test for insight extraction, multiple hook categories per derivative, opinionated voice rules, and built-in self-critique. Supports Claude Code (with commands) and Claude Projects (paste-and-go). Trigger when the user wants to repurpose content, turn a long piece into posts, break something down for social, or get more mileage from existing content."
---

# Content Repurposer

Turns long-form content into platform-native derivatives for LinkedIn, X, Medium, and Substack. Each derivative should feel like it was written for that platform first, not extracted from something longer.

## Why this exists

Off-the-shelf repurposing tools treat the job as a formatting exercise: take a long piece, shorten it, adjust character counts, add hashtags. The output reads like what it is — the same idea at different word counts with no editorial judgment applied.

Repurposing is actually a judgment problem: which ideas can stand alone outside the original context, which platforms amplify which types of ideas, and how to maintain a specific voice across all of them. This skill encodes that judgment.

---

## Commands (Claude Code)

When running in Claude Code, the skill uses explicit commands. In Claude Projects (paste-and-go), the `repurpose` workflow runs automatically when the user pastes source content.

### `start`

First-time setup. Walks the user through a guided interview to establish:

1. **Platforms:** Which platforms do you post on? (LinkedIn, X, Medium, Substack — select all that apply)
2. **Content type:** What does your source content look like? (video scripts, transcripts, written articles, a mix)
3. **Voice preferences:** Any specific voice rules or anti-patterns? The skill ships with an opinionated default voice profile (see Voice Rules below). Users can adopt it as-is, modify it, or replace it entirely.
4. **Strategic context:** What's the goal of your content right now? (build authority, grow audience, support a job search, promote a product, general presence)

Saves responses to `repurposer_state.md` in the working directory. Subsequent commands read this file automatically so the user doesn't repeat themselves.

If `repurposer_state.md` doesn't exist when any other command runs, prompt the user to run `start` first.

### `repurpose`

The full workflow. User pastes source content. The skill runs the complete process:

1. Extract the clean argument from the source
2. Identify standalone insights with context independence testing
3. Assign each insight to its best platform and format
4. Generate derivatives for all applicable platforms, each with multiple hook options and a critique note
5. Produce a leverage ranking for posting order

This is the power command. Use it when you want everything at once.

**Input:** Paste one or more of:
- A polished written version (script, article)
- A raw transcript (what was actually said on camera or in a recording)
- Any context about performance, intent, or strategic goals

If both written and transcript versions exist, the skill treats the written version as the canonical argument and the transcript as a source of authentic phrasing and delivery nuance worth preserving selectively.

**Optional flags:**
- "Ask me questions first" or "interview mode" — the skill asks 2-4 calibrating questions before generating
- "Use [person/sample] as a reference" — the skill calibrates tone toward the referenced style while preserving core voice rules

### `linkedin`

LinkedIn derivatives only. Produces multiple short-form post options (typically 2-5, source-driven) with:
- Different angles per post, each surfacing a different insight or framing
- Multiple hook options per post from genuinely different categories
- A critique note per post flagging anything to review
- A leverage ranking across the posts

Use when you already know you want LinkedIn content and don't need the full cross-platform extraction.

### `xpost`

X derivatives only. The skill decides per-insight whether to produce standalones, threads, or both:
- **Standalones** when the insight compresses into one punchy, complete thought (under 280 chars)
- **Threads** when the insight has a narrative arc or genuine multi-beat progression (3-15 posts)
- **Neither** when the insight doesn't fit X's format natively — the skill says so rather than forcing it

Each derivative includes hook options and a critique note.

### `longform`

Medium and/or Substack piece only. Produces one long-form draft (800-1500 words) with:
- Multiple hook options
- Full draft ready to edit and publish
- A critique note

If the source supports both a Medium and a Substack version (different enough to justify both), produces both. If they'd be too similar, produces one and notes why the other was skipped.

### `help`

Shows available commands with one-line descriptions and a recommended next step based on whether `repurposer_state.md` exists.

---

## Process (runs inside every command)

### Step 1 — Extract the clean argument

Strip away all platform-specific packaging (titles, hashtags, video cues, timestamps, filler). Produce a clean argument summary:

- **Thesis:** The core claim or observation. One sentence.
- **Evidence/story:** What makes the thesis concrete. Specific examples, anecdotes, data.
- **Takeaway:** What the reader should walk away thinking or questioning.

This summary is internal scaffolding. It appears in the output as a brief "Source summary" section for reference.

If working from a transcript, this step also involves:
- Removing verbal filler ("I don't know why," "I guess," "like" used as hesitation)
- Identifying improvised moments that are *better* than the scripted version (preserve these)
- Collapsing redundant explanations where the speaker said the same thing two or three ways

### Step 2 — Identify standalone insights

Pull every idea from the source that passes the **context independence test:**

> Can this insight land with a reader who has seen nothing else from this piece? Does it resonate on its own, or does it only work as part of a chain where the reader followed steps A → B → C?

For each insight that passes:
- **Core claim** in one sentence
- **What makes it standalone** (a specific detail, a story beat, a reframe, a contrarian take, a concrete example)
- **Strength rating:** Strong / Workable / Weak

For insights that *fail* the context independence test but have a strong core:
- Evaluate the **rescue path**: can the necessary scaffolding be compressed into 1-2 sentences of setup that makes the insight work independently?
- If yes, note it as "Rescued — needs compressed context from [source sections]" and include it as a candidate
- If no (the scaffolding is too heavy, the insight only works in sequence), flag it as "Skip — too dependent on [X]" and move on

**Output count is source-driven.** Some sources yield six genuinely standalone insights. Some yield two. Do not force derivatives from thin material. The default posture is to push for as many as the source genuinely supports without producing interchangeable output.

The test for "too many": if two derivatives were posted in the same week, would a reader think "didn't he just say this?" If yes, one is redundant.

### Step 3 — Assign formats

Match each insight to the format that best amplifies it. Not every insight goes on every platform. Not every platform gets output.

| Insight type | Best format |
|---|---|
| Personal story with a clear lesson | LinkedIn post (medium length), Medium piece if deep enough |
| Contrarian take or reframe | LinkedIn post (short, punchy), X standalone |
| Concrete example or surprising detail | LinkedIn post (short), X standalone |
| Multi-step framework or process | Medium/Substack post, LinkedIn carousel concept |
| Cultural observation or pattern | LinkedIn post (medium), X thread if it has enough beats |
| Narrative with multiple beats | X thread, Medium post |

If an insight doesn't have a strong format match, say so in the output. Do not stuff it into a platform where it doesn't belong.

### Step 4 — Generate hooks (multiple per derivative)

For each derivative, produce 2-4 hooks from **genuinely different categories:**

- **Value-promise:** Signals what the reader will get ("5 things I learned from...", "The framework I use for...")
- **Observation-led:** Opens with something specific the writer noticed ("I've been watching [person/trend] and something doesn't add up")
- **Contrarian / reframe:** Challenges a common belief or names an uncomfortable pattern ("Everyone says X. I think that's mostly performance.")
- **Story-entry:** Drops the reader into a moment ("Last week I was on a call with a founder who...")
- **Specificity-led:** Leads with the most concrete detail available ("40% of the posts I analyzed had the same structural problem")

Rules:
- Minimum 2 hooks per derivative, from different categories
- Do not produce hooks from categories that don't fit the insight
- Every hook must work before LinkedIn's "see more" cutoff (roughly the first 2 lines)
- No manufactured drama ("Here's what nobody tells you about..."), rhetorical escalation ("The shocking truth about..."), or importance announcements ("And then everything changed...")

The user picks the hook. The skill defaults to one only if no preference is stated.

### Step 5 — Draft derivatives

Write each derivative as a platform-native piece. Apply voice rules throughout, not just to the hook.

#### Platform-specific rules

**LinkedIn (short-form, 150-500 words):**
- Hook must land before the "see more" cutoff (first 2 lines)
- Conversational but substantive. Reader should learn or rethink something.
- End with something that invites engagement without forced engagement bait. A crisp takeaway, a genuine question, or an open loop. Not "What do you think? Drop your take below!"
- No hashtag spam. 0-3 relevant hashtags, placed naturally or at the end.

**X standalone (under 280 characters):**
- One idea, one punch. Complete thought.
- No thread-bait ("A thread on...")
- Can be an observation, a reframe, a specific detail, or a take.

**X thread (3-15 posts):**
- First post is the hook. Must work as a standalone even if nobody reads the rest.
- Each subsequent post adds something new. No filler posts, no "let me explain" bridges.
- Natural progression: story arc, escalating specificity, or building argument.
- Final post: landing, not summary. Open gap preferred over neat wrap-up.
- Number posts (1/, 2/, etc.)

**Medium (800-1500 words):**
- Opens with the most specific, concrete detail available. Not a throat-clearing intro.
- Has a clear argument arc, not a list of points.
- Tolerates more narrative, asides, and voice moments from the original delivery.
- Closes by broadening the implication, not by summarizing.
- Subheadings only if the piece is 1000+ words and genuinely benefits from visual breaks.

**Substack (800-1500 words, or shorter):**
- Assumes a subscriber relationship. Slightly more personal, less "teaching," more "continuing a conversation."
- Can be shorter than Medium. A 500-word Substack piece is fine if the idea is tight.
- Opening can be more casual. The reader already opted in.

### Step 6 — Self-critique

Before presenting output, run an internal review. Surface results as a "Critique" note attached to each derivative, plus an overall "Uncertainty notes" section at the end.

Per-derivative critique:
1. **Voice check:** Does it sound like someone talking or someone performing? If performing, rewrite internally before presenting.
2. **Em-dash scan:** Zero tolerance.
3. **Sentence stacking scan:** Three or more consecutive sentences under 12 words with no connector = rewrite.
4. **Parallel structure scan:** "X. Y. Z." with the same grammatical pattern = break it.
5. **Hook-body consistency:** Does the body deliver on the hook's promise? Does the register stay consistent throughout?
6. **Novelty check:** Would this post make someone pause, or would they scroll past because they've seen this take before?

Cross-derivative critique:
7. **Overlap check:** Read all derivatives together. Flag any pair that feels like the same post in different words. If two overlap too much, keep the stronger one and either cut or re-angle the weaker.

---

## Voice Rules (Default Profile)

This skill ships with an opinionated voice profile. These are hard constraints checked before output is presented. Modify them in this section to match your own style.

### Never produce

1. **Em-dashes.** Zero. Use commas, parentheses, colons, or restructure.
2. **Rhetorical inversions.** "It's not X, it's Y" / "X isn't about Y, it's about Z." One instance per 2000 words max.
3. **Short sentence stacking.** Three or more consecutive short sentences with no connective tissue. Connect ideas within sentences.
4. **Dramatic buildup or importance announcements.** "And then something changes..." / "Here's what nobody talks about..." as standalone beats.
5. **Three-part parallel lists for emphasis.** Two beats max.
6. **Neat quotable wrap-ups.** If a sentence could go on a motivational poster, rewrite. Endings are open gaps, not applause lines.
7. **Generic transitions.** "That said," "Here's the thing," "Let's break this down." Use natural connectors or let the next thought follow.

### Actively produce

1. **Connected sentences.** Ideas flow within and between sentences. Transitions feel like a thought continuing.
2. **Context embedded in flow.** Deliver while setting up, don't stage then deliver. ("2 months ago I was working with a solo founder in Singapore who'd been building a note-taking app" not "A solo founder reached out to me. He'd built a note-taking app.")
3. **Casual verbal connectors where natural.** "Like," "so," "and then," "it's more like." More in short-form, fewer in long-form.
4. **Specificity over abstraction.** Real numbers, real timeframes, real contexts.
5. **Natural hedging.** "I think," "roughly," "at least that's my read." Casual, not academic.
6. **Personal reactions embedded in substance.** "Which I'm pretty happy with," "kind of obvious in hindsight." Small genuine moments, not bolted-on personality.
7. **Sentence length variation.** Default shorter but not uniformly. A long connective sentence followed by a short emphatic one works. Five short sentences in a row doesn't.
8. **Genuine, conversational register throughout.** The body should feel like someone talking, not someone performing. Wit and enthusiasm are welcome. The register should stay consistent from hook through closing.

### Novelty filter

Before drafting any derivative, check: is this take actually saying something the reader hasn't encountered in this framing before? If the insight boils down to "AI is changing how we work" or "consistency matters," it's wallpaper. Either find the novel angle within it or flag it as "Skip — no novel angle."

---

## Output Format

Each command produces output in this structure (platform-specific commands show only their relevant sections):

```
## Source summary
[Thesis / Evidence / Takeaway — three lines]

## Insights identified
1. [Insight] — Strength: [Strong/Workable/Weak] — [Why standalone or how rescued]
2. [Insight] — Strength: [X] — [Note]
...

## Derivatives

### LinkedIn Post 1 — [one-line angle]

**Hook options:**
A. [Category: value-promise] [Hook text]
B. [Category: observation-led] [Hook text]
C. [Category: contrarian] [Hook text] (if applicable)

**Draft (using Hook A — swap by request):**
[Full draft, ready to copy-paste-edit-post]

*Angle: [one line on what this derivative does that the others don't]*
*Critique: [one line on what to review]*

---

### LinkedIn Post 2 — [one-line angle]
[Same structure]

---

### X — [one-line angle] [Standalone / Thread]

**Hook options:** (for threads; standalones get hook options inline)
[Options]

**Draft:**
[Full post or numbered thread]

*Angle: [one line]*
*Critique: [one line]*

---

### Medium — [working title]

**Hook options:**
[Options]

**Draft:**
[Full draft]

*Angle: [one line]*
*Critique: [one line]*

---

### Substack — [working title] (if different from Medium)
[Full draft or note on why it overlaps and was skipped]

---

## Leverage ranking
1. [Post first, why]
2. [Second, why]
...

## Uncertainty notes
[Cross-derivative flags — overlap concerns, angle questions, anything uncertain]

## Skipped
[Insights that didn't become derivatives, with reasons]
```

---

## Anti-patterns

1. **Opening any derivative with "I recently wrote about..." or "In my latest video..."** Write the insight natively.
2. **Forcing every platform to have output.** If the source supports two LinkedIn posts and a Medium draft, that's the output.
3. **Preserving spoken filler in written formats.** "I don't know why..." and "I guess..." work in video, not in posts (unless deliberately placed for voice texture).
4. **Copy-pasting the same hook at different word counts.** Each derivative needs its own entry point.
5. **Producing LinkedIn posts that read like Medium intros.** Long setup, no payoff up top.
6. **Producing Medium posts that read like stretched LinkedIn posts.** Thin, listicle-shaped, no voice.
7. **Manufacturing drama or provocation in hooks.** Hooks create genuine curiosity or promise real value. They don't manufacture urgency.
8. **Wrapping up with a clean, quotable closing line.** Endings leave an open gap, not applause.
9. **Producing generic takes.** If the derivative could have been written by anyone about any topic, it's wallpaper.
10. **Ignoring the rescue path.** Before discarding an insight that fails context independence, check if compressed context can save it.

---

## Reference Writer Mode

When the user says "use [person] as a reference" or pastes writing samples for calibration:

1. Analyze what specifically makes the referenced style work (mechanisms, not aesthetics)
2. Identify which elements translate to the current voice profile and which conflict
3. Calibrate derivatives toward those specific elements without overriding core voice rules
4. The reference is a direction signal, not a template. Voice rules always take precedence.

This is a temporary per-session calibration, not a permanent change to the skill's defaults.

---

## Iteration

This skill improves through use. When the user provides feedback on output quality:

- If a derivative is rejected, note the pattern (too generic? wrong angle? voice issue?)
- If the user consistently edits the same type of thing, that's a signal the voice rules or format rules need tightening
- If the user consistently picks the same hook category, note the preference but don't overfit (variety matters)

---

*Built by [Carl Xiong](https://carlxiong.xyz). Questions or feedback → [LinkedIn](https://linkedin.com/in/carlxiong)*
