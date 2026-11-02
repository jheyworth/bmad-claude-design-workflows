# Product workflow templates

Each initiative follows one template. The initiative file names it on its own line, below the frontmatter: `Workflow: <id>`, where the id is `poc`, `product`, `existing` or `design-led`.

**What's next.** When the user asks what to do next, read the active initiative's `Workflow:` line and that template's table. The next step is the first one whose "Done when" output does not exist yet in the initiative's folder. Recommend that step's skill, say why in one line, and offer to run it. Never skip a step unless the user says so. With no `Workflow:` line, show the four exec summaries below and ask which one fits. Then ask Sofia (`bmad-agent-po`) to add the line, or add it yourself if the user agrees.

**Claude Design.** Three Claude artifact types do the design work: **Design System** (tokens, components and a brand README that every later design and deck is built on), **Design** (a canvas of screens and mockups) and **Slides** (a deck). The Design System's link is kept in `design-system-<slug>/design-system-<slug>.md` in the initiative's folder, and every later step reads it from there.

**Who owns the look.** In `design-led`, the Design System artifact owns the look and DESIGN.md is copied from it. In every other template, DESIGN.md owns the look and the Design System artifact is published from it.

## Template `poc`: prove one idea works, fast

**In plain English.** For one person testing a product idea in days, not weeks. You end with a tested idea, a look-and-feel published as a Claude Design System, a working demo and a short pitch deck. You skip the brief, the PRD and the review panels.

```
forge idea → UX (fast) → Design System → architecture (fast) → tickets → build → retro → Slides
```

| # | Step | Skill | Done when this exists |
|---|---|---|---|
| 1 | Test the idea | `bmad-forge-idea` | `forge-<slug>/forge-<slug>.md` |
| 2 | Look and behaviour (Fast path) | `bmad-ux` | `ux-<slug>/DESIGN.md` with `status: final` |
| 3 | Publish the Design System from DESIGN.md, then the key screens on a Design canvas (both done at UX Finalize) | `bmad-ux` | `design-system-<slug>/design-system-<slug>.md` |
| 4 | Tech decisions (Fast path) | `bmad-architecture` | `architecture-<slug>/` |
| 5 | Tickets that cite DESIGN.md and the Design System | `bmad-agent-po` | `tickets/epic-<slug>/` |
| 6 | Build, one story per session | `bmad-build` | every story in the epic is `built` |
| 7 | Look back | `bmad-retrospective` | `tickets/epic-<slug>/epic-<slug>-retrospective.md` |
| 8 | Demo deck, built on the Design System | Claude Slides artifact | `slides-<slug>/slides-<slug>.md` holding the deck link |

## Template `product`: a full product several people must agree on

**In plain English.** For a product with stakeholders, several epics, or more than one person building. You end with an agreed PRD, a Claude Design System, an architecture spine, a sliced backlog and reviewed code. It is slower than `poc` because other people have to agree on the PRD.

```
forge idea → brief or PRFAQ → PRD → UX → Design System → architecture → epics and stories → build + code review → retro
```

| # | Step | Skill | Done when this exists |
|---|---|---|---|
| 1 | Test the idea | `bmad-forge-idea` (or `bmad-brainstorming` first) | `forge-<slug>/forge-<slug>.md` |
| 2 | Write the product story | `bmad-product-brief`, or `bmad-prfaq` for a customer-facing product | `brief-<slug>/` or `prfaq-<slug>/` |
| 3 | Requirements people sign off | `bmad-prd` | `prd-<slug>/prd-<slug>.md` |
| 4 | Look and behaviour (Coaching path) | `bmad-ux` | `ux-<slug>/DESIGN.md` with `status: final` |
| 5 | Publish the Design System from DESIGN.md, then the key screens on a Design canvas (both done at UX Finalize) | `bmad-ux` | `design-system-<slug>/design-system-<slug>.md` |
| 6 | Tech decisions | `bmad-architecture` | `architecture-<slug>/` |
| 7 | Initiative requirements, then epics, then stories | `bmad-agent-po` | `tickets/epic-*/` with stories |
| 8 | Build each story, then review the code | `bmad-build`, then `bmad-code-review` | every story is `built` |
| 9 | Look back on each epic | `bmad-retrospective` | `epic-<slug>-retrospective.md` in each epic folder |

## Template `existing`: add to a product whose code already exists

**In plain English.** For new work on an existing codebase. BMad first learns the code's conventions, and the Design System is built from the code's real styles so the new screens match the old ones. You skip the idea and PRD steps; the intent comes from the change you want.

```
project context → Design System from code → UX (update) → architecture (reads code) → tickets → build + code review
```

| # | Step | Skill | Done when this exists |
|---|---|---|---|
| 1 | Learn the code's rules | `bmad-project-context` | the BMad block in `AGENTS.md` |
| 2 | Build the Design System from the code's styles and components | Claude Design System artifact | `design-system-<slug>/design-system-<slug>.md` |
| 3 | Look and behaviour of the new work | `bmad-ux` | `ux-<slug>/DESIGN.md` with `status: final` |
| 4 | Tech decisions that fit the existing code | `bmad-architecture` | `architecture-<slug>/` |
| 5 | Tickets | `bmad-agent-po` | `tickets/epic-<slug>/` |
| 6 | Build each story, then review the code | `bmad-build`, then `bmad-code-review` | every story is `built` |

## Template `design-led`: the look comes first

**In plain English.** For when the visual identity drives the product, such as a brand, a consumer app or a pitch. You design in Claude first: the Design System, then screens on a Design canvas. BMad then turns those into DESIGN.md and EXPERIENCE.md and carries on as usual. The Design System owns the look; DESIGN.md is copied from it.

```
Design System → Design canvas screens → UX (imports the canvas) → architecture → tickets → build → Slides
```

| # | Step | Skill | Done when this exists |
|---|---|---|---|
| 1 | Make the Design System in Claude | Claude Design System artifact | `design-system-<slug>/design-system-<slug>.md` |
| 2 | Key screens on a Design canvas built on that system; export a copy | Claude Design artifact | an export saved under `ux-<slug>/imports/` |
| 3 | Turn the designs into DESIGN.md and EXPERIENCE.md | `bmad-ux` | `ux-<slug>/DESIGN.md` with `status: final` |
| 4 | Tech decisions | `bmad-architecture` | `architecture-<slug>/` |
| 5 | Tickets that cite the Design System | `bmad-agent-po` | `tickets/epic-<slug>/` |
| 6 | Build, one story per session | `bmad-build` | every story is `built` |
| 7 | Demo deck, built on the Design System | Claude Slides artifact | `slides-<slug>/slides-<slug>.md` holding the deck link |
