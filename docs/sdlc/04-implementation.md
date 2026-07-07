# 4. Implementation

> **Phase in one line:** actually build it — write the markup, styles, and
> behavior that turn the design into a working page.

## What implementation covered on this project

Everything lives in one `index.html`: markup, a single `<style>` block, and one
`<script>` at the end of `<body>` (plus a tiny pre-paint script in `<head>`).
No bundler, no framework. The notable implementation pieces:

### Theming via CSS variables
Flipping one attribute on `<html>` re-themes the whole page:

```css
:root{ --bg:#F5F2EA; --text:#1A1815; --brass:#3E6B63; /* …tokens… */ }
[data-theme="dark"]{ --bg:#1B1914; --text:#F2EEE2; --brass:#5EA394; /* …overrides… */ }
```

### No flash of the wrong theme
A tiny script runs *before* the page paints, reading the saved preference (or the
OS setting) and setting the attribute early:

```html
<script>
  (function(){
    var stored = localStorage.getItem('theme');
    var theme = stored || (window.matchMedia &&
      window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
    if (theme === 'dark') document.documentElement.setAttribute('data-theme', 'dark');
  })();
</script>
```

### One audio source at a time
The demo switcher and the OST player coordinate so only one clip ever plays —
starting one stops the other. `play()` is always called *directly inside a click
handler*, which is what browser autoplay policies require.

### Vanilla-JS features
Scroll-spy table of contents, the 4-state audio switcher, a reusable lightbox
that auto-binds any `.cert-thumb`, the theme toggle, and the mobile menu — all
plain DOM code, no libraries.

## Prompt-engineering lessons

- **One feature per prompt.** The scroll-spy, the audio switcher, and dark mode
  were each built and verified separately. Small, single-responsibility prompts
  produce output you can actually check before moving on.
- **Make output verifiable.** Each feature had an observable behavior ("active
  section highlights on scroll") so correctness was testable, not a matter of
  opinion.
- **Reference code by `file:line`.** Pointing the model at the exact block to
  change beat re-describing it in prose and avoided accidental rewrites of
  working code.
- **Match the surrounding code.** Instructing the model to reuse existing tokens
  and the established vanilla-JS style kept the single file coherent instead of
  drifting into three different coding styles.
