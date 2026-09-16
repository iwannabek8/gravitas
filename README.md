# Gravitas

A design skill for building cinematic HTML reels — 30-second looping camera-move sequences over stylized CSS/SVG scenes, with cinematographer-grade taste baked into the instructions.

Named for what the pieces are supposed to do: give digital things weight.

## What it makes

A single HTML file. Drop it anywhere — email attachment, presentation cold-open, Slack, portfolio site, iframe. Renders on any browser. Loops seamlessly. No player, no cloud, no login, no MP4.

Every reel is:
- 24–36 seconds, looping
- 3–6 shots (variable — the skill will cut shots that don't earn their duration)
- One subject on a designed territory, with two motivated light sources
- Canvas particle weather (rain, dust, snow, embers)
- Web Audio ambient bed (clean-harmonic drone + soft hiss, click-to-start)
- Editorial title card + credits, letterbox, static grain

## Why not just use Higgsfield / Runway / Sora?

You should — when you need photorealism or a client-facing "video."

Use Gravitas when you need:
- **Speed & iteration.** 30 seconds from prompt to loop. Tweak a number, refresh. No re-render, no credits, no waiting.
- **Determinism.** Every decision is code you can edit. Real generative video is a lottery.
- **Portability.** One HTML file, tens of KB, no dependencies. Works offline.
- **Seamless loops.** Video files always have a loop point. CSS animation is mathematically continuous.
- **No third-party model in the pipeline.** Just your code, your rights.
- **Expressive over generative.** Gravitas conveys *your* interpretation of the mood — not a model's.

Higgsfield is a camera. Gravitas is a sketchbook.

## How to use it

Gravitas is a **prompt-authored skill** — a set of instructions you give to a capable AI coding assistant (Claude, GPT, etc.). It doesn't run itself; it teaches the AI to build reels *your way*.

1. Copy the contents of [`SKILL.md`](./SKILL.md) into your AI assistant as a system prompt, custom instruction, or skill definition.
2. Ask for a reel: *"Build me a Gravitas reel: a lone samurai mid-draw in a bamboo grove at storm. Mood: tense."*
3. The AI will produce a single HTML file. Open it in a browser.
4. Iterate: *"Hold shot 3 for two more seconds."* / *"Regrade the CU cooler."* / *"Cut the crash zoom — it isn't earning it."*

## Which model to run it under

Gravitas is model-sensitive — it asks the assistant to make a lot of small taste calls (composition, palette, shot motivation, when to *cut* a shot). Not every model handles that well. In descending order of what I've actually shipped good reels on:

| # | Model | Why |
|---|---|---|
| 1 | **Claude Opus 4.8** | Taste ceiling. Best at SVG anatomy — contrapposto poses, weight distribution, silhouette-that-reads. Slowest, but the reel that comes out is the one you'd share. |
| 2 | **GPT-6 Astra @ high or xhigh** | Best all-around. Best speed/quality ratio for this skill. Where I'd default to if I were shipping reels regularly. |
| 3 | **Claude Sonnet 5 @ xhigh** | Very close to Opus on taste, meaningfully faster. Solid choice when iterating quickly. |
| 4 | **GPT-5.6 Sol @ high** | Reliable, competent. Slightly less audacious with camera + palette but nails the structural template. |
| 5 | **GPT-5.3-Codex @ high** | Execution-only. Use for surgical edits to an existing reel (retune timing, swap a color, rewrite the audio block). Don't ask it to design a piece from scratch — it'll produce a technically correct but taste-flat reel. |

If a model isn't on this list, expect the skill's principles to get flattened into checklist compliance rather than a felt design decision.

## Examples

- [`examples/01-tokyo-alley-detective.html`](./examples/01-tokyo-alley-detective.html) — cyberpunk detective in a rain-soaked Tokyo alley. Cold indigo dominant + sodium amber key + one small distant neon. Rain catches the sodium beam.
- [`examples/02-iowa-autumn-cornfield.html`](./examples/02-iowa-autumn-cornfield.html) — *The way home.* An Iowa cornfield in the fall, held in memory.
- [`examples/03-ice-castle-snow-queen.html`](./examples/03-ice-castle-snow-queen.html) — *The Winter I Refused.* A snow queen reel: the first thing to thaw was fear.

Open any in a browser. Click the "sound" button in the corner to start the audio.

## The design principles (short version)

The full ruleset is in [`SKILL.md`](./SKILL.md). The taste bar in one page:

1. Every camera move must be motivated — no move exists because it's on a shot list.
2. **Cutting a shot is a stronger design move than adding one.** Holds > orbits.
3. Composition is asymmetric by default — subjects go on thirds, not center.
4. Two light sources minimum. One warm, one cool.
5. Color is composed with a temperature axis and a 60/30/10 dominance ratio.
6. Subjects must live even when still — breath loops + one detail loop.
7. Rain must be canvas particles, not CSS gradient slats.
8. Web Audio: no detuned unison beating, no bandpass hiss. Clean intervals, lowpass, master gain ≤ 0.15.
9. Every element earns its place. Frame density is a taste failure.
10. Chrome (HUDs, REC lights) is default off. Earn it or kill it.

## License

MIT — see [`LICENSE`](./LICENSE).

## Credits

Skill authored by Kate Steele. Contributions and forks welcome.
