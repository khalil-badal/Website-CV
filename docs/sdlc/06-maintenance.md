# 6. Maintenance

> **Phase in one line:** keep the thing working, deployed, and easy to extend
> after the initial build — the longest phase of any real system's life.

## What maintenance covers on this project

### Deployment
GitHub Pages is set to "Deploy from a branch" → `main` / `(root)`. There is no
build step and no Actions workflow: **any push to `main` redeploys the site** a
minute or two later. That's the entire pipeline (see [README](../../README.md) §1).

### Replacing assets
Every audio clip, image, and the CV PDF is referenced by a relative path. To
update one, **overwrite the file in place** — no code change needed. Raw
originals (HEIC photos, source PDFs, CVs) live in the gitignored
`source-material/` folder for future edits without bloating the repo.

### Documented extension points ([README](../../README.md) §8)
The README spells out exactly how to extend the site so future changes stay
consistent:

- **New project module:** copy a `.module` block; add a 4th accent color in
  *both* `:root` and `[data-theme="dark"]` if needed.
- **New nav section:** update **all three** nav layers — `.nav-links`,
  `.toc-inner`, and `#mobileMenu` — or one viewport will be missing the link.
- **New zoomable image:** reuse `.cert-thumb`; the JS auto-binds it, no script
  change.
- **New colors:** always edit light *and* dark tokens together so themes don't
  drift.

### Known limitations (tracked, not hidden)
Things intentionally not done — no contact form (mailto only), no build tooling —
are recorded in [README](../../README.md) §9 so they read as decisions, not
oversights.

## Prompt-engineering lessons

- **Document conventions so future prompts inherit context.** "Update all three
  nav layers" only stays true because it's written down; a future prompt (or
  person) will otherwise update two and ship a bug.
- **Treat the README as durable memory.** The model starts each session cold —
  the README is what lets a new prompt know *why* the site is one file and *what*
  not to reintroduce.
- **Record decisions, not just code.** Noting *why* the contact form was removed
  stops a well-meaning future prompt from "helpfully" adding it back.
- **Small, in-place, reversible changes age best.** Designing assets to be
  swapped by overwriting a file means routine maintenance never needs the model
  at all — the cheapest maintenance is the kind you don't have to prompt for.
