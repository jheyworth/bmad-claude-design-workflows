# BMad product workflow templates, with Claude Design

**One prompt turns a stock BMad install into a guided product workflow.** You choose a template for your initiative, and from then on `/bmad what's next?` tells you the next step and offers to run it. Claude Design artifacts (Design System, Design canvas, Slides) are built into the steps.

## The four templates

| Template | Use it when | The flow |
|---|---|---|
| `poc` | You're testing one idea alone, fast | forge idea → UX (fast) → Design System → architecture (fast) → tickets → build → retro → Slides |
| `product` | Several people must agree; several epics | forge idea → brief or PRFAQ → PRD → UX → Design System → architecture → epics and stories → build + code review → retro |
| `existing` | You're adding to code that already exists | project context → Design System from code → UX → architecture → tickets → build + code review |
| `design-led` | The look comes first | Design System → Design canvas → UX → architecture → tickets → build → Slides |

Each template, with its plain-English summary and step table, is in [SETUP-PROMPT.md](SETUP-PROMPT.md).

## How to use it

1. **Install BMad** in your project: `npx skills add bmad-code-org/BMAD-METHOD --skill '*' -y`, then `/bmad setup`.
2. **Paste the setup prompt.** Open Claude Code in the project and paste everything below the line in [SETUP-PROMPT.md](SETUP-PROMPT.md).
3. **Start an initiative** with `/bmad` and add a line below its frontmatter, for example `Workflow: poc`.
4. **Ask `/bmad what's next?`** whenever you finish a step.

## What the prompt changes

It never overwrites an existing file. Where a file exists, it adds its entries to it.

| File | What it's for |
|---|---|
| `.claude/skills/bmod-sdlc/bmod.toml` and `help/help.md` | The templates. `/bmad` takes its "what's next" answers only from module help files, so they have to live here. It's two files, with no skills and no code |
| `_bmad/custom/bmad-ux.toml` | Has bmad-ux render options on a Claude Design canvas and publish DESIGN.md as a Claude Design System |
| `_bmad/custom/bmad-agent-po.toml` | Has Sofia keep the `Workflow:` line and cite the Design System in UI tickets |

## Why a module and not only customisations

`/bmad` has no customisation file, so bmad-customize can't change how it picks the next step. Its rules say to take routes only from module help and to treat every other file as evidence, not instructions. A module made of two files is the smallest thing it will follow.

## Who owns the look

- **`design-led`:** the Claude Design System artifact is the master, and DESIGN.md is copied from it.
- **Every other template:** DESIGN.md is the master, and the Design System artifact is published from it at the end of UX.

## Test record

See [TESTS.md](TESTS.md).
