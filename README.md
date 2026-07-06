# Khalil Badal — Portfolio Site

A single-file, self-contained portfolio site (`index.html`) for Khalil T. Badal —
Electronics Engineering student at UST, spanning reinforcement learning /
software work, hardware/circuit design, and audio DSP under the "Khyle Audio"
brand.

No build step. No dependencies beyond two Google Fonts CDN links. Open
`index.html` directly in any browser.

---

## 1. Design system — "Signal Chain"

**Concept:** everything in Khalil's work is a signal moving through a system
and being transformed — audio through a pedal, current through a circuit,
state through a policy network. Visual identity is inspired by boutique pedal
brand websites (specifically **Chase Bliss Audio**): warm, illustrated,
craft-forward — not a clean corporate SaaS template.

### Color palette (light theme)

All colors are CSS custom properties defined once in `:root`, at the top of
the `<style>` block:

| Variable | Hex | Role |
|---|---|---|
| `--bg` | `#F5F2EA` | Page background (warm cream/paper) |
| `--surface` | `#FFFFFF` | Card backgrounds |
| `--surface-2` | `#EDE9DE` | Secondary surface (module tags, chips, players) |
| `--text` | `#1A1815` | Primary text (near-black) |
| `--muted` | `#4A453C` | Body copy / descriptions (high contrast, readable at length) |
| `--dim` | `#756D5E` | Metadata only — nav links, tag chips, small labels |
| `--brass` | `#3E6B63` | Accent color — currently a dusty teal (variable name is a holdover from an earlier brass/gold iteration; rename if it bothers future-you) |
| `--border` | `#D8D2C3` | All borders, dividers, SVG divider strokes |

**Contrast rule of thumb:** `--muted` is for anything meant to be *read*
(paragraphs, descriptions, timeline entries). `--dim` is only for
decorative/secondary chrome (nav links, tiny tag labels) — never for
sentence-length copy. This distinction was added after early drafts had body
text sitting on too-low-contrast values.

⚠️ **Known fragile spot:** a few decorative elements (SVG divider strokes,
the hero illustration stroke color, two `rgba()` opacity washes) are
**hardcoded hex/rgb values**, not CSS variables, because inline SVG `stroke`
attributes and some `rgba()` washes can't reference CSS custom properties
directly in all contexts used here. If you change `--brass` or `--border`,
search the file for:
- `stroke="#D8D2C3"` (7 instances, the wavy section dividers)
- `stroke="#3E6B63"` (1 instance, the hero pedal illustration)
- `rgba(62,107,99,` (2 instances, teal opacity washes)

and update them to match, or they'll silently drift out of sync with the
palette.

### Typography

Three font families, each with a distinct job — don't blur these roles:

- **`--display`: 'Big Shoulders Display'** (condensed, industrial, "amp
  faceplate" feel) — used only for `h1`, `h2`, `h3`. Uppercase, bold (800
  weight), tight line-height. This is the one place the site gets loud.
- **`--mono`: 'Space Mono'** — used for labels, nav, kickers, tags, module
  metadata, buttons. This is the "technical readout" voice.
- **`--sans`: 'Inter'** — used for actual body copy / paragraphs. This is the
  "just let me read it" voice.

Fonts are loaded from Google Fonts in `<head>`:
```html
<link href="https://fonts.googleapis.com/css2?family=Big+Shoulders+Display:wght@600;800&family=Space+Mono:wght@400;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
```
If deploying somewhere with strict CSP or wanting to go fully offline,
self-host these three families instead.

### Background texture

`body` has a repeating SVG data-URI circuit-trace pattern (right-angle
traces + via dots) at **35% opacity**, tiled at 160×160px, in `--border`
color. This was intentionally dialed down from an earlier full-opacity pass
that felt too busy — if it ever creeps back to feeling loud, drop the
`opacity` value inside the SVG `<g>` tag further (currently `0.35`), don't
just fight it with darker text.

### Signature illustration

The hero includes one hand-drawn-style line-art illustration (a stompbox
pedal with knobs, footswitch, and loose signal-trace squiggles), drawn
directly as inline SVG in the teal accent color. This is deliberately the
**one bold illustrated moment** on the page — per Chase Bliss's own
restraint, the rest of the site doesn't try to compete with it with more
illustrations. If adding future illustrations, keep that principle: one
signature moment per major section at most, not decoration everywhere.

---

## 2. Page structure

Single HTML file, sections in this order (see `<section id="...">` tags):

1. `#top` — Hero (name, one-line identity, pedal illustration, contact links)
2. `#about` — Bio bridging EE / RL / audio threads
3. `#work` — Three featured project "modules": MARL thesis, MPU
   microprocessor design, Khyle Audio plugin suite
4. `#demo` — "Hear My Plugins" interactive 4-state audio switcher (Rootbound
   / Pocket Tracer / both / dry)
5. `#code` — 5 GitHub repo cards
6. `#experience` — Timeline: RoboGear Auto (with inline OST player), UST
   Tiger TV, UST NEES
7. `#education` — Timeline: UST, Romblon State University, CCNA certs
8. `#contact` — Email / GitHub links

A fixed **left-side table of contents** (`<aside class="toc">`) sits outside
this flow, scroll-spies the sections above, and highlights the active one.
It's desktop-only (hidden below `1180px` viewport width) since a fixed left
rail needs real screen real estate; the existing sticky top nav
(`<nav>` → `.nav-links`) handles navigation on narrower screens.

---

## 3. Interactive features

### A. Khyle Audio demo switcher (`#demo`)
Four buttons toggle between four `<audio>` elements (dry / Rootbound /
Pocket Tracer / both). Only one clip plays at a time — clicking a new state
stops whatever's currently playing and starts the new clip from `0`. Active
button gets teal border + highlighted label. Status text and a small dot
indicator show playing/paused/finished state.

### B. RoboGear Auto OST player (inside `#experience`)
A compact inline play/pause button + track title + status text, styled as a
small pill component (`.ost-player`). Coordinated with the demo switcher
above so **only one audio source plays across the whole site at a time** —
starting the OST pauses any active demo clip and resets its UI, and vice
versa.

### C. Scroll-spy table of contents
Plain JS, no framework — on scroll, calculates which section the viewport
is currently over (using each section's `offsetTop` vs. scroll position +
35% of viewport height as the trigger line) and toggles `.active` on the
matching `.toc-link`.

All JS lives in one `<script>` block at the end of `<body>`. No external JS
dependencies.

---

## 4. Placeholder assets to swap in

These are the only things standing between this draft and a fully "real"
site. Each has a clearly marked HTML comment at its location in the file —
search for `<!-- Replace with:` to find all of them.

| Placeholder | Expected filename | Location |
|---|---|---|
| Dry guitar clip | `dry-guitar.mp3` | `#demo` section |
| Rootbound demo clip | `rootbound-guitar.mp3` | `#demo` section |
| Pocket Tracer demo clip | `pockettracer-guitar.mp3` | `#demo` section |
| Both-plugins demo clip | `both-guitar.mp3` | `#demo` section |
| RoboGear Auto OST excerpt | `robogear-ost.mp3` | `#experience`, inline in RoboGear entry |

All five are referenced via relative `src` paths, so just drop the actual
audio files in the same folder as `index.html` and they'll work with zero
code changes.

### Not yet placeholder-stubbed, but worth adding (discussed, not yet built)

- **Plugin GUI screenshots** for Rootbound / Pocket Tracer in the Khyle Audio
  module card (`#work`, module 3)
- **A chart or figure from the thesis manuscript** (e.g. the SARL vs. MARL
  architecture diagram, or the Stage A ideal-condition results chart) in the
  MARL thesis module card (`#work`, module 1)
- **A diagram/photo from the MPU project** (architecture block diagram,
  simulation output) in the MPU module card (`#work`, module 2)
- **Cisco certificate images/PDFs** — currently the `#education` section only
  lists CCNA certs as text; consider linking to or embedding the actual
  certificates
- **Resume/CV PDF download link** near `#contact`
- **Outbound links for the thesis and MPU project** — right now those two
  modules have no link out (unlike the GitHub cards), since there's no public
  repo or hosted PDF for either yet

---

## 5. Extending the site

- **Adding a new project module:** copy one `.module` block inside `#work`,
  update the `.module-tag` (`<span class="tag-label">`) and, if it's a team
  project, the `.team` span with real names — do **not** apply
  `text-transform: lowercase` to a container that also holds proper names
  (this was a real bug caught during development — see note below).
- **Adding a new nav/TOC section:** add the link to both `.nav-links` (top
  nav) and `.toc-inner` (sidebar), matching `href="#id"` /
  `data-section="id"` to a real `<section id="id">` elsewhere in the page.
- **Changing the accent color:** update `--brass` in `:root`, then grep for
  the hardcoded hex/rgba values listed in the "Known fragile spot" note above
  and update those too.

### Bugs already found and fixed during development (don't reintroduce these)

1. **Stray HTML comment inside a `<style>` block** broke the entire
   stylesheet parse for everything below it. HTML comments (`<!-- -->`) are
   not valid inside `<style>` — use `/* */` for CSS comments instead.
2. **`text-transform: lowercase` on a parent element** force-lowercased
   proper names (e.g. "Badal" → "badal") in a nested child span that
   inherited the transform. Fixed by scoping the transform to a dedicated
   `.tag-label` span rather than the whole `.module-tag` container.
3. **Missing `background` on the `html` element** (only `body` had one)
   caused white bleed at the top of the page on some mobile browsers when
   content didn't fully fill the viewport.

---

## 6. Known limitations / not done yet

- No contact form (mailto: links only — intentional, kept simple)
- No dark mode toggle (site is light-theme only; an earlier dark/beige-on-
  black iteration was scrapped in favor of this light theme)
- No build tooling, bundler, or minification — this is meant to be a single
  portable file. If it grows significantly, consider splitting into
  `index.html` / `styles.css` / `script.js` for maintainability, but that's
  optional.
- Not yet tested on an actual desktop browser at full width — this was
  built and reviewed entirely on mobile during the draft phase. **Test on a
  real screen size before treating anything visual as final**, especially
  the sidebar TOC (`>1180px` only) and the multi-column grids in `#work`,
  `#code`, and the hero.

---

## 7. Credits / attribution notes baked into the content

- MARL thesis (`#work`, module 1) is a 5-person team project — Badal, Lopez,
  Mananguit, Marquez, Medenilla — advised by Asst. Prof. Kanny Krizzy D.
  Serrano, MSc. Copy explicitly frames Khalil's contribution (MARL
  architecture, DDQN implementation, simulation/evaluation design) rather
  than claiming sole authorship.
- MPU microprocessor design (`#work`, module 2) is a 9-person team project —
  Abamonga, Badal, Barola, Fernando, Ko, Mananguit, Nacu, Villajin, Zoleta.
  Same framing principle applies.
- Khyle Audio plugin suite (`#work`, module 3) is solo work — no team
  attribution needed.

Keep this framing intact in any future copy edits — it was a deliberate
choice, not an oversight.
