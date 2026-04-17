# Content Repurposer

A Claude skill that turns long-form content into platform-native derivatives for LinkedIn, X, Medium, and Substack. Treats repurposing as an editorial judgment problem — deciding which ideas stand alone, which platforms amplify them, and how to preserve voice — instead of just reformatting at different word counts.

## The problem

Most repurposing tools take a long piece, shorten it per platform, adjust character counts, and add hashtags. The result is the same idea at three different lengths with no editorial thinking applied. The output sounds generic because the tool has no opinion about what makes a good post.

The actual work of repurposing is judgment: can this insight stand on its own without the surrounding context? Is this a LinkedIn post or a thread? Should this even be repurposed, or is it too thin? Does the output sound like a person or like a template?

This skill encodes that judgment.

## What it does

**Input:** A video script, transcript, written article, or any long-form content.

**Process:**
1. Extracts the clean argument from the source (stripping filler, redundancy, platform-specific packaging)
2. Identifies standalone insights using a **context independence test** — each insight must land with a reader who's seen nothing else from the piece
3. Evaluates a **rescue path** for insights that fail the independence test but have a strong core (can compressed context save them?)
4. Matches each insight to the platform and format that best amplifies it
5. Generates **multiple hooks per derivative** from genuinely different categories (value-promise, observation-led, contrarian, story-entry, specificity-led)
6. Drafts each derivative with embedded voice rules and runs a **self-critique** before output

**Output:** Platform-native derivatives with hook options, per-derivative critique notes, a leverage ranking for posting order, and flags on anything uncertain.

## Key design choices

**Source-driven output count.** No fixed number of derivatives. The skill produces as many as the source genuinely supports — could be two, could be six. Forcing more from thin material produces interchangeable posts.

**Context independence test with rescue path.** Instead of discarding insights that don't stand alone, the skill checks whether 1-2 sentences of compressed context can save them. This preserves strong ideas that happen to be dependent, which most tools either force (weak output) or drop entirely.

**Multiple hook categories per derivative.** Each post gets 2-4 hooks from genuinely different angles, not three variations of the same approach. The user picks.

**Opinionated voice rules.** The skill ships with a default voice profile that rejects common AI writing patterns (em-dashes, rhetorical inversions, sentence stacking, dramatic buildups, quotable wrap-ups) and enforces connected sentences, specificity, natural hedging, and consistent register. Voice rules are modifiable — swap in your own style.

**Per-derivative critique.** Every derivative comes with a critique note flagging what to review (voice issues, hook-body inconsistency, novelty concerns). The user gets a pre-filtered list of where to focus editing attention instead of reading everything from scratch.

**No forcing.** If an insight doesn't fit X, the skill skips X for that insight and says why. If the source only supports LinkedIn and Medium, those are the only outputs. Platforms are never padded.

## Commands (Claude Code)

| Command | What it does |
|---|---|
| `start` | First-time setup. Guided interview for platforms, content type, voice preferences, and strategic goals. Saves to `repurposer_state.md` for persistence. |
| `repurpose` | Full workflow. Paste source content, get derivatives across all applicable platforms with hooks and critique. |
| `linkedin` | LinkedIn derivatives only. Multiple posts with different angles, hook options, and critique per post. |
| `xpost` | X derivatives only. Skill decides standalones vs. threads vs. both per insight. |
| `longform` | Medium/Substack piece only. Full draft with hook options and critique. |
| `help` | Show commands and recommended next step. |

In Claude Projects (no commands), the `repurpose` workflow runs automatically when you paste source content.

## Quick start

### Claude Code (recommended for the full command experience)

```bash
git clone https://github.com/carlxiong/content-repurposer.git
cd content-repurposer
mv SKILL.md CLAUDE.md
```

Open the folder in Claude Code and type `start` to begin setup, or `repurpose` and paste your content.

### Claude Project (paste-and-go)

1. Create a new Claude Project or open an existing one
2. Copy the contents of `SKILL.md` into the project's instructions
3. Paste your long-form content into the chat
4. The skill generates derivatives automatically

## Example

**Input:** 2000-word video script about building in public as performative behavior

**Output shape:**
- 3 LinkedIn posts (the performative pattern observation, the gym-selfie irony, the "would you post even if nobody watched" filter), each with 2-3 hook options and a critique note
- 1 X standalone (the contrarian reframe, compressed to 280 chars)
- 1 Medium piece (the full argument with a specific story as the anchor)
- Skipped: X thread (the source doesn't have enough distinct beats for genuine thread progression)
- Leverage ranking: LinkedIn post with the most concrete detail first, Medium piece second, remaining LinkedIn posts spread across the week

*This shows the shape of the output, not actual generated content.*

## Output platforms

- **LinkedIn** — short-form posts (150-500 words) with hook options
- **X** — standalone posts (under 280 chars) and/or threads (3-15 posts), chosen per insight
- **Medium** — long-form pieces (800-1500 words) with narrative arcs
- **Substack** — long-form or shorter, adapted for a subscriber relationship

Not every platform gets output every time. Format follows the insight.

## Limitations

- **Voice rules are opinionated.** The default profile reflects one creator's style. Modify the Voice Rules section in `SKILL.md` to match your own.
- **Requires good source material.** The skill surfaces insights that exist in the source. Weak source content produces honest "skip" recommendations, not dressed-up filler.
- **No auto-posting.** Output is text you copy and paste. Always preview on the platform before publishing — formatting between what Claude produces and what platforms render can differ.
- **English output only.** The skill can process source material with non-English elements (titles, tags, packaging) but all derivatives are in English.
- **Single-user focus.** Built for one person repurposing their own content. No team workflows or approval chains.

## License

MIT

---

*Built by [Carl Xiong](https://carlxiong.xyz). Questions, feedback, or want to chat about content systems → [LinkedIn](https://linkedin.com/in/carlxiong)*
