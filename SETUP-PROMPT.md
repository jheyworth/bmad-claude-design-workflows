# Setup prompt: Claude Design workflows for BMad

Paste everything below the line into Claude Code, opened in the project you want to set up. The project needs a stock BMad install first (`npx skills add bmad-code-org/BMAD-METHOD --skill '*' -y`, then `/bmad setup`).

---

Set up Claude Design workflows for BMad in this project: the workflow templates and the Claude Design steps. Do exactly the steps below, in order, and change nothing else.

## Rules for this run

- Work only inside this project folder.
- Never overwrite a file that already exists. If one of the `_bmad/custom/*.toml` files below exists, add the new entries to its existing lists and keep everything already in it. Show me the merged result before you save it.
- If `_bmad/` or `.claude/skills/bmad/` is missing, stop and tell me to install BMad first.
- At the end, show me the list of every file you created or changed.

## Step 1. Write the template module

Create `.claude/skills/bmod-claude-design-workflows/bmod.toml` with exactly this content:

```toml
[bmod]
code = "claude-design-workflows"
version = "0.3.0"
update_source = "file:.claude/skills"
skills = []
```

Create `.claude/skills/bmod-claude-design-workflows/help/help.md` with exactly the content between the two `~~~~` lines:

~~~~markdown
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
~~~~

## Step 2. Teach bmad-ux to use Claude Design

Create or merge `_bmad/custom/bmad-ux.toml` with these entries under `[workflow]`:

```toml
[workflow]
persistent_facts = [
  "Read the active initiative's `Workflow:` line. In `design-led`, the Claude Design System artifact owns the look: read its README and tokens, and copy them into DESIGN.md. In every other template, DESIGN.md owns the look and the Design System artifact is published from it at Finalize.",
]
design_handoffs = [
  "Claude Design: make a Design artifact in this session from the assembled prompt. Save its link and an exported copy to {doc_workspace}/imports/.",
]
external_handoffs = [
  "Unless the template is design-led, publish DESIGN.md as a Claude Design System artifact. Put the tokens in project/tokens.json in the list shape the type requires ({\"tokens\":[{name, value, usage}]}, never the nested DTCG map), Brand & Style and Do's and Don'ts in project/README.md, and Components in components/<Name>/. Write its link to design-system-<slug>/design-system-<slug>.md in the initiative's folder and show it to the user.",
  "Unless the template is design-led, after the Design System is published, put the promoted key-screen mockups (every HTML file in {doc_workspace}/mockups/) on one Claude Design canvas (the Design artifact type) built on that Design System, one board per mockup, named after the screen. Keep each screen's layout and strings as the mockup has them. Write the canvas link to the memlog and as a 'Canvas:' line in {doc_workspace}/ux-<slug>.md, and show it to the user. If mockups/ holds no HTML file, skip this and say so.",
]
```

## Step 3. Teach Sofia to keep the template line

Create or merge `_bmad/custom/bmad-agent-po.toml` with these entries under `[agent]`:

```toml
[agent]
persistent_facts = [
  "An initiative file may hold a line `Workflow: <id>` below its frontmatter. It names the initiative's workflow template. Keep it exactly as it is whenever you edit the file.",
  "When `design-system-<slug>/design-system-<slug>.md` exists in the initiative's folder, list it in the References of every ticket that builds UI.",
]
```

## Step 4. Register the module

Run `/bmad setup`, or this command if the skill cannot run:

```
uv run _bmad/scripts/setup.py --project-root . --skill .claude/skills/bmad --root .claude/skills
```

It must report `"problems": []` and `"claude-design-workflows"` with scripts `created` or `current`. If it reports anything else, show me the output and stop.

## Step 5. Check it

Run `uv run _bmad/scripts/knowledge.py --root .claude/skills` and confirm that `claude-design-workflows` appears in `documents`. Then tell me in one line which template to start with, and that `/bmad what's next?` will now follow it.
