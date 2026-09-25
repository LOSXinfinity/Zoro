# ⚔️ ZORO — King of Hell

> *"Nothing happened."*

A cinematic, scroll-driven fan page for **Roronoa Zoro** (One Piece) — built as a **single HTML file** with vanilla JavaScript, GSAP ScrollTrigger, and the Canvas API. No frameworks, no build step. Open it and scroll.

![Made with](https://img.shields.io/badge/Made%20with-Vanilla%20JS-b34dff?style=flat-square)
![GSAP](https://img.shields.io/badge/Animation-GSAP%20ScrollTrigger-e0004d?style=flat-square)
![No Framework](https://img.shields.io/badge/Framework-None-07050d?style=flat-square)

---

## ✨ Features

### 👁️ Scroll-Scrubbed Eye Opening
- A 50-frame image sequence scrubbed by scroll position, with frame cross-blending for buttery smooth playback
- Feathered "eyelid" overlays that part as you scroll
- Lightning strikes fire at opening thresholds (28% / 62% / fully open)
- Once fully open, a light veil fades in with ghost kanji — **地獄の王** (King of Hell)

### ⚡ Realistic Lightning Engine
Not zigzag lines — a proper strike system:
- **Fractal channels** built with midpoint displacement (big kinks up high, fine detail low)
- **4-layer rendering**: wide violet corona → haze → pale sheath → white-hot core, with tapering forked branches
- **Real restrike behavior**: each strike keeps its channel and re-illuminates the *same path* 2–3 times, just like actual lightning
- **Local sky illumination** around the strike origin instead of a flat full-screen flash

### 🔊 Procedural Thunder (Web Audio API)
Three synthesized layers, no audio files:
1. A sharp high-frequency **crack** at the strike
2. A **rolling body** whose loudness wanders as it decays, with a lowpass sweep as the sound "travels"
3. A slow **sub-bass rumble** underneath

### 🗡️ Gravity Sword Drop
- Intro screen with a blurred Zoro portrait + title text that lifts and dissolves as the fall begins
- The katana hangs, accelerates with gravity easing, and **slams** down
- Speed-based motion blur, vertical stretch smear, air sway, and a fading light trail with a hot tip
- On impact: thunder strike into the blade, screen shake, shockwave — then the Zoro cut reveal with ambient crackling arcs
- Scroll input is **lerp-smoothed**, so even a fast wheel-fling plays the full fall instead of jumping to the end

### ⚔️ Slash-Split Hover Reveal
- Drag the tilted blade line across the screen to split between two artworks
- Spark particles trail the cut; fast drags call lightning down near your cursor
- Full-screen thunderstorm fires when your cursor enters the section
- Electric-border cards (a vanilla remake of the ReactBits "Electric Border" effect)

### 🎨 Extra Polish
- Slash-wipe preloader, film grain, custom cursor, live HUD clock + section state
- `prefers-reduced-motion` respected throughout
- Audio gated behind first user interaction (browser autoplay policy)

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Structure & styling | HTML5, CSS3 (custom properties, masks, blend modes) |
| Scroll animation | [GSAP 3](https://gsap.com/) + ScrollTrigger (CDN) |
| Lightning, wind, sparks, trails | Canvas 2D API |
| Thunder audio | Web Audio API (fully procedural) |
| Fonts | Pirata One · Space Mono · Shippori Mincho · Bebas Neue (Google Fonts) |

---

## 📁 Project Structure

```
├── index.html          # everything lives here — markup, styles, and scripts
├── frames/
│   ├── frame_0001.jpg  # eye-opening sequence (50 frames)
│   └── ... frame_0050.jpg
└── img/
    ├── sword.png       # falling katana
    ├── zoro_cut.png    # impact reveal artwork
    ├── zoro_dark.png   # sword-drop intro backdrop
    ├── zoro1.png       # slash split — top layer
    └── zoro2.png       # slash split — bottom layer
```

---

## 🚀 Getting Started

```bash
git clone https://github.com/LOSXinfinity/Zoro.git
cd Zoro
```

Then serve it locally (any static server works):

```bash
# Python
python -m http.server 8000

# or Node
npx serve
```

Open `http://localhost:8000` and scroll. That's it — no install, no build.

> **Tip:** click or scroll once before expecting sound — browsers block audio until the first user interaction.

---

## 🎛️ Tuning Knobs

A few values worth knowing if you want to remix it:

| What | Where | Default |
|---|---|---|
| Sword-fall glide smoothness | `dropFrame()` lerp factor | `0.09` (lower = floatier) |
| Eye lightning thresholds | `eyeStrikes` array | `0.28 / 0.62 / 0.96` |
| Hover-storm cooldown | `lastMoveBolt` throttle | `800ms` |
| Intro text lifetime | `clamp(p/0.16, ...)` in `applyDrop` | first 16% of scroll |
| Thunder loudness / length | `thunderSound(vol, dur)` calls | varies per strike |

---

## 🌐 Browser Support

Works in all modern browsers (Chrome, Edge, Firefox, Safari). Best experienced on desktop with a mouse for the slash-split section; touch is supported everywhere.

---

## ⚠️ Disclaimer

This is a **non-commercial fan project**. Roronoa Zoro and One Piece are the property of **Eiichiro Oda, and Toei Animation**. All character artwork belongs to its respective owners. The code is free to learn from and remix.

---

## 💜 Support

If this project helped or inspired you:

- ⭐ Star this repo
- 🍴 Fork it and build your own character page
- 📸 Follow me on Instagram for more builds :[https://www.instagram.com/jahid_mahin/](url)

*Forged in Wano.* 🌊⚡

---

## ⚡ Performance Notes

| Aspect | Detail |
|--------|--------|
| **Frame budget** | Targets 60fps; `requestAnimationFrame` loops for eye sequence, wind trails, and ambient aura |
| **GPU acceleration** | `will-change` on animated elements; Canvas layers use `lighter` composite for additive lightning |
| **Image loading** | 50 eye frames preload async; fallback message if any frame fails |
| **Memory** | Single HTML ~40KB gzipped; frames ~2–5MB total depending on compression |
| **Reduced motion** | `prefers-reduced-motion: reduce` disables grain, cursor trails, preloader wipe, electric borders, and all `requestAnimationFrame` loops |
| **Scroll jank prevention** | GSAP `scrub` values tuned per-section; sword drop uses 0.45 scrub + lerp smoothing (`0.09` factor) so fast flings don't skip frames |

---

## ♿ Accessibility

- **Reduced motion**: Fully respected — all decorative animation pauses, lightning/restrikes skipped, preloader instant
- **Keyboard nav**: Scroll-driven; no focus traps. HUD clock updates via `setInterval`
- **Color contrast**: Primary text on `--ink` (`#07050d`) meets WCAG AA; accent colors (`--oni`, `--blood`) are decorative
- **No flashing**: Lightning flashes are local (Canvas), not full-screen; frequency stays well below 3Hz threshold
- **Alt text**: All `<img>` have descriptive `alt`; decorative canvases have `aria-hidden="true"`
- **Autoplay audio**: Gated behind first `pointerdown`/`touchstart`/`keydown`/`wheel` — compliant with browser policies

---

## 🐛 Debugging & Console Helpers

Open DevTools console and try:

```js
// Force eye fully open
eyeTargetFrame = 49; eyeFrame = 49; renderEyeFrame(49);

// Trigger sword impact thunder manually
thunder();

// Spawn a lightning strike at cursor (slash section)
const rect = slashBoltC.getBoundingClientRect();
const st = buildStrike(event.clientX, -20, event.clientX, rect.height, rect.width * 0.14);
strikeSeq(a => drawStrike(slctx, st, 2, a, rect.width * 0.4), a => slashFlash.style.opacity = a * 0.3, () => { slctx.clearRect(0,0,rect.width,rect.height); slashFlash.style.opacity = 0; });

// Toggle reduced motion simulation
document.documentElement.style.setProperty('--reduced', '1'); // or remove to re-enable

// Inspect current scroll progress per section
ScrollTrigger.getAll().forEach(st => console.log(st.vars.id || st.trigger.id, st.progress.toFixed(3)));

// Audio context state
actx?.state // 'running' | 'suspended' | 'closed'
```

---

## 🎨 Customization Guide

### Swap the character
1. Replace `frames/frame_*.jpg` with your own 50-frame sequence (same naming)
2. Update `img/zoro_dark.png`, `img/zoro_cut.png`, `img/zoro1.png`, `img/zoro2.png`
3. Change kanji in `.eye__awake-k`, `.drop__glyphs span`, `.drop__seal`, `.outro .k`
4. Adjust colors in `:root` — `--oni`, `--blood`, `--steel`, `--ink`

### Add a hero video
Place an MP4 at `video/hero.mp4` — the `.hero__video` element will auto-play muted, looped, and cover the hero section. The fallback image (first eye frame) shows if video fails.

### Change fonts
Edit the Google Fonts `<link>` in `<head>` and update `--disp`, `--mono`, `--kanji`, `--beb` in `:root`.

### Tweak lightning "personality"
```js
// In buildStrike(): rough controls jaggedness
// In drawStrike(): layer widths (coreW*9, *3.6, *1.5, *0.55) control glow thickness
// In strikeSeq(): seq array = [main, gap, return1, gap, return2, ...] — adjust alphas & delays
```

---

## ⚠️ Known Limitations

| Limitation | Workaround / Note |
|------------|-------------------|
| **No IE11 / legacy Edge** | Uses `clamp()`, `ScrollTrigger`, `AudioContext`, `IntersectionObserver` — polyfills not included |
| **Mobile slash-split** | Touch drag works but lacks hover-triggered thunderstorm; consider adding `touchmove` listener |
| **Very long pages** | `height: 260vh/340vh` sections assume desktop viewport; extreme mobile heights may need CSS adjustments |
| **AudioContext suspension** | If user never interacts, `thunderSound()` silently returns; call `actx.resume()` on first gesture |
| **High-DPI canvas blur** | `DPR = Math.min(devicePixelRatio, 2)` caps at 2×; increase for 3× screens if needed |
| **Frame sequence format** | Hardcoded to 50 JPGs; change `eyeFrames` array length & padding for different counts |

---

## 🤝 Contributing

This is a learning/portfolio piece, but PRs welcome for:

- **Performance**: WebGL lightning port, OffscreenCanvas, `requestIdleCallback` for frame loading
- **Accessibility**: Better screen-reader announcements for section transitions
- **Mobile**: Touch gesture improvements, haptic feedback (Vibration API) on impact
- **Code quality**: TypeScript port, ESLint/Prettier config, unit tests for `boltPath`/`thunderSound`
- **Localization**: i18n for HUD, kanji, preloader text

1. Fork → branch → PR with clear description
2. Keep it **single-file** (or document build step if you add one)
3. No external deps beyond GSAP CDN

---

## 📄 License

**Code**: MIT License — free to use, modify, distribute.

**Assets** (`frames/`, `img/`, `video/`): **Not licensed for reuse**. These are fan-art / screenshots from One Piece. Replace with your own artwork before publishing derivatives.

**Character IP**: Roronoa Zoro, One Piece © Eiichiro Oda/ Toei Animation. This project is non-commercial transformative work.

---

## 🙏 Credits & Inspiration

- **GSAP / ScrollTrigger** — GreenSock (@GreenSock)
- **Electric Border effect** — ReactBits (vanilla remake in `.slash__card`)
- **Lightning reference** — "How to draw realistic lightning" by Inigo Quilez / Shadertoy community
- **Thunder synthesis** — Web Audio API noise shaping techniques (MDN, various Gists)
- **Fonts** — Google Fonts (Pirata One, Space Mono, Shippori Mincho, Bebas Neue)
- **One Piece** — Eiichiro Oda, for the goat swordsman

---

*Forged in Wano.* 🌊⚡
