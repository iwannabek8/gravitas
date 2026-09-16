# Gravitas

Generate a single-file HTML cinematic reel with the taste bar of a real cinematographer + motion designer, not a "pretty things coder." The output must earn every element on screen.

Gravitas is named for what it's supposed to do: give digital things weight.

## Design principles (non-negotiable)

These override any temptation to just ship features. If a decision violates one of these, redo it.

1. **Every camera move must be motivated.** No move exists because it's on the shot list. Each shot answers: *what is this move revealing about the subject or world that the previous shot didn't?* Write the motivation for each shot as a code comment before you write its keyframes.

2. **Movement discipline: cutting a shot is a stronger design move than adding one.** If a shot doesn't earn its duration — if the camera is moving but nothing in the world or subject is *changing* during that move — kill the shot. Orbits, whip pans, and dolly moves only work when something is happening beneath them. Otherwise it's panning for panning's sake. **Holds are stronger than orbits.** Trust stillness. A 6–8s hold with only weather, breath, and light flicker will out-drama any bullet-time orbit on a static subject.

3. **Variable shot count.** Do NOT force 6 shots. The reel can be 3, 4, 5, or 6 shots depending on what the piece needs. Total loop 24–36s. Fewer, longer, earned shots > more, shorter, arbitrary ones.

4. **Dutch tilts are locked or absent — never drifting.** A slow drift from 0° to -4° over 5 seconds reads as indecisive. Either the shot IS tilted (frame 1 rotation, held) or it isn't. Same for hue-rotates, saturation drifts, and other slow parameter changes: commit, don't drift.

5. **Composition is asymmetric by default.** Subject is never dead-center unless the piece is explicitly about symmetry (ritual, confrontation, sacred). Use thirds, weighted asymmetry, or intentional negative space.

6. **One dominant, one subordinate, one accent.** In every shot, the eye should land somewhere first, drift somewhere second, and register a third detail. If everything glows equally, nothing reads. Dim, obscure, or de-saturate the non-dominant elements.

7. **Color is composed, not applied.** Pick a palette with a *temperature axis* (warmest point in frame, coolest point in frame) and a *dominance ratio* (roughly 60/30/10). Name the warmest light source in the scene — that's where the eye goes.

8. **Two light sources minimum.** A single practical light in a dark scene crushes exposure to unreadable. Always add at least one secondary practical (different temperature, different position) to sculpt the subject and lift shadow readability.

9. **Depth via atmospheric perspective, not just blur.** Distant elements: lower contrast, cooler temperature, lower saturation, softer edges. Foreground: higher contrast, warmer if lit, sharper. Parallax speeds must match perceived depth.

10. **Grade shifts per shot** — but only via cheap filters. Contrast, saturate, brightness only. **Never `blur()` or `hue-rotate()` in an animated filter on the camera rig** — both are GPU-expensive and cause visible chop when interpolated on a scaled/rotated container.

11. **The subject must live even when still.** Every subject gets at least two micro-motions: (a) a breath/sway loop on the whole silhouette (subtle translateY or skew, 3–5s cycle), and (b) one detail loop (blink, ember pulse, hair drift, tail flick, fabric ripple, weapon glint). Micro-motion never syncs to camera — it lives on its own clock.

12. **Timing is rhythmic, not metronomic.** No even-slot division. Write out shot lengths in seconds first: some want 2–3s (crashes), some want 6–8s (holds, pulls). Total 24–36s.

13. **Every element earns its place.** If you can't say what a prop, sign, or particle contributes to mood or narrative in one sentence, delete it. Frame density is a taste failure, not a feature.

14. **Sound is half the piece.** Add a Web Audio ambient bed. Rules:
    - **No detuned unison beating** in the drone (e.g. 220Hz + 220.7Hz) — fatiguing over 30s+. Use clean harmonic intervals only: octaves, perfect fifths, perfect fourths.
    - **Weather hiss must be lowpass, not bandpass.** Bandpass at 2–4kHz is harsh and painful. Lowpass at 700–1000Hz is soft ambient.
    - **Master gain 0.10–0.15**, not 0.30+. Fade in over 3–4s from silence.
    - One-shot punches only on shots that genuinely need punctuation (pull-back sub swell, crash-zoom hit). If you cut a shot, cut its one-shot too.
    - Always click-to-start (autoplay policies).

## Performance guardrails (kills the choppy render)

- **No animated grain overlay** — static grain only. The `steps(4)` shift is invisible and costs frames.
- **No gate weave / rig jitter animations** — same reason.
- **No `blur()` or `hue-rotate()` in the `.rig`'s animated filter.** Only `contrast()`, `saturate()`, `brightness()`. These are cheap.
- **`will-change: transform, filter` on the `.rig`.**
- **Perspective rotations on walls are static, not animated.**

## Weather must be canvas particles, not CSS gradient slats

CSS repeating-linear-gradient rain reads as diagonal slats, not weather. Always use a `<canvas>` particle system with:
- Particles varied by depth (0 = far, 1 = near). Longer, faster, thicker, brighter for near.
- Particles within a defined light source radius get warm-tinted and brightened.
- Ground effects (rain splashes, ember settle, dust settle) where particles reach the bottom third of frame.
- 120–200 particles total. 60fps rAF loop.

Applies equally to rain, snow, embers, dust motes, floating spores, etc. — pick the particle behavior that fits the territory.

## Anatomy of a real subject SVG

Silhouette-with-two-rim-strokes is the floor, not the ceiling.

- **Pose reads weight.** Contrapposto: weight on one leg, shoulders counter-tilted to hips. Or a clear gesture (reaching, drawing, listening). Never symmetrical A-pose unless intentional.
- **Silhouette test.** If you removed all interior detail and rim light, could you still identify the character from the silhouette alone? If not, redo the pose.
- **Anatomical anchor points.** Head tilt, shoulder line, hip line, weight-bearing foot. Even in stylized form, these must be consistent.
- **Rim light has a source.** Two rim strokes should be motivated by two named light sources in the scene, not just "left color right color."
- **Micro-motion is anatomically plausible.** Chest expands on breath, not head. Fabric hem ripples, not the whole coat.

## Composition rules per shot type

- **Establishing:** subject small in frame (bottom third or off-center), world dominant. Depth cues front to back.
- **Crash zoom:** subject reframed to rule-of-thirds intersection on landing, not center.
- **Hold (default choice for the mid-piece):** no camera motion. Held wide-medium on the subject. Life comes from weather, subject micro-motion, and light flicker. This is usually the strongest shot in the reel — do not skip it.
- **Bullet time / orbit:** ONLY include if the subject or world is doing something extraordinary during the orbit. Otherwise cut it.
- **Whip pan:** ONLY include if it lands on a specific new element and stays there. Whip pans back to the subject read as tics.
- **Low-angle CU:** if using dutch tilt, LOCK the tilt at frame 1. No drift. Rim light silhouettes a specific anatomical feature.
- **Pull-back:** the reveal must add information. New context, new depth, or new element that wasn't in the wide.

## Exposure floor

Vignette max: `inset 0 0 160px rgba(0,0,0,0.55)`. Anything darker crushes readability.
Baseline brightness in shot filters: `brightness(1.0)` or higher. Never below 0.95.

## Typography

Pick ONE display face per piece, from system stacks. Editorial serif for literary/human. Grotesque sans for clinical. Mono for diegetic HUDs only. Never mix more than two.

## HUD / chrome — earn it or kill it

Default off. Only include if diegetic (represents in-story surveillance/reticle), editorial (title card at open + credits at close, no persistent chrome), or intentional camp. Letterbox always stays with soft inner vignette.

## Structural template (build the file to this shape)

Single HTML file. `<style>` in `<head>`. One `<canvas id="weather">` for particles; everything else CSS/SVG.

Layer stack inside a `.rig` container that owns the `shotSequence` @keyframes:

1. `.sky` — deep radial gradients, sets ambient mood
2. `.far-silhouette` — distant backdrop SVG, blurred + desaturated for atmospheric perspective
3. Prop/signage divs — positioned absolute, self-flickering, sparse
4. `.set` container — perspective-warped walls/columns/arches/dunes converging toward center
5. `.shaft` + secondary practical — soft blurred gradient beams, `mix-blend-mode: screen`
6. `.key-light` — the warmest point in frame; box-shadow layered glows
7. `.ground` + `.specular` — highlight where the warm source catches the surface
8. `.fog` (or dust/haze equivalent) — slow horizontal drift
9. `.figure-far` — optional secondary story element
10. `.subject` — silhouette SVG with breath animation on the parent (translateY + skew)
11. `<canvas id="weather">` — particle rain/snow/embers/dust

Outside `.rig`:

- `.vignette` — subtle inset shadow that integrates letterbox with frame
- `.grain` — static SVG turbulence noise, `mix-blend-mode: overlay`
- `.bar.top` + `.bar.bot` — 12vh letterbox
- `.title-card` — editorial fade-in at open (~3–10% of loop)
- `.credits` — editorial fade-in at close (~95–99% of loop)
- `.audio-toggle` — button to start Web Audio

`.rig` has `will-change: transform, filter` and animates `shotSequence` on a 24–36s infinite loop. Filter transitions only use `contrast()`, `saturate()`, `brightness()`.

## Web Audio pattern

Click-to-start button. On click:
- Create AudioContext
- Master GainNode, fade from 0 → 0.14 over 3–4s
- **Drone:** two sine oscillators at clean-harmonic intervals (e.g. 55Hz root + 82.5Hz perfect fifth). Feed through a lowpass biquad (freq ~300Hz, Q 0.7). Optional very slow filter sweep (12s ramps).
- **Weather hiss:** looping white-noise buffer through a lowpass biquad (freq ~900Hz, Q 0.5), gain ~0.035.
- **Shot one-shots:** schedule with `setTimeout` per 24–36s loop. Sub swell for pull-back reveal (60→40Hz glide, gain up to 0.18, 5s envelope). Skip whip/crash one-shots unless the shot actually earns them.
- Toggle button suspends/resumes the AudioContext.

## Mandatory self-critique loop

After building, before showing the user:

1. Open the file in a browser (`Start-Process` on Windows, `open` on macOS, `xdg-open` on Linux).
2. Take a screenshot or headless-browser snapshot at 2 key moments (first shot land, mid-hold).
3. Grade honestly against principles: pass / partial / fail. Hostile critic mode.
4. Identify the **two weakest failures** and fix them before shipping.
5. **Common failures to check specifically:**
   - Too dark? (Overcorrected vignette + brightness — lift them.)
   - Choppy? (Something is animating that shouldn't be. Kill grain shift, gate weave, or filter blur/hue-rotate.)
   - Weather looks like slats? (Not using canvas particles.)
   - Sound painful? (Detuned beating, bandpass at 2–4kHz, master gain too high.)
   - Any shot where the camera moves but nothing changes underneath? (Cut it.)
6. Present with a note on what critique caught and how you fixed it.

## Inputs to collect

If not provided, ask the user:

1. **Subject** — with enough character to imply a pose ("lone samurai mid-draw", not "samurai")
2. **Territory** — user's own words; no menu, no picklist
3. **Mood** — one word (tense, sacred, grieving, feral, euphoric, dissociated)
4. **Director credit** — the user's own name for the closing card

## Build order

1. **Design brief** (write out before coding):
   - Palette with temperature axis + dominance ratio
   - Two named light sources (warm + cool, or key + fill)
   - Subject pose sentence (weight, gesture, gaze direction)
   - 5-slot territory (sky, silhouette, props, weather, ground)
   - **Shot list with timings in seconds AND per-shot motivation.** Prefer 4–5 shots over 6. Include a HOLD.
   - Typography choice
   - Chrome decision
   - Audio bed concept (clean-harmonic drone frequencies + lowpass hiss)
2. **Build the HTML** honoring the brief and the structural template above.
3. **Run the self-critique loop** — snapshot, grade, fix the two worst issues.
4. **Ship** with filename `gravitas-{territory-slug}-{subject-slug}.html`, open in browser, report back with:
   - Shot table (timing + motivation)
   - Self-critique findings + fixes
   - Two iteration hooks

## Iteration hooks to offer

- Cut or extend a shot's duration
- Rework the subject pose (redraw the SVG with different gesture)
- Batch mode: same subject through 3 mood variants as a triptych

## Constraints

- Single file HTML, no CDN, no external assets.
- Web Audio generated on the fly (oscillators, biquad filters, noise buffers), never MP3.
- Canvas for weather, CSS/SVG for everything else. JS for canvas particles + audio + interaction.
- Never suggest a menu of territories or subjects. Offer ONE invented seed inline if the user is stuck.
