# SDLC notes — lessons from the Prompt Engineering Orientation

These notes were written after the **Prompt Engineering Orientation**. The goal
was to take the six classic **Software Development Life Cycle (SDLC)** phases and
document what each one actually meant *in practice* — using this portfolio site
(the `index.html` one repo root over) as the worked example, and paying specific
attention to how prompt engineering fit into each phase when the build is done
with an AI assistant.

Each phase has its own file so the concept, the concrete decisions we made on
this project, and the prompt-engineering takeaways stay together:

| # | Phase | File | One-line summary |
|---|---|---|---|
| 1 | Planning | [01-planning.md](01-planning.md) | Decide *what* we're building, *why*, and what "done" means. |
| 2 | Analysis | [02-analysis.md](02-analysis.md) | Turn the goal into concrete, listed requirements and an asset inventory. |
| 3 | Design | [03-design.md](03-design.md) | The design system, information architecture, and component patterns. |
| 4 | Implementation | [04-implementation.md](04-implementation.md) | Building the single-file site — markup, theming, and vanilla-JS features. |
| 5 | Testing | [05-testing.md](05-testing.md) | Verifying behavior across breakpoints/themes, and the bugs we caught. |
| 6 | Maintenance | [06-maintenance.md](06-maintenance.md) | Extending, deploying, and keeping the site healthy over time. |

## The one idea that ties them together

> **A prompt is a spec.** Every SDLC phase produces information that a good prompt
> needs to carry — goals, constraints, requirements, design tokens, acceptance
> criteria. The clearer the phase's output, the better the prompt, and the less
> rework at the next phase.

The phases are drawn as a linear list for teaching, but the real work looped:
testing surfaced design gaps, maintenance reopened analysis. That iteration is a
feature of the model, not a failure of the plan.
