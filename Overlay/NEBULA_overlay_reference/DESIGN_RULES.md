# NEBULA overlay — reference v2

Everything decided on 2026-11-14. These files are the **reference** — you write the real build yourself.

## What's in here

| Folder | What it is |
|---|---|
| `previews/` | Self-contained versions (fonts embedded). Open anywhere to watch the screen run. |
| `source/` | Same pages, readable. Fonts load from `../assets/fonts/`, same layout as your `build/` folder. **Read these, not the previews.** |
| `assets/` | Fonts (4), the v2 stinger webm, the stinger SFX. |
| `images/` | Stills of each screen, the wordmark, the gameplay preview. |

`source/stinger_v2_render.html` is the page the v2 stinger was rendered from (only needed if you re-render it).

## Palette

| Variable | Value | Job |
|---|---|---|
| `--base` | `#0A0D0B` | background |
| `--accent` | `#39FF88` | **green: structure.** Borders, frames, main lines. The main color. |
| `--accent-light` | `hsl(145, 100%, 80%)` | NEBULA tag square, gameplay moving hatch |
| `--purple` | `hsl(286, 90%, 59%)` | **details:** corner plates, corner brackets, text undertone, LIVE square |
| `--yellow` | `hsl(54, 100%, 55%)` | **markers:** `//` tag lines, squares, chain lines, callouts, divider diamond |
| `--white` | `#FFFFFF` | resting: titles and handles (always with an undertone) |
| `--text` / `--muted` | `#E8F5EC` / `#9FB8A8` | body / secondary text |

Gold is gone. Don't add colors without giving them a job no other color has.

## Text rules

- **Every white text gets a purple undertone:** `class="wt"` → `color: var(--white); text-shadow: .07em .07em 0 var(--purple);` (em so it scales with font size).
- Big titles use a hard `7px 7px 0 var(--purple)` shadow.
- **NEBULA wordmark** is the one exception: Iceberg, **green** undertone, with a 35° slash cut across the whole N (built as an SVG mask so the cut is see-through). See `brandMark()` in any source file.
- Fonts: Orbitron 800 (big titles), Orbitron 600, JetBrains Mono 500 (everything else), Iceberg 400 (NEBULA only). All OFL.

## Motion rules

- **Easing:** `--ease-out: cubic-bezier(.25,.6,.35,1)` for things **arriving**, `--ease-in: cubic-bezier(.6,0,.8,.4)` for things **leaving**. Both stay inside 0..1 → **no recoil / overshoot.**
- **Glitches and typing:** `steps()` — hard cuts, never smooth.
- **Squares are a 4-step drill, one step at a time:** zoom in (`--zoom-in: 350ms`) → label types out → label slides left into the box → box zooms out (`--zoom-out: 200ms`). JS reads the CSS vars so the text starts the instant the zoom ends.
- **Lines:** straight, with 1–2 sharp 90° turns max (chain). Callout leader lines use one 45° bend (matches the shard reference).
- **Gameplay:** the only thing that moves is the light-green hatch inside the vitals plate.

## Layering

- **Titles and the brand go ABOVE the scanlines + vignette** (otherwise they render gray). Background decoration stays under.
- On Starting Soon the title is still under the scanlines (faint stripes) — your call whether to lift it.

## Screens

| Screen | Border label | Background systems |
|---|---|---|
| Starting Soon | `STANDBY // 01` | single squares (5–9s, max 2), bottom chain (18–28s), terminal top-left (30–45s), callout top-right (25–40s) |
| Loading | `ON AIR // 03` | single squares only, header gap + bottom strip (8–14s, max 1) |
| BRB | `PAUSED // 02` | single squares, bottom-left chain, terminal. No callouts (chat sits there). Chat frame 640×790 at (1200, 140). |
| Ending | `SIGNING OFF // 04` | shutdown terminal (3.5s, then 45–60s), rare squares (8–13s), callouts. No chain. |

All screens: green border draws in, purple corner plates, dot-matrix + scanlines + vignette, socials bottom-left, NEBULA bottom-right.

## OBS setup

**Stinger:** `assets/NEBULA_glitch_stinger_v2.webm`, transition point **600 ms (time-based)**. Same timing/SFX as v1.

**Full-screen scenes** (Starting Soon / Loading / BRB / Ending): browser source 1920×1080, tick **"Refresh browser when scene becomes active"**.

**Loading + BRB:** the page goes at the **bottom**; cam + chat sources go **on top**, sized to sit inside the frames.

**Gameplay** (three separate browser sources, all transparent):

| File | Source size | Position | Layer |
|---|---|---|---|
| `gameplay_vitals` | 1920×1080 | 0, 0 | above the game |
| `gameplay_chat_frame` | 420×276 | 1504, 461 | **under** the chat source |
| `gameplay_cam_frame` | 420×252 | 1504, 706 | **above** the cam source |

Chat source inside the chat frame: (36, 54) → (390, 248). Cam source inside the cam frame: 384×216 at (18, 18).

## Still open

- Check the vitals plate against a **real match** screenshot (it was aligned in the firing range).
- Social handles are placeholders: `[HANDLE]`, `[DAY] · [TIME]`, `TONIGHT: [...]`.
- OBS settings breakdown (encoder, bitrate, recording, multistream, vertical plugin).
