# 2. Analysis

> **Phase in one line:** turn the goal into a concrete, enumerated list of
> requirements — functional and non-functional — plus an inventory of what
> assets and content the build actually needs.

## What analysis covered on this project

### Content / section requirements
The single page was broken down into eight sections, each with a defined job
(see [README](../../README.md) §5):

1. Hero — name, one-line identity, contact links
2. About — first-person bio + skill tags
3. Work ("Modules") — three deep technical builds (MARL thesis, SAP-2 MPU, Khyle Audio)
4. Audio demo — interactive 4-state plugin switcher
5. Code — GitHub project cards
6. Experience — timeline (RoboGear Auto, Tiger TV, NEES)
7. Education — timeline + CCNA certifications
8. Contact — email + GitHub

### Non-functional requirements
- **Responsive** across phone → desktop (tested 375–1920px).
- **Dark mode** with no flash of the wrong theme on load.
- **Accessible**: keyboard focus states, `aria-*` on interactive controls, alt text.
- **Zero dependencies** beyond fonts; deployable as static files.

### Asset inventory (the gap analysis)
Analysis is also where we listed every asset the design would require and marked
what existed vs. what was still a placeholder. This inventory drove the rest of
the project and is now tracked in [README](../../README.md) §7 — audio clips,
plugin GUI screenshots, extracted thesis/MPU diagrams, CCNA certificate images
and Credly badges, RoboGear photos, and the CV PDF.

## Why analysis matters

The placeholder-vs-real asset list turned a vague "add screenshots later" into a
checklist. Every item on it eventually flipped from *placeholder* to *real*, and
the ones that couldn't (no public repo for the thesis yet) became documented
*known limitations* instead of silent gaps.

## Prompt-engineering lessons

- **Enumerate requirements explicitly; don't imply them.** A numbered list of
  sections in the prompt produced a complete page; "make a portfolio" produced a
  hero and not much else.
- **Separate must-have from nice-to-have.** Tagging dark mode and responsiveness
  as *required* (vs. animations as *optional*) kept the model from spending
  effort in the wrong place.
- **Give the model an inventory.** Listing which assets existed and which were
  placeholders let it wire up the real files and clearly mark the stubs, instead
  of inventing image paths.
- **Ambiguity surfaces here or in testing — cheaper here.** Questions like "is
  contact a form or a mailto link?" are one line to resolve during analysis and
  a rebuild to resolve after implementation.
