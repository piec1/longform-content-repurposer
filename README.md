# Content Repurposer

A Claude skill that repurposes long-form content into platform-native derivatives for LinkedIn, X, Medium, and Substack. Editorial judgment, not formatting.

## What it does

Give it a video script, transcript, or article, and it:

- **Finds the standalone ideas** — runs a context independence test on every insight to check if it lands without the surrounding piece, and tries a rescue path for ideas that are strong but need a sentence or two of setup
- **Matches ideas to platforms** — a contrarian reframe might work as an X standalone while a multi-beat story belongs in a Medium piece. The skill decides per-insight rather than producing the same thing at different word counts
- **Generates multiple hooks per post** — 2-4 hooks from genuinely different categories (story-entry, contrarian, specificity-led, etc.) so you pick the angle, not the AI
- **Writes in your voice** — ships with an opinionated default voice profile that rejects common AI patterns (short sentence stacking, em-dashes, "it's not X it's Y" constructions, preachy "most people" framing) and enforces conversational, connected prose
- **Critiques its own output** — every derivative comes with a note on what might be off (voice issues, overlap with other posts, novelty concerns) so you know where to focus your editing
- **Skips what doesn't fit** — if the source only supports two LinkedIn posts and a Medium draft, that's the output. Platforms are never padded.

## Example prompts

```
repurpose
[paste a 2000-word video script]
```

```
linkedin
[paste a transcript — just want LinkedIn posts from this one]
```

```
longform
[paste a script — just want the Medium/Substack version]
```

```
Ask me questions first
[paste content — skill interviews you about goals before generating]
```

```
Use certain writing samples as reference
[paste content + paste 2-3 of writing samples — skill calibrates toward his style while keeping your voice rules]
```

## Commands

| Command | What it does |
|---|---|
| `start` | First-time setup. Guided interview for platforms, content type, voice, goals. Saves preferences for future runs. |
| `repurpose` | Full workflow. Paste source, get derivatives across all platforms with hooks and critique. |
| `linkedin` | LinkedIn posts only. Multiple angles, hook options, critique per post. |
| `xpost` | X only. Decides standalones vs. threads vs. both per insight. |
| `longform` | Medium/Substack only. Full draft with hooks and critique. |
| `help` | Show commands and recommended next step. |

In Claude Projects (no commands), paste your content and the skill runs automatically.

## Quick start

### Claude Code

```bash
git clone https://github.com/carlxiong/content-repurposer.git
cd content-repurposer
mv SKILL.md CLAUDE.md
```

Open the folder in Claude Code and type `start` to begin setup, or `repurpose` and paste your content.

### Claude Project

1. Create a new Claude Project or open an existing one
2. Copy the contents of `SKILL.md` into the project instructions
3. Paste your content into the chat

## How it's different from generic repurposers

**It has opinions about what good writing sounds like.** Most repurposers change word count and add hashtags. This one rejects short sentence stacking, preachy "most people" framing, rhetorical inversions, manufactured drama hooks, and neat quotable wrap-ups. It enforces connected, flowing sentences with a conversational register. The voice rules are modifiable if your style is different.

**It decides how many derivatives to make, not you.** Some sources have six standalone insights. Some have two. The skill pushes for as many as the source supports and stops before producing interchangeable posts.

**It skips platforms that don't fit.** If the source doesn't have enough beats for an X thread, it says so instead of forcing a thin thread. Not every insight goes everywhere.

**It tells you what might be wrong.** Every derivative comes with a critique note. The overall output includes uncertainty flags on overlap between posts, novelty concerns, and angle issues. You know where to spend your editing time.

## Output platforms

- **LinkedIn** — short-form posts (150-500 words)
- **X** — standalone posts and/or threads, chosen per insight
- **Medium** — long-form (800-1500 words)
- **Substack** — long-form adapted for subscriber relationship

## Limitations

- **Voice rules are opinionated.** Default profile reflects one creator's style. Modify the Voice Rules section in SKILL.md for your own.
- **Requires good source material.** Weak sources get honest "skip" recommendations, not dressed-up filler.
- **No auto-posting.** Output is text to copy and paste. Always preview on the platform before publishing.
- **English output only.** Can process source material with non-English elements, but derivatives are English.
- **Single-user focus.** No team workflows or approval chains.

## License

MIT

---

*Built by [Carl Xiong](https://carlxiong.xyz). Questions, feedback, or want to chat about content systems → [LinkedIn](https://linkedin.com/in/carlxiong)*
