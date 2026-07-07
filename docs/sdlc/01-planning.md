# 1. Planning

> **Phase in one line:** decide *what* is being built, *why*, who it's for, and
> what "done" looks like — before any code is written.

## What planning covered on this project

The project is a personal portfolio site for **Khalil T. Badal**, an Electronics
Engineering student at UST aiming for a software-engineering career. Before
writing markup, the planning phase settled the questions that shape everything
downstream:

- **Purpose:** one page that presents Khalil's work across three areas — RL /
  software, digital hardware, and audio DSP ("Khyle Audio") — to people
  reviewing him for internships/roles.
- **Scope (deliberately small):** a *single* static `index.html`, no build step,
  no JS framework, no backend. Hosted free on **GitHub Pages** from `main`.
- **Constraints:** no dependencies beyond three Google Fonts; must work as a
  plain file server; must be maintainable by one person.
- **Definition of done:** every section populated with *real* assets (no
  placeholders), responsive from phone to desktop, light **and** dark mode, and
  deployable by a `git push`.

These decisions are captured in the main [README](../../README.md) §1–§2.

## Why planning first

Almost every "known limitation" and every design constraint later in the project
traces back to a planning decision (single file, mailto-only contact, no build
tooling). Because those were chosen *up front*, they read as intentional trade-offs
rather than things we forgot to do.

## Prompt-engineering lessons

- **Put the goal and the constraints in the prompt before the task.** "Build a
  portfolio section" produces generic output; "Build a portfolio section for an
  EE student targeting SWE roles, as a single static HTML file with no
  dependencies" produces something usable. The constraints *are* the spec.
- **Define "done" explicitly.** Telling the model the acceptance criteria
  (real assets, responsive, dark mode, deploys from `main`) lets it self-check
  instead of stopping at "looks plausible."
- **Decide scope before generating, not during.** Locking "one file, no framework"
  early stopped the model from reaching for React/Tailwind/etc. on every request.
- **State the audience.** "For recruiters skimming quickly" changed tone and
  information hierarchy more than any styling instruction did.
