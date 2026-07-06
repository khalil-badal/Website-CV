# Khalil Badal — Portfolio Site

A portfolio site for Khalil T. Badal — Electronics Engineering student at UST,
spanning reinforcement learning / software work, digital hardware design, and
audio DSP under the "Khyle Audio" brand.

Live at: `https://khalil-badal.github.io/Website-CV/` (GitHub Pages, deployed
from `main`).

The markup/styles/script live in one `index.html` — no build step, no JS
framework, no dependencies beyond three Google Fonts. Open `index.html`
directly in a browser, or run any static file server from the repo root.

---

## 1. Repo layout

```
index.html              all markup, CSS, and JS (single file, see below)
README.md               this file
.gitignore
assets/
  images/                photos, screenshots, extracted diagrams
  certs/                 CCNA certificate images
dry-guitar.mp3           Khyle Audio demo clips (repo root, see §4)
rootbound-guitar.mp3
pockettracer-guitar.mp3
both-guitar.mp3
robogear-ost.mp3
source-material/         gitignored — raw originals (PDFs, HEIC photos, CVs)
                         used to produce the assets/ files. Not needed for
                         deployment; kept locally for reference only.
```

**Why one HTML file:** the site is small enough that splitting into
`index.html` / `styles.css` / `script.js` wouldn't meaningfully help
maintainability yet. Revisit if it keeps growing.

**Why `source-material/` is gitignored:** those are raw inputs (a 9MB PDF, HEIC
originals, CVs) used once to produce the actual web assets — not needed to
build or deploy the site. They stay on disk for anyone extending the project
later, just not in version control.

---

## 2. Design system — "Signal Chain"

**Concept:** everything in Khalil's work is a signal moving through a system
and being transformed — audio through a pedal, current through a circuit,
state through a policy network. Visual identity is inspired by boutique pedal
brand websites (specifically **Chase Bliss Audio**): warm, craft-forward — not
a clean corporate SaaS template.

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
| `--brass` | `#3E6B63` | Primary accent — a dusty teal (variable name is a holdover from an earlier brass/gold iteration) |
| `--rust` | `#B5502E` | Secondary accent — used on work module 2 (SAP-2) |
| `--gold` | `#B98A2E` | Tertiary accent — used on work module 3 (Khyle Audio) |
| `--border` | `#D8D2C3` | All borders, dividers, SVG divider strokes |

**Contrast rule of thumb:** `--muted` is for anything meant to be *read*
(paragraphs, descriptions, timeline entries). `--dim` is only for
decorative/secondary chrome (nav links, tiny tag labels) — never for
sentence-length copy.

**Module accent stripes:** each card in `#work` gets a 4px colored left
border via a `--module-accent` custom property, overridden per card with
`.module:nth-child(2)` / `nth-child(3)` — module 1 (thesis) stays the default
teal, module 2 (SAP-2) is rust, module 3 (Khyle Audio) is gold. This was added
specifically to break up what was originally an all-white/cream `#work`
section; the same pattern (a CSS variable overridden by `:nth-child`) is the
easiest way to add a 4th color if a 4th module is ever added.

⚠️ **Known fragile spot:** a handful of decorative SVG elements are
**hardcoded hex/rgba values**, not CSS variables, because inline SVG `stroke`
attributes and `rgba()` washes can't reference CSS custom properties directly
in all the contexts used here. If you change `--border` or `--brass`, grep
for and update:
- `stroke="#D8D2C3"` (7 instances — the wavy section dividers between sections)
- `rgba(62,107,99,` (1 instance — the active-state tint on `.demo-btn.active`)

(The hero illustration that used to need its own hardcoded `stroke="#3E6B63"`
entry here was removed — see §6.)

### Typography

Three font families, each with a distinct job — don't blur these roles:

- **`--display`: 'Big Shoulders Display'** (condensed, industrial, "amp
  faceplate" feel) — used for `h1`/`h2`/`h3` and the nav wordmark
  (`.nav-logo`). Uppercase, bold (800 weight), tight line-height. This is the
  one place the site gets loud/distinctive.
- **`--mono`: 'Space Mono'** — used for labels, nav links, kickers, tags,
  module metadata, buttons. This is the "technical readout" voice.
- **`--sans`: 'Inter'** — used for actual body copy / paragraphs. This is the
  "just let me read it" voice.

Fonts are loaded from Google Fonts in `<head>`:
```html
<link href="https://fonts.googleapis.com/css2?family=Big+Shoulders+Display:wght@600;800&family=Space+Mono:wght@400;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
```
If deploying somewhere with strict CSP or wanting to go fully offline,
self-host these three families instead.

### Background texture

`body` has a repeating SVG data-URI circuit-trace pattern (right-angle traces
+ via dots) at **35% opacity**, tiled at 160×160px, in `--border` color. If it
ever creeps back to feeling loud, drop the `opacity` value inside the SVG
`<g>` tag (currently `0.35`) rather than fighting it with darker text.

---

## 3. Page structure

Single HTML file, sections in this order (see `<section id="...">` tags):

1. `#top` — Hero (name, one-line identity, contact links). No illustration —
   an earlier draft had a hand-drawn pedal SVG here; it was cut for feeling
   like an unnecessary flourish, and the hero grid collapsed to a single
   full-width text column rather than replaced with something else.
2. `#about` ("Background") — bio in first person, split across a two-column
   grid (4 short paragraphs + skill tags)
3. `#work` ("Modules") — three featured project cards: MARL thesis (teal
   accent), SAP-2 microprocessor design (rust accent), Khyle Audio plugin
   suite (gold accent). Each includes a supporting figure/screenshot.
4. `#demo` ("Khyle Audio demo") — "Hear My Plugins" interactive 4-state audio
   switcher (Rootbound / Pocket Tracer / both / dry)
5. `#code` ("From GitHub") — 2 repo cards: Spotify Data Analysis (own
   description), and a consolidated "UST ECE Advanced Programming Course"
   card linking out to 4 near-identical coursework exercises (`Exp. 1`–`Exp.
   4`) rather than 4 separate cards with duplicate descriptions
6. `#experience` ("Track record") — Timeline: RoboGear Auto (OST player +
   PGDX photo pair + nomination-proof lightbox thumbnail), UST Tiger TV, UST
   NEES
7. `#education` ("Foundations") — Timeline: UST, Romblon State University,
   2 CCNA certs (thumbnail + lightbox each)
8. `#contact` ("Let's talk") — Email / GitHub links

A fixed **left-side table of contents** (`<aside class="toc">`) sits outside
this flow, scroll-spies the sections above, and highlights the active one.
It's desktop-only (hidden at ≤1180px viewport width, confirmed via testing at
1180/1200px) since a fixed left rail needs real screen real estate; the
sticky top nav (`<nav>` → `.nav-links`) handles navigation on narrower
screens.

---

## 4. Interactive features

### A. Khyle Audio demo switcher (`#demo`)
Four buttons toggle between four `<audio>` elements (dry / Rootbound /
Pocket Tracer / both). Only one clip plays at a time — clicking a new state
stops whatever's currently playing and starts the new clip from `0`. Active
button gets teal border + highlighted label. Status text and a small dot
indicator show playing/paused/finished state. All 5 audio files are real
(see §5) — no placeholders remain.

### B. RoboGear Auto OST player (inside `#experience`)
A compact inline play/pause button + track title + status text, styled as a
small pill component (`.ost-player`). Coordinated with the demo switcher
above so **only one audio source plays across the whole site at a time** —
starting the OST pauses any active demo clip and resets its UI, and vice
versa. (Verified: clicking through both rapidly in the same session can look
like it's misbehaving during manual/scripted testing due to promise timing —
if debugging this, always confirm with a genuinely fresh page load rather
than a soft reload, and check actual `<audio>`.paused state, not just the UI
text.)

### C. Certificate / proof-photo lightbox
A single reusable component (`.cert-thumb` + `#lightbox`): any button with
`data-cert-src` / `data-cert-alt` attributes shows a small thumbnail that,
when clicked, opens a full-screen overlay with the full image; clicking
anywhere (or pressing Escape) closes it. Currently used in 3 places — the 2
CCNA certs in `#education`, and the Best Game Audio Design and Music
nomination photo in `#experience`. The RoboGear entry deliberately uses a
tightly-cropped image for the small thumbnail but points `data-cert-src` at
the full uncropped photo (crowd included) for the expanded view, since the
crowd is part of what makes it read as proof the event happened. Follow that
same pattern (cropped thumbnail, full image on expand) for any future
addition to this component.

### D. Scroll-spy table of contents
Plain JS, no framework — on scroll, calculates which section the viewport is
currently over (using each section's `offsetTop` vs. scroll position + 35% of
viewport height as the trigger line) and toggles `.active` on the matching
`.toc-link`.

All JS lives in one `<script>` block at the end of `<body>`. No external JS
dependencies.

---

## 5. Assets

The original draft was built entirely around placeholders — five `<audio>`
tags pointing at `.mp3` files that didn't exist yet, each marked with an
HTML comment (`<!-- Replace with: ... -->`), plus a handful of gaps that
weren't even stubbed out yet (no screenshots, no diagrams, certs listed as
plain text). All of that has since been resolved:

| Item | Originally | Now |
|---|---|---|
| Dry / Rootbound / Pocket Tracer / both-plugins demo clips | Placeholder `<audio>` tags, `<!-- Replace with: ... -->` comments, files didn't exist | Real recordings — `dry-guitar.mp3`, `rootbound-guitar.mp3`, `pockettracer-guitar.mp3`, `both-guitar.mp3` (repo root) |
| RoboGear Auto OST excerpt | Same placeholder pattern, inline in the RoboGear entry | Real excerpt — `robogear-ost.mp3` (repo root) |
| Rootbound / Pocket Tracer GUI | Not built yet, just discussed | Real plugin screenshots — `assets/images/rootbound-gui.png`, `pockettracer-gui.png` |
| Thesis architecture figure | Not built yet, just discussed | SARL-vs-MARL diagram cropped from the thesis manuscript — `assets/images/thesis-marl-diagram.png` |
| MPU diagram | Not built yet, just discussed | SAP-2 block diagram extracted from the project PDF — `assets/images/mpu-block-diagram.png` |
| CCNA certs | Listed as plain text only in `#education` | Real cert images, each behind a thumbnail + lightbox — `assets/certs/ccna-intro-to-networks.png`, `ccna-switching-routing-wireless.png` |

Two photos were added beyond the original placeholder list: the RoboGear PGDX
photo pair (`assets/images/robogear-pgdx-1.jpg`, `-2.jpg`, converted from HEIC
originals) and the Best Game Audio Design and Music nomination photo
(`assets/images/robogear-nominee.jpg` thumbnail crop + `robogear-nominee-full.jpg`
full image for the lightbox).

All five audio files are referenced via relative `src` paths from the repo
root — if any ever need replacing, just overwrite the file in place, no code
changes needed.

### Still not added
- **Resume/CV PDF download link** near `#contact` — CVs exist in
  `source-material/` but aren't linked from the live site (not requested yet)
- **Outbound links for the thesis and MPU project** — those two `#work`
  modules have no link out (unlike the GitHub cards and Khyle Audio), since
  there's no public repo or hosted PDF for either yet

---

## 6. Extending the site

- **Adding a new project module:** copy one `.module` block inside `#work`,
  update the `.module-tag` (`<span class="tag-label">`) and, if it's a team
  project, the `.team` span with real names — do **not** apply
  `text-transform: lowercase` to a container that also holds proper names
  (this was a real bug caught during development — see below). If you want a
  distinct accent color for the new module, add a 4th color variable in
  `:root` and a `.module:nth-child(4){--module-accent:var(--your-color);}`
  rule (see §2).
- **Adding a new nav/TOC section:** add the link to both `.nav-links` (top
  nav) and `.toc-inner` (sidebar), matching `href="#id"` /
  `data-section="id"` to a real `<section id="id">` elsewhere in the page.
- **Adding a new lightbox-able thumbnail:** reuse `.cert-thumb` — give the
  button `data-cert-src` (the image to show full-size) and `data-cert-alt`;
  the existing JS picks up any `.cert-thumb` on the page automatically, no
  script changes needed. See §4C for the crop-vs-full-image convention.
- **Changing the accent colors:** update `--brass`/`--rust`/`--gold` in
  `:root`, then grep for the hardcoded hex/rgba values listed in the "Known
  fragile spot" note in §2 and update those too.

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

## 7. Known limitations / not done yet

- No contact form (mailto: links only — intentional, kept simple)
- No dark mode toggle (site is light-theme only)
- No build tooling, bundler, or minification — this is meant to stay a
  single portable file unless it grows enough to justify splitting into
  `index.html` / `styles.css` / `script.js`
- Desktop-width testing (1024/1180/1200/1280/1440/1920px) and mobile
  (375px) has been done — no known layout breakage at any tested breakpoint

---

## 8. Credits / attribution notes baked into the content

- MARL thesis (`#work`, module 1) is a 5-person team project, currently at
  the development stage (proposal complete) — Badal, Lopez, Mananguit,
  Marquez, Medenilla — advised by Asst. Prof. Kanny Krizzy D. Serrano, MSc.
- SAP-2 microprocessor design (`#work`, module 2) is a 9-person team project
  — Abamonga, Badal, Barola, Fernando, Ko, Mananguit, Nacu, Villajin,
  Zoleta. Copy explicitly credits Khalil with leading the architecture and
  control unit design *and* driving the bulk of the cross-module integration
  debugging — this is deliberately specific (not just "contributed to"),
  since that debugging work was substantial and was previously undersold in
  an earlier copy pass.
- Khyle Audio plugin suite (`#work`, module 3) is solo work — no team
  attribution needed. Copy is careful to frame the plugins as *inspired by*
  favorite gear's sound and features (then built out from there in original
  code), not as literal circuit models — an earlier draft overclaimed
  "modeling real circuit behavior," which wasn't accurate.
- RoboGear Auto (`#experience`) copy is specific that Khalil was the team's
  **sole** sound engineer working directly with developers, not one of
  several collaborating sound engineers, and that the Best Sound Design
  nomination is separate from (concurrent with) the studio's own Best Mobile
  Game win — don't conflate the two, or blur "nominee" into "winner."

Keep this framing intact in any future copy edits — none of it is
incidental; each distinction above was a deliberate correction made during
development.
