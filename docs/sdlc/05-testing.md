# 5. Testing

> **Phase in one line:** confirm the thing actually behaves correctly — across
> the conditions real users will hit — and fix what doesn't.

## What testing covered on this project

### What was verified
- **Breakpoints:** 375 / 1024 / 1180 / 1200 / 1280 / 1440 / 1920px — no layout
  breakage at any tested width, and all three navigation layers hand off with no
  dead zone.
- **Both themes:** every section checked in light *and* dark mode, including the
  parts that don't re-theme through plain CSS variables (see below).
- **Audio:** only one clip plays at a time; the demo switcher and OST player stop
  each other correctly.

### Bugs caught and fixed (don't reintroduce — [README](../../README.md) §8)
1. A stray **HTML comment inside `<style>`** broke stylesheet parsing for
   everything after it (`<!-- -->` is invalid in CSS; use `/* */`).
2. **`text-transform: lowercase` on a parent** force-lowercased proper names
   ("Badal" → "badal") in a nested span — fixed by scoping the transform.
3. **Missing `background` on `<html>`** caused white bleed at the top on some
   mobile browsers.
4. **Hardcoded SVG `stroke` / `rgba()` colors** couldn't read CSS variables, so
   they silently drifted out of sync once dark mode needed them — fixed with
   `currentColor` and `--*-rgb` token variables.

### A real testing gotcha worth remembering
Automated/scripted `.click()` (via devtools/CDP) can make `audio.play()` fail or
resolve out of order, because it isn't a real user gesture. That looked like a
bug but wasn't — it worked under a real pointer click. **Distinguish a
tool/test-harness artifact from an actual defect before "fixing" it.**

## Prompt-engineering lessons

- **Ask the model to test its own output, with specifics.** "Check this at 375px
  and in dark mode" catches far more than "does it look okay?"
- **Describe observed behavior precisely.** "The teal divider stays light-gray in
  dark mode" points straight at the hardcoded-stroke bug; "dark mode looks off"
  does not.
- **Separate harness artifacts from real bugs.** The scripted-click audio issue
  shows why you verify a suspected bug the way a *user* would trigger it before
  asking the model to change working code.
- **Turn every fixed bug into a durable note.** Writing the four bugs into the
  README as "don't reintroduce these" is what stops the model from regenerating
  them in a later prompt.
