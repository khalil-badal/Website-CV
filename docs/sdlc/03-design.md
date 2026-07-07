# 3. Design

> **Phase in one line:** decide *how* the thing will look and be structured — the
> design system, the information architecture, and the reusable component
> patterns — so implementation is assembly, not invention.

## What design covered on this project

### Design system
The visual identity is intentionally *craft-forward*, inspired by boutique guitar
pedal brands (specifically **Chase Bliss Audio**) rather than a clean corporate
SaaS template — fitting for someone whose work is audio + hardware + software.
The system (full detail in [README](../../README.md) §3) is built on:

- **A tokenized color palette.** Every color is a CSS custom property defined
  once in `:root`, with a `[data-theme="dark"]` block overriding each one. This
  single decision is what made dark mode cheap to add later.
- **Three typefaces with strict, non-overlapping roles:** *Big Shoulders Display*
  (loud industrial headings), *Space Mono* (technical "readout" labels/nav), and
  *Inter* (body copy). Blurring these roles is explicitly disallowed.
- **A circuit-trace background texture** tying the whole page to the electronics
  theme.

### Information architecture
Section order was designed to front-load the strongest material (identity →
featured work → interactive proof) and end on contact. Three-layer responsive
navigation was designed here, not bolted on:

- ≥1180px: fixed sidebar table of contents with scroll-spy
- 641–1180px: inline top-nav links
- ≤640px: hamburger + dropdown menu

### Component patterns
Reusable patterns were designed once and reused: the `.cert-thumb` +
`#lightbox` pair powers every zoomable image; the module accent-stripe uses a
single `--module-accent` variable overridden per card.

## Why design before implementation

The palette-as-tokens decision is the clearest payoff: because design committed
to "every color is a variable," dark mode was later added by writing one
override block instead of touching every rule. Good design decisions show up as
*cheap* changes later.

## Prompt-engineering lessons

- **Establish the design system as reusable context.** Once the palette, fonts,
  and their roles were written down, every later prompt could say "use the
  existing tokens" and get consistent output — the design system became shared
  memory between prompts.
- **Name things, and give the model the *why*.** "`--dim` is metadata only, never
  body copy" constrains the model far more usefully than "use a gray."
- **Design the responsive behavior explicitly.** Spelling out all three nav
  breakpoints up front prevented a real gap (phones briefly had no on-page nav)
  from recurring.
- **Prefer one reusable pattern over many one-offs.** Asking for a single
  lightbox component reused everywhere produced less code — and fewer bugs — than
  asking for each zoomable image separately.
