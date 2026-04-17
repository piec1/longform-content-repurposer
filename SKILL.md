---
name: content-repurposer
description: "A Claude skill for repurposing long-form content (video scripts, transcripts, articles) into platform-native derivatives for LinkedIn, X, Medium, and Substack. Treats repurposing as an editorial judgment problem, not a formatting exercise. Includes a context independence test for insight extraction, multiple hook categories per derivative, opinionated voice rules, and built-in self-critique. Supports Claude Code (with commands) and Claude Projects (paste-and-go). Trigger when the user wants to repurpose content, turn a long piece into posts, break something down for social, or get more mileage from existing content."
---

# Content Repurposer

Turns long-form content into platform-native derivatives for LinkedIn, X, Medium, and Substack. Each derivative should feel like it was written for that platform first, not extracted from something longer.

## Why this exists

Off-the-shelf repurposing tools treat the job as a formatting exercise: take a long piece, shorten it, adjust character counts, add hashtags. The output reads like what it is, the same idea at different word counts with no editorial judgment applied.

Repurposing is actually a judgment problem: which ideas can stand alone outside the original context, which platforms amplify which types of ideas, and how to maintain a specific voice across all of them. This skill encodes that judgment.

---

## Commands (Claude Code)

When running in Claude Code, the skill uses explicit commands. In Claude Projects (paste-and-go), the `repurpose` workflow runs automatically when the user pastes source content.

### `start`

First-time setup. Walks the user through a guided interview to establish:

1. **Platforms:** Which platforms do you post on? (LinkedIn, X, Medium, Substack)
2. **Content type:** What does your source content look like? (video scripts, transcripts, written articles, a mix)
3. **Voice preferences:** The skill ships with an opinionated default voice profile (see Voice Rules below). Adopt as-is, modify, or replace entirely.
4. **Strategic context:** What's the goal of your content right now? (build authority, grow audience, support a job search, promote a product, general presence)

Saves responses to `repurposer_state.md`. Subsequent commands read this file automatically.

If `repurposer_state.md` doesn't exist when any other command runs, prompt the user to run `start` first.

### `repurpose`

The full workflow. Paste source content, get derivatives across all applicable platforms.

**Input:** Paste one or more of:
- A polished written version (script, article)
- A raw transcript (what was actually said on camera or in a recording)
- Any context about performance, intent, or strategic goals

If both written and transcript versions exist, treat the written version as the canonical argument. The transcript is a source of authentic phrasing and delivery nuance worth preserving selectively.

**Optional flags:**
- "Ask me questions first" or "interview mode" — asks 2-4 calibrating questions before generating
- "Use [person/sample] as a reference" — calibrates tone toward the referenced style while preserving core voice rules

### `linkedin`

LinkedIn derivatives only. Multiple short-form posts with different angles, hook options, and critique per post.

### `xpost`

X derivatives only. Skill decides standalones vs. threads vs. both per insight.

### `longform`

Medium and/or Substack piece only. Full draft with hook options and critique.

### `help`

Shows commands and recommended next step.

---

## Process

### Step 1 — Extract the clean argument

Strip all platform-specific packaging (titles, hashtags, video cues, timestamps, filler). Produce:

- **Thesis:** One sentence.
- **Evidence/story:** What makes it concrete.
- **Takeaway:** What the reader walks away thinking or questioning.

If working from a transcript:
- Remove verbal filler ("I don't know why," "I guess," "like" as hesitation)
- Identify improvised moments that are *better* than the scripted version (preserve these)
- Collapse redundant explanations

### Step 2 — Identify standalone insights

Every idea must pass the **context independence test:**

> Can this insight land with a reader who has seen nothing else from this piece? Does it resonate on its own, or does it only work as part of a chain?

For each insight:
- **Core claim** in one sentence
- **What makes it standalone**
- **Strength:** Strong / Workable / Weak

For insights that fail but have a strong core, evaluate the **rescue path**: can 1-2 sentences of compressed context make it work independently? If yes, include as "Rescued." If no, flag as "Skip."

**Output count is source-driven.** Push for as many as the source genuinely supports without producing interchangeable output. If two would feel like the same post in one week, one is redundant.

### Step 3 — Assign formats

| Insight type | Best format |
|---|---|
| Personal story with a clear lesson | LinkedIn post (medium), Medium piece if deep enough |
| Contrarian take or reframe | LinkedIn post (short), X standalone |
| Concrete example or surprising detail | LinkedIn post (short), X standalone |
| Multi-step framework or process | Medium/Substack, LinkedIn carousel concept |
| Cultural observation or pattern | LinkedIn post (medium), X thread if enough beats |
| Narrative with multiple beats | X thread, Medium post |

If an insight doesn't fit a platform naturally, skip that platform and say why. Never force.

### Step 4 — Generate hooks (multiple per derivative)

2-4 hooks from **genuinely different categories:**

- **Value-promise:** Signals what the reader gets
- **Observation-led:** Opens with something specific the writer noticed
- **Contrarian / reframe:** Challenges a common belief or names an uncomfortable pattern
- **Story-entry:** Drops the reader into a moment
- **Specificity-led:** Leads with the most concrete detail available

Rules:
- Minimum 2 hooks, from different categories
- Every hook must work before LinkedIn's "see more" cutoff
- No manufactured drama, rhetorical escalation, or importance announcements

### Step 5 — Draft derivatives

Write each derivative as a platform-native piece. Apply ALL voice rules (see below) throughout the entire piece.

#### Platform-specific rules

**LinkedIn (short-form, 150-500 words):**
- Hook before "see more" cutoff (first 2 lines)
- Conversational, substantive. Reader learns or rethinks something.
- End with a crisp takeaway, genuine question, or open loop. No "What do you think? Drop your take below!"
- 0-3 hashtags max.

**X standalone (under 280 characters):**
- One idea, one punch. Complete thought. No thread-bait.

**X thread (3-15 posts):**
- First post works as standalone. Each subsequent post adds something new.
- Natural progression. Final post: landing, not summary.
- Number posts (1/, 2/, etc.)

**Medium (800-1500 words):**
- Opens with the most specific, concrete detail available.
- Clear argument arc, not a list of points.
- Tolerates narrative, asides, and voice moments from the original delivery.
- Closes by broadening the implication, not summarizing.

**Substack (800-1500 words, or shorter):**
- Assumes subscriber relationship. More personal, less teaching.
- Opening can be more casual.

### Step 6 — Self-critique (HARD GATE)

**Do not present ANY derivative until every check below has been run and confirmed zero violations. If any violation is found, rewrite internally. Never present a draft that contains known violations.**

Per-derivative checks:

1. **Em-dash scan:** Search the entire draft for em-dashes (—). There must be zero. Replace every one with a comma, period, colon, or restructured sentence.

2. **"It's not X, it's Y" scan:** Search for "It's not," "It isn't," "This isn't," "That's not," "This is not" followed by a contrasting clause. Rewrite every instance. Zero tolerance. This is the single most common violation and must be caught.

3. **Short sentence scan:** Find every pair of consecutive sentences where both are under 12 words. Merge them using connectors ("and," "because," "which," "so," "where"). See Before/After Examples below for exact patterns.

4. **"Most people" scan:** Search for "most people," "nobody," "everyone," "no one" used as a setup to tell the reader what they're doing wrong. Rewrite from first-person perspective or specific observation. See Posture Rules below.

5. **Preachy posture check:** Read the entire derivative imagining you're saying it to a friend over coffee. If any sentence would make you sound like you think you're smarter than your friend, rewrite it.

6. **Parallel structure scan:** "X. Y. Z." with the same grammatical pattern = break it.

7. **Hook-body consistency:** Does the body deliver on the hook? Does the register stay consistent throughout?

8. **Novelty check:** Would this make someone pause, or is it a take they've seen before?

Cross-derivative checks:

9. **Overlap check:** Read all derivatives together. Flag any pair that feels like the same post.

10. **Angle register check:** Is any derivative primarily negative, accusatory, or ranty? If yes, reframe toward observation and personal learning, or flag it. See Angle Selection Rules below.

---

## Voice Rules (Default Profile)

These are hard constraints. Modify this section to match your own style.

### NEVER produce (zero tolerance)

**1. Em-dashes**
Never use em-dashes (—) anywhere. Use commas, colons, parentheses, or restructure.

**2. Rhetorical inversions**
Any structure that sets up and negates: "It's not X, it's Y" / "This isn't about X, it's about Y" / "That's not a bug, that's a feature." Zero tolerance. Do not produce a single instance. Before presenting ANY draft, scan for these patterns and rewrite every one found.

BAD: "It's not 'build in public' as a strategy. It's closer to this: if you'd keep posting either way..."
REWRITE: "Building in public works less as a strategy and more as a filter, where the people who'd keep posting either way are the ones something eventually catches for."

BAD: "The gym selfie wasn't a calculated move. It was a normal day of her posting."
REWRITE: "The gym selfie wasn't calculated, just a normal day of her posting, and the algorithm happened to find it funny."

**3. Short sentence stacking**
Two or more consecutive sentences under 12 words with no connective tissue = violation. Merge using "and," "because," "which," "so," "where," or restructure into one flowing sentence.

BAD: "It's pretty rigorous. She tags communities, posts daily, studies which tweet templates go viral."
REWRITE: "She ran a pretty rigorous process: tagging communities, posting daily, studying which tweet templates went viral in her space and rebuilding from the structure."

BAD: "I don't think that's bad advice. There's real value in it."
REWRITE: "I don't think that's bad advice because there's definitely real value in it."

BAD: "You hear the phrase everywhere now. Every founder, every creator, everyone trying to grow an audience."
REWRITE: "You hear it everywhere these days, especially from founders and creators who are trying to grow an audience."

BAD: "The tell is usually how clean the posts are. A real process isn't clean."
REWRITE: "The tell is usually how clean the posts are, because a real process has weird weeks and failed experiments and moments where you're not sure why the thing you shipped didn't work."

BAD: "Most people never land on a clean number. The value of taking ownership of the question is..."
REWRITE: "Most people never land on a clean number, and the value of taking ownership of the question is that the system stops quietly defining the answer for you."

BAD: "So the question just sits there unasked. And I think the avoidance is the expensive part."
REWRITE: "So the question just sits there unasked, and the avoidance actually becomes pretty expensive because every major career decision you're making is built on top of a number somebody else came up with."

**4. Dramatic buildup or importance announcements**
"And then something changes..." / "Here's what nobody talks about..." as standalone dramatic beats. Cut them.

**5. Three-part parallel lists for emphasis**
Two beats max. Not "X, Y, and Z" where Z is the dramatic punchline.

**6. Neat quotable wrap-ups**
If a sentence could go on a motivational poster, rewrite. Endings are open gaps, not applause lines.

**7. Generic transitions**
"That said," "Here's the thing," "Let's break this down," "Let's dive in." Use natural connectors or let the next thought follow.

### NEVER adopt this posture

**8. "Most people" educator framing**
Do not write from the position of someone educating the masses. Never use "most people don't realize," "nobody thinks about," "everyone gets this wrong" as a setup to tell readers what they should be doing instead. This reads as preachy, superior, and defensive-making.

BAD: "Most people never really answer the question 'is my pay about right.'"
REWRITE: "I never really answered the question 'is my pay about right' until I had to."

BAD: "Most people don't know what they're worth at work."
REWRITE: "I didn't know what I was actually worth at work until the proxy system stopped working for me."

The default voice comes from first-person observation and experience, not from diagnosing what other people are doing wrong. Write as someone sharing what they've been chewing on, not someone delivering a lesson.

**9. Preachy or superior tone**
The writer is not above the reader. Every sentence should pass the coffee test: "Would I say this to a friend without sounding like I think I'm smarter than them?" If no, rewrite.

Signals of preachy tone to catch:
- Telling the reader what they should do or think
- Diagnosing a problem that "everyone" has
- Positioning yourself as having figured something out that others haven't
- Any sentence that reads like a keynote speech or self-help book

The fix is almost always the same: shift from "here's what people get wrong" to "here's what I noticed" or "here's what happened to me."

### ALWAYS produce

**1. Connected, flowing sentences**
Ideas flow within and between sentences. Transitions feel like a thought continuing, not a new chapter starting. Use commas, "and," "because," "so," "which," "where" to connect related thoughts into single flowing sentences instead of chopping them into fragments.

**2. Context embedded in flow**
Deliver while setting up. Don't stage information in one sentence, then deliver in the next.

GOOD: "She ran a pretty rigorous process: tagging communities, posting daily, studying which tweet templates went viral in her space."
BAD: "It's pretty rigorous. She tags communities, posts daily, studies which tweet templates go viral."

**3. Casual verbal connectors**
"Like," "so," "and then," "it's more like," "which is basically." More in short-form, fewer in long-form. These are natural speech patterns.

**4. Specificity over abstraction**
Real numbers, real timeframes, real contexts. "2 months ago" not "recently." "About 2,000 followers" not "a growing audience."

**5. Natural hedging**
"I think," "roughly," "at least that's my read," "something like," "about." Casual, not academic. Makes it feel like telling someone over coffee rather than presenting a deck.

**6. Personal reactions embedded in substance**
"Which I'm pretty happy with," "kind of obvious in hindsight," "which I think is kind of funny." Small genuine moments, not bolted-on personality. React before analyzing when the reaction is genuine.

**7. Sentence length variation**
Default shorter but not uniformly. A long connective sentence followed by a short emphatic one works. Five short sentences in a row never works.

**8. Genuine, conversational register throughout**
The body should feel like someone talking to a friend, not someone performing for an audience. Wit and enthusiasm are welcome. The register should stay consistent from hook through closing. No switching from casual personality to formal analysis mid-piece.

**9. "I just noticed this" energy**
Share observations in near-real-time rather than from a position of manufactured authority. "I've been watching this happen" > "Here's the truth about X." The vibe is sharing something you found interesting, not delivering a verdict.

**10. Let the artifact speak**
Don't over-explain why something is interesting. State the observation, give the specific detail, and let the reader draw the implication. If you're writing "and I think that's really important because..." consider cutting everything after "because."

### Novelty filter

Before drafting any derivative: is this take saying something the reader hasn't encountered in this framing? If the insight boils down to "AI is changing how we work" or "consistency matters," it's wallpaper. Find the novel angle or flag as "Skip."

---

## Angle Selection Rules

Before finalizing any derivative's angle, run these filters:

**Negativity filter:** Is this post primarily calling something out, accusing a group of people, or ranting about a problem? If yes, reframe. The default posture is curiosity and observation ("here's something I noticed, and I'm not sure what to do with it") rather than accusation ("here's what people are doing wrong").

A post that says "building in public has become performative and people are being fake" is ranty. A post that says "the post that blew up for her was the one she didn't plan, which is kind of funny when you think about how much effort goes into the planned ones" is observational. Same insight, different posture.

**Value filter:** Each derivative should leave the reader with something: a reframe they can apply, a question worth sitting with, a specific detail they'll remember, or a story that makes them think. "Here's what I think other people are doing wrong" is not value. "Here's something I noticed that made me rethink my own approach" is value.

**Intrigue filter:** Would you click on this? Would you keep reading past the hook? If the angle is accurate but boring, find the entry point that makes someone curious. The Caitlyn gym selfie story is more intriguing than "building in public is becoming performative" even though both lead to the same argument.

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
[Full post or numbered thread with hook options and critique]

---

### Medium — [working title]
[Full draft with hook options and critique]

---

### Substack — [working title] (if different from Medium)
[Full draft or note on why it overlaps and was skipped]

---

## Leverage ranking
1. [Post first, why]
2. [Second, why]
...

## Uncertainty notes
[Cross-derivative flags — overlap, angle questions, anything uncertain]

## Skipped
[Insights that didn't become derivatives, with reasons]
```

---

## Anti-patterns

1. **Opening with "I recently wrote about..." or "In my latest video..."** Write the insight natively.
2. **Forcing every platform to have output.** If the source supports two LinkedIn posts and a Medium draft, that's it.
3. **Preserving spoken filler in written formats.** Unless deliberately placed for voice texture.
4. **Copy-pasting the same hook at different word counts.**
5. **LinkedIn posts that read like Medium intros.** Long setup, no payoff up top.
6. **Medium posts that read like stretched LinkedIn posts.** Thin, listicle-shaped, no voice.
7. **Manufacturing drama or provocation in hooks.**
8. **Clean, quotable closing lines.** Endings leave an open gap.
9. **Generic takes.** If it could have been written by anyone, it's wallpaper.
10. **Ignoring the rescue path.** Check if compressed context can save a dependent insight before discarding.
11. **"Most people" framing.** Write from first-person observation, not mass diagnosis.
12. **Ranty or accusatory angles.** Observe and share, don't accuse and lecture.

---

## Reference Writer Mode

When the user says "use [person] as a reference" or pastes writing samples:

1. Analyze what specifically makes the referenced style work (mechanisms, not aesthetics)
2. Identify which elements translate to the current voice profile and which conflict
3. Calibrate derivatives toward those elements without overriding core voice rules
4. The reference is a direction signal, not a template

Temporary per-session calibration, not a permanent change.

---

## Iteration

This skill improves through use. When the user provides feedback:

- If a derivative is rejected, note the pattern (too generic? wrong angle? voice issue? preachy?)
- If the user consistently edits the same type of thing, tighten the relevant voice rule
- If the user consistently picks the same hook category, note the preference but don't overfit

---

*Built by [Carl Xiong](https://carlxiong.xyz). Questions or feedback → [LinkedIn](https://linkedin.com/in/carlxiong)*
