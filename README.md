# Khalil Badal — Portfolio Site

A portfolio site for Khalil T. Badal — Electronics Engineering student at UST,
spanning reinforcement learning / software work, digital hardware design, and
audio DSP under the "Khyle Audio" brand.

Live at: `https://khalil-badal.github.io/Website-CV/` (GitHub Pages, deployed
from `main`).

The markup/styles/script live in one `index.html` — no build step, no JS
framework, no dependencies beyond three Google Fonts.

![Portfolio site — hero / top of page (light mode)](assets/screenshots/hero-light.png)

The site ships with a built-in dark mode (toggle in the nav) and a **CV** button
that opens the résumé PDF in a new tab:

![Portfolio site — hero / top of page (dark mode)](assets/screenshots/hero-dark.png)

> 📚 **Building this was also a learning exercise.** Notes mapping the whole build
> to the six SDLC phases — written after the Prompt Engineering Orientation — live
> in [`docs/sdlc/`](docs/sdlc/README.md) and are summarized in §11 below. A full
> section-by-section screenshot gallery is in the [Screenshots](#screenshots) block.

---

## 1. Getting started

There's nothing to install or build. To preview locally:

```
# from the repo root
python -m http.server 8080
# then open http://localhost:8080
```

Opening `index.html` directly via `file://` mostly works too, but some
browsers restrict `fetch`/audio behavior on `file://` URLs — a local server
is the more reliable way to test the audio demo switcher and OST player.

### Deployment
GitHub Pages is configured in the repo's **Settings → Pages** as "Deploy from
a branch," pointed at `main` / `(root)`. There's no GitHub Actions workflow —
Pages rebuilds automatically a minute or two after any push to `main`. Since
it's a single static file with no build step, that's the entire deploy
pipeline.

---

## 2. Repo layout

```
index.html              all markup, CSS, and JS (single file, see below)
README.md               this file
.gitignore
docs/
  sdlc/                  SDLC learning notes, one file per phase (see §11)
assets/
  images/                photos, plugin screenshots, extracted diagrams
  certs/                 CCNA certificate images + Credly badge images
  pdf/                   Khalil-Badal-CV.pdf (linked from the nav "CV" button)
  screenshots/           README screenshots (hero + one per section)
dry-guitar.mp3           Khyle Audio demo clips (repo root, see §6)
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

## 3. Design system

**Concept:** everything in Khalil's work is a signal moving through a system
and being transformed — audio through a pedal, current through a circuit,
state through a policy network. Visual identity is inspired by boutique pedal
brand websites (specifically **Chase Bliss Audio**): warm, craft-forward — not
a clean corporate SaaS template.

### Color palette

All colors are CSS custom properties defined once in `:root` (light theme,
the default) with a `[data-theme="dark"]` block overriding every one of them
for dark mode — see §4 for how the toggle itself works.

| Variable | Light | Dark | Role |
|---|---|---|---|
| `--bg` | `#F5F2EA` | `#1B1914` | Page background |
| `--surface` | `#FFFFFF` | `#242119` | Card backgrounds |
| `--surface-2` | `#EDE9DE` | `#2E2A20` | Secondary surface (module tags, chips, players) |
| `--text` | `#1A1815` | `#F2EEE2` | Primary text |
| `--muted` | `#4A453C` | `#C7C0AE` | Body copy / descriptions (high contrast, readable at length) |
| `--dim` | `#756D5E` | `#8D8570` | Metadata only — nav links, tag chips, small labels |
| `--brass` | `#3E6B63` | `#5EA394` | Primary accent (dusty teal in light, brightened for dark-bg contrast) |
| `--rust` | `#B5502E` | `#E2825A` | Secondary accent — work module 2 (SAP-2) |
| `--gold` | `#B98A2E` | `#E3B15A` | Tertiary accent — work module 3 (Khyle Audio) |
| `--border` | `#D8D2C3` | `#3A3527` | All borders, dividers, SVG divider strokes |

Every color is a token defined once and overridden as a block for dark mode —
this is the single decision that made dark mode cheap to add later:

```css
:root{
  --bg:#F5F2EA;  --surface:#FFFFFF;  --text:#1A1815;
  --brass:#3E6B63; /* primary accent */  --border:#D8D2C3;
  /* …all other tokens… */
}
[data-theme="dark"]{
  --bg:#1B1914;  --surface:#242119;  --text:#F2EEE2;
  --brass:#5EA394; /* brightened for dark-bg contrast */  --border:#3A3527;
}
```

`--bg-rgb` and `--brass-rgb` also exist (the same colors as R,G,B triplets,
no `rgb()`/`#` wrapper) specifically so a couple of translucent
`rgba(var(--bg-rgb),0.88)` / `rgba(var(--brass-rgb),0.08)` uses (the nav
backdrop blur tint, and the active demo-button tint) can react to the theme
without being hardcoded. This used to be a "known fragile spot" — see §4.

**Contrast rule of thumb:** `--muted` is for anything meant to be *read*
(paragraphs, descriptions, timeline entries). `--dim` is only for
decorative/secondary chrome (nav links, tiny tag labels) — never for
sentence-length copy. This holds in both themes.

**Module accent stripes:** each card in `#work` gets a 4px colored left
border via a `--module-accent` custom property, overridden per card with
`.module:nth-child(2)` / `nth-child(3)` — module 1 (thesis) stays the default
teal, module 2 (SAP-2) is rust, module 3 (Khyle Audio) is gold. This was added
specifically to break up what was originally an all-white/cream `#work`
section; the same pattern (a CSS variable overridden by `:nth-child`) is the
easiest way to add a 4th color if a 4th module is ever added.

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
+ via dots), tiled at 160×160px, in `--border` color — 35% opacity in light
mode, 45% in dark mode (needed a touch more to stay visible against the dark
background without looking noisy). There are two separate data-URIs, one
per theme (see `[data-theme="dark"] body` in the CSS) — a data-URI
`background-image` can't reference a CSS custom property internally, so this
couldn't be done with one URI plus a variable.

---

## 4. Dark mode

Toggle button lives in the nav (sun/moon icon, immediately left of the mobile
hamburger). Mechanics:

- **No flash of the wrong theme:** a tiny inline `<script>` right before
  `</head>` reads `localStorage.theme` (falling back to
  `prefers-color-scheme: dark` if nothing's stored yet) and sets
  `data-theme="dark"` on `<html>` *before* the page paints. The full toggle
  logic (click handler, icon swap, `localStorage` write) lives in the main
  script at the bottom of `<body>`, same as everything else.

  ```html
  <!-- runs before <body> paints, so the correct theme is applied on first frame -->
  <script>
    (function(){
      var stored = localStorage.getItem('theme');
      var theme = stored || (window.matchMedia &&
        window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
      if (theme === 'dark') document.documentElement.setAttribute('data-theme', 'dark');
    })();
  </script>
  ```
- **Everything is a CSS variable.** Flipping `data-theme` on `<html>` is
  enough for the vast majority of the page — see §3's palette table.
- **The two exceptions that needed real work:** the 7 wavy `<svg class="trace">`
  section dividers used to have a literal `stroke="#D8D2C3"` hardcoded in the
  markup (inline SVG attributes can't read CSS custom properties), and two
  `rgba()` values (the nav backdrop tint, the active demo-button tint) were
  hardcoded RGB triplets. Fixed by: giving the dividers `stroke="currentColor"`
  plus a `.trace{color:var(--border);}` rule (SVG `currentColor` *does*
  inherit from the CSS `color` property, so this reacts to the theme
  normally), and adding `--bg-rgb`/`--brass-rgb` variables holding the same
  colors as bare `R,G,B` triplets for the `rgba()` cases. If you add another
  hardcoded-color decorative element in the future, follow one of these two
  patterns rather than hardcoding a hex value again.

If you ever debug dark mode and it looks stuck on the wrong theme: check
`localStorage.getItem('theme')` and `document.documentElement.getAttribute('data-theme')`
directly rather than trusting a quick visual glance after a soft reload — in
testing, a plain `location.reload()` occasionally appeared to serve a stale
cached render immediately afterward; a hard navigation (or just trusting what
a real user's browser does on a normal page load) reflects the real behavior.

---

## 5. Page structure

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
   2 CCNA certs. Each cert entry is a two-column row: date/title/description
   on the left, the real Credly badge image (72px, clickable) on the right —
   below 640px the badge stacks under the text instead of sitting beside it.
8. `#contact` ("Let's talk") — email / GitHub links (mailto: only,
   deliberately — see §9)

The nav bar (`nav-right`) also holds two persistent controls to the right of the
links, visible at every viewport width: a **CV** button (`.nav-cv`, a document
icon + "CV" label) that previews `assets/pdf/Khalil-Badal-CV.pdf` in the same
lightbox overlay used for the certificates (see §6C), and the dark-mode toggle
(see §4). On phones, where the inline link row collapses, both stay visible
alongside the hamburger. The button keeps a plain `href` to the PDF as a no-JS
fallback — the click handler `preventDefault()`s it to open the overlay instead.

### Navigation — three layers depending on viewport width
- **≥1180px:** fixed left-side table of contents (`<aside class="toc">`),
  scroll-spies the sections above and highlights the active one.
- **641px–1180px:** sidebar TOC hides (no room for a fixed rail); the sticky
  top nav's inline link row (`.nav-links`) is visible instead.
- **≤640px:** the inline link row *also* hides — there genuinely isn't room
  for `about / work / audio / code / contact` in one line at phone widths.
  A hamburger button (`.nav-hamburger`) appears instead and opens
  `#mobileMenu`, a full-width dropdown with the same 5 links, closing again
  on any link click. **This was a real gap until it was added** — for a
  while, phones (which are almost always under 640px wide) had no on-page
  navigation at all, just the logo and whatever you could reach by
  scrolling. If you resize the nav breakpoints in the future, make sure all
  three layers still hand off to each other with no dead zone in between.

---

## 6. Interactive features

### A. Khyle Audio demo switcher (`#demo`)
Four buttons toggle between four `<audio>` elements (dry / Rootbound /
Pocket Tracer / both). Only one clip plays at a time — clicking a new state
stops whatever's currently playing and starts the new clip from `0`. Active
button gets teal border + highlighted label. Status text and a small dot
indicator show playing/paused/finished state. All 5 audio files are real —
no placeholders remain (see §7).

```js
buttons.forEach(btn => {
  btn.addEventListener('click', () => {
    stopAll();                       // pause + reset every clip first
    setActive(btn);
    currentAudio = clips[btn.dataset.state];
    currentAudio.currentTime = 0;
    currentAudio.play();             // called inside the click = a real user gesture
  });
});
```

### B. RoboGear Auto OST player (inside `#experience`)
A compact inline play/pause button + track title + status text, styled as a
small pill component (`.ost-player`). Coordinated with the demo switcher
above so **only one audio source plays across the whole site at a time** —
starting the OST pauses any active demo clip and resets its UI, and vice
versa.

**Browser autoplay note:** both this and the demo switcher call
`audio.play()` directly inside a click handler, which is exactly what
browser autoplay policies require (a direct user gesture) — this works
correctly in real usage. It's only automated/scripted testing (calling
`.click()` via devtools/CDP rather than a real pointer event) where you might
see `play()` silently fail or resolve out of order; don't mistake that for a
real bug without testing with an actual click first.

### C. Certificate / proof-photo / CV lightbox
A single reusable component (`.cert-thumb` + `#lightbox`): any button with
`data-cert-src` / `data-cert-alt` attributes opens a full-screen overlay with
the full image on click; clicking the backdrop or image, pressing Escape, or
hitting the ✕ close button dismisses it. Currently used in 3 places — the 2 CCNA
certs in `#education`, and the Best Game Audio Design and Music nomination photo
in `#experience`.

The same overlay does double duty for the **CV**: the lightbox holds both an
`<img id="lightboxImg">` and an `<iframe id="lightboxPdf">`. `openImageLightbox()`
shows the image and hides the iframe (certs); `openPdfLightbox()` does the
reverse (the nav CV button, see §5). Clicks *inside* the PDF iframe don't bubble
to the backdrop, so the document stays open while you scroll it — that's why the
persistent ✕ button was added, giving a reliable way to close either mode.

The JS binds *every* `.cert-thumb` on the page automatically — adding a new
zoomable image needs zero script changes, just the two data attributes:

```js
document.querySelectorAll('.cert-thumb').forEach(btn => {
  btn.addEventListener('click', () => {
    lightboxImg.src = btn.dataset.certSrc;      // full-size image
    lightboxImg.alt = btn.dataset.certAlt || '';
    lightbox.classList.add('open');
  });
});
```

The two "thumbnail" treatments on top of the shared `.cert-thumb` behavior
differ by design intent:
- **RoboGear nomination photo:** a small pill button (icon + "View
  nomination" label). Deliberately uses a tightly-cropped image for the
  visible thumbnail but points `data-cert-src` at the full uncropped photo
  (crowd included) for the expanded view, since the crowd is part of what
  makes it read as proof the event happened.
- **CCNA certs:** the real Credly badge image *is* the clickable thumbnail
  (class `cert-thumb cert-badge` on the same `<button>`) — no separate label
  or pill chrome, just the 72px badge with a hover scale-up. `.cert-badge`
  overrides `.cert-thumb`'s default pill styling (border/background/padding)
  back to nothing, since a Credly badge doesn't need a frame around it. This
  is the pattern to copy for "the real asset itself is naturally a good
  thumbnail" cases; use the RoboGear pill pattern instead when the thumbnail
  needs to be cropped differently from the full image, or needs a text label
  to make sense as a button.

### D. Scroll-spy table of contents
Plain JS, no framework — on scroll, calculates which section the viewport is
currently over (using each section's `offsetTop` vs. scroll position + 35% of
viewport height as the trigger line) and toggles `.active` on the matching
`.toc-link`.

### E. Dark mode toggle
See §4.

### F. Mobile nav menu
See §5.

All JS lives in one `<script>` block at the end of `<body>`, plus one tiny
inline script before `</head>` for the pre-paint dark-mode check (see §4). No
external JS dependencies.

---

## 7. Assets

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
| CCNA cert badges (thumbnails) | Certs listed as plain text only in `#education` | Real **Credly badge** images used as the clickable thumbnails in each cert row (72px, `.cert-badge`) — `assets/certs/ccna-intro-to-networks-badge.png`, `ccna-switching-routing-wireless-badge.png` |
| CCNA full certificates (lightbox) | No expanded view | Full certificate scans shown in the lightbox when a badge is clicked — `assets/certs/ccna-intro-to-networks.png`, `ccna-switching-routing-wireless.png` |
| Résumé / CV | CVs existed only in gitignored `source-material/`, not linked from the site | Deployed PDF opened by the nav **CV** button — `assets/pdf/Khalil-Badal-CV.pdf` |
| README screenshots | None | Hero (light + dark) and one image per section, captured from the running site — `assets/screenshots/*.png` (see the [Screenshots](#screenshots) gallery) |

Each CCNA row therefore uses two files: the small Credly **badge** (the thumbnail
you see inline) and the full **certificate** image (what opens in the lightbox).
The `.cert-badge` treatment strips the default pill chrome so the badge stands on
its own — see §6C.

Two photos were added beyond the original placeholder list: the RoboGear PGDX
photo pair (`assets/images/robogear-pgdx-1.jpg`, `-2.jpg`, converted from HEIC
originals) and the Best Game Audio Design and Music nomination photo
(`assets/images/robogear-nominee.jpg` thumbnail crop + `robogear-nominee-full.jpg`
full image for the lightbox).

All five audio files are referenced via relative `src` paths from the repo
root — if any ever need replacing, just overwrite the file in place, no code
changes needed.

### Still not added
- **Outbound links for the thesis and MPU project** — those two `#work`
  modules have no link out (unlike the GitHub cards and Khyle Audio), since
  there's no public repo or hosted PDF for either yet

> **Resolved:** the résumé/CV is now live — `assets/pdf/Khalil-Badal-CV.pdf`,
> opened from the **CV** button in the nav (see §5). It's a copy of
> `source-material/CV_Badal_V2.pdf` promoted into the tracked `assets/` tree so
> it deploys with the site.

---

## 8. Extending the site

- **Adding a new project module:** copy one `.module` block inside `#work`,
  update the `.module-tag` (`<span class="tag-label">`) and, if it's a team
  project, the `.team` span with real names — do **not** apply
  `text-transform: lowercase` to a container that also holds proper names
  (this was a real bug caught during development — see below). If you want a
  distinct accent color for the new module, add a 4th color variable in
  `:root` **and** its dark-mode override in `[data-theme="dark"]`, then a
  `.module:nth-child(4){--module-accent:var(--your-color);}` rule (see §3).
- **Adding a new nav/TOC section:** add the link to `.nav-links` (top nav),
  `.toc-inner` (sidebar), *and* `#mobileMenu` (mobile dropdown) — all three
  need to match `href="#id"` / `data-section="id"` to a real
  `<section id="id">` elsewhere in the page. It's easy to update the first
  two and forget the mobile menu since it's not visible at desktop widths
  while you're editing.
- **Adding a new lightbox-able thumbnail:** reuse `.cert-thumb` — give the
  button `data-cert-src` (the image to show full-size) and `data-cert-alt`;
  the existing JS picks up any `.cert-thumb` on the page automatically, no
  script changes needed. See §6C for the crop-vs-full-image convention.
- **Changing the accent colors:** update `--brass`/`--rust`/`--gold` **in
  both** the `:root` and `[data-theme="dark"]` blocks. Both need updating
  together or the two themes will drift out of sync with each other.
- **Adding a new hardcoded-looking decorative color:** don't — see §4's note
  on `currentColor` and the `--*-rgb` variable pattern, which exist
  specifically so this doesn't need to happen again.

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
4. **Hardcoded SVG `stroke`/`rgba()` colors** (the section dividers, the nav
   backdrop tint, the active demo-button tint) silently drifted out of sync
   whenever `--border`/`--bg`/`--brass` changed, since inline SVG attributes
   and literal `rgba()` triplets can't read CSS custom properties. This
   became a real blocker once dark mode needed those same elements to
   re-theme correctly. Fixed via `currentColor` (for SVG strokes, which
   *does* respect the inherited CSS `color` property) and `--bg-rgb` /
   `--brass-rgb` variables holding bare RGB triplets for the `rgba()` cases.
   See §4.

---

## 9. Known limitations / not done yet

- No contact form (mailto: links only — a real form with a Formspree
  backend was built and then deliberately removed; direct email felt better
  and simpler for this site, and it avoids depending on a third-party
  service just to receive messages)
- No build tooling, bundler, or minification — this is meant to stay a
  single portable file unless it grows enough to justify splitting into
  `index.html` / `styles.css` / `script.js`
- Desktop-width testing (1024/1180/1200/1280/1440/1920px) and mobile
  (375px) has been done — no known layout breakage at any tested breakpoint,
  in both light and dark mode

---

## Screenshots

Every section of the live site, captured from the running page (light mode).
The hero/top-of-page shot — in both light and dark mode — is at the top of this
README.

### About
![About section — bio and skill tags](assets/screenshots/about.png)

### Work — featured modules
![Work section — MARL thesis, SAP-2 microprocessor, and Khyle Audio plugin suite](assets/screenshots/work.png)

### Audio — Khyle Audio demo
![Audio demo section — 4-state plugin switcher](assets/screenshots/demo.png)

### Code — from GitHub
![Code section — GitHub project cards](assets/screenshots/code.png)

### Experience — track record
![Experience section — RoboGear Auto OST player, PGDX photos, and timeline](assets/screenshots/experience.png)

### Education & certifications
![Education section — timeline with clickable CCNA Credly badges](assets/screenshots/education.png)

### Contact
![Contact section — email and GitHub links](assets/screenshots/contact.png)

> Regenerate these anytime with `python -m http.server 8080` running and a
> headless-Chrome screenshot pass — they're plain captures of the deployed
> markup, so any UI change should be re-shot to keep this gallery honest.

---

## 10. Credits / attribution notes baked into the content

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
- The About section deliberately does **not** frame Khalil as specializing
  in "software that interacts closely with hardware" — an earlier draft said
  this, but it doesn't actually hold up for the VST plugin work (pure
  software, no real hardware interfacing) and undersold the actual
  software/AI ambition. His EE/ECE background is framed as an asset
  (grounding in lower-level systems than most software engineers deal with),
  not as the specialization itself.

Keep this framing intact in any future copy edits — none of it is
incidental; each distinction above was a deliberate correction made during
development.

---

## 11. Learning notes — SDLC & prompt engineering

Building this site doubled as a worked example for the **Prompt Engineering
Orientation**. The [`docs/sdlc/`](docs/sdlc/README.md) folder documents the six
**Software Development Life Cycle** phases — the concept, the concrete decisions
made on *this* project, and the prompt-engineering takeaways for each:

| # | Phase | Notes | In one line |
|---|---|---|---|
| 1 | Planning | [docs/sdlc/01-planning.md](docs/sdlc/01-planning.md) | Decide what/why, and what "done" means. |
| 2 | Analysis | [docs/sdlc/02-analysis.md](docs/sdlc/02-analysis.md) | Enumerate requirements + inventory assets. |
| 3 | Design | [docs/sdlc/03-design.md](docs/sdlc/03-design.md) | Design system, IA, reusable components. |
| 4 | Implementation | [docs/sdlc/04-implementation.md](docs/sdlc/04-implementation.md) | Build the single-file site. |
| 5 | Testing | [docs/sdlc/05-testing.md](docs/sdlc/05-testing.md) | Verify across breakpoints/themes; the bugs caught. |
| 6 | Maintenance | [docs/sdlc/06-maintenance.md](docs/sdlc/06-maintenance.md) | Deploy, extend, and keep it healthy. |

The through-line: **a prompt is a spec** — every phase produces the information
(goals, constraints, requirements, design tokens, acceptance criteria) that a
good prompt has to carry. See the [SDLC index](docs/sdlc/README.md) for the full
write-up.
