# BMad product workflow templates, with Claude Design

**One prompt turns a stock BMad install into a guided product workflow.** You choose a template for your initiative, and from then on `/bmad what's next?` tells you the next step and offers to run it. Claude Design artifacts (Design System, Design canvas, Slides) are built into the steps.

## How Claude Design is built into the workflows

**Claude Design here means the new version: three Claude artifact types, made from inside Claude Code.** It doesn't use the standalone Claude Design app, which closes on 14 December 2026. Each artifact is a claude.ai page, private to you until you share it.

| Artifact type | What it holds | What the templates use it for |
|---|---|---|
| **Design System** | Tokens (colours, type, spacing, radius), components with live previews, and a brand README | The look of the product. Every later screen, ticket and deck is built on it |
| **Design** | A canvas of screens and mockups | Design directions and key screens during UX. In `design-led`, the first screens |
| **Slides** | A 16:9 deck | The demo deck at the end of `poc` and `design-led` |

### What each template does with it

| Template | Where the Design System comes from | Design canvases | Deck at the end |
|---|---|---|---|
| `poc` | bmad-ux (Fast path) writes DESIGN.md, then publishes it as a Design System at UX Finalize | When bmad-ux shows directions | Yes |
| `product` | bmad-ux (Coaching path) writes DESIGN.md, then publishes it as a Design System at UX Finalize | Yes, as part of the Coaching path | No |
| `existing` | Built first, from the code's real styles and components, so new screens match old ones. bmad-ux then designs the new work on it | When bmad-ux shows directions | No |
| `design-led` | Made in Claude first, then key screens on a Design canvas. bmad-ux imports the canvas and copies the system into DESIGN.md | Yes, before UX | Yes |

### How the pieces connect

```
                 ┌─ poc, product: bmad-ux publishes DESIGN.md at UX Finalize
                 ├─ existing:     built from the code's styles
                 └─ design-led:   made in Claude first, copied into DESIGN.md
                          │
                          ▼
        Design System artifact ── link saved in design-system-<slug>/design-system-<slug>.md
                          │
       ┌──────────────────┼────────────────────┐
       ▼                  ▼                    ▼
 Design canvases    UI tickets cite it    Slides deck
 (bmad-ux)          (Sofia)               (poc, design-led)
```

- **One link, kept in one file.** The Design System's link lives in `design-system-<slug>/design-system-<slug>.md` in the initiative's folder. `/bmad` counts that step as done once the file exists, and every later step reads the link from there.
- **bmad-ux uses Claude Design without being asked.** The setup prompt adds Claude Design to bmad-ux's creative tools and handoffs. bmad-ux shows design directions on Design canvases built on the Design System, and saves each link and an exported copy. Outside `design-led`, it publishes DESIGN.md as the Design System at Finalize.
- **Sofia keeps builds on the same look.** Every ticket that builds UI lists the Design System file in its References.
- **Some steps are Claude itself, not a BMad skill.** Those are building the Design System from code (`existing`), the Design System and first canvas (`design-led`), and the Slides deck (`poc`, `design-led`). `/bmad` routes you to them like any other step. Claude makes the artifact and saves its link to the file the step names.

### Who owns the look

- **`design-led`:** the Claude Design System artifact is the master, and DESIGN.md is copied from it.
- **Every other template:** DESIGN.md is the master, and the Design System artifact is published from it at the end of UX.

### What you need, and what's been tested

- **The claude.ai artifacts connector, available in Claude Code.** Without it, `/bmad` still routes you to the design steps, but nothing can make the artifacts.
- **Tested:** `/bmad` routing to the design steps.
- **Not tested yet:** bmad-ux actually making a canvas and publishing a Design System, and building a Design System from code. See [Open items](#open-items).

## The four templates

| Template | Use it when | The flow |
|---|---|---|
| `poc` | You're testing one idea alone, fast | forge idea → UX (fast) → Design System → architecture (fast) → tickets → build → retro → Slides |
| `product` | Several people must agree; several epics | forge idea → brief or PRFAQ → PRD → UX → Design System → architecture → epics and stories → build + code review → retro |
| `existing` | You're adding to code that already exists | project context → Design System from code → UX → architecture → tickets → build + code review |
| `design-led` | The look comes first | Design System → Design canvas → UX → architecture → tickets → build → Slides |

Each template, with its plain-English summary and step table, is in [SETUP-PROMPT.md](SETUP-PROMPT.md).

## How to use it

1. **Install BMad** in your project: `npx skills add bmad-code-org/BMAD-METHOD --skill '*' -y`, then `/bmad setup`. The Claude Design steps also need Claude's artifact tools (the claude.ai artifacts connector) available in Claude Code. Without them, `/bmad` still routes you to those steps, but they can't make anything.
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

## Test record

See [TESTS.md](TESTS.md).

---

# Working on this

Everything you need to change the templates and test them again.

## How it works

```
/bmad what's next?
   │
   ├─ knowledge.py reads every module's help/help.md ── finds bmod-sdlc's templates
   ├─ reads the active initiative file ──────────────── finds "Workflow: poc"
   ├─ lists the initiative's folders ────────────────── forge-x/ exists, ux-x/ does not
   └─ recommends the first step whose output is missing ─ "bmad-ux, Fast path"
                                                              │
                                    bmad-ux reads _bmad/custom/bmad-ux.toml
                                    ── Claude Design canvas, Design System publish
```

- **The templates are a help document, not code.** `/bmad` reads them and reasons over the tables. Change the wording and you change its behaviour.
- **"Done when" drives everything.** `/bmad` works out where you are only from which output files exist. Each step's "Done when" must name a file or folder the skill really writes.
- **The other skills are steered by override files.** bmad-ux and Sofia read `_bmad/custom/<skill>.toml` when they start. In those files, lists are added to the defaults and single values replace them.

## Where the source of truth lives

**[SETUP-PROMPT.md](SETUP-PROMPT.md) is the only source.** The module files and override files are written out from it. Don't edit an installed copy in a project and forget to bring the change back here.

| To change | Edit this part of SETUP-PROMPT.md |
|---|---|
| A template, a step, or what "what's next" does | Step 1, between the `~~~~` lines (it becomes `help/help.md`) |
| How bmad-ux uses Claude Design | Step 2 (`bmad-ux.toml`) |
| How Sofia treats the `Workflow:` line or Design System links | Step 3 (`bmad-agent-po.toml`) |
| Another skill's behaviour | Add a step with a new `_bmad/custom/<skill>.toml` |

When you change the templates, bump `version` in the step 1 `bmod.toml`.

## Rules to keep

- **Keep `update_source` in `bmod.toml`.** Without it, `bmad setup` reports a problem.
- **Never name the module after a retired BMad skill.** Setup deletes any skill listed in a module's `retired.toml`. `bmod-sdlc` is safe.
- **Only use output names the skills really write.** Check the skill's own files before adding a step. For example, bmad-retrospective writes `epic-<slug>-retrospective.md` in the epic folder.
- **Keep the prompt merge-safe.** It must add to existing `_bmad/custom/*.toml` files, never replace them.
- **Write Design System tokens as a list.** The Design System type only reads tokens in the shape `{"tokens":[{name, value, usage}]}`. A nested token map shows up empty.

## BMad files worth knowing

Paths are inside a BMad install (`.claude/skills/` or `.agents/skills/`).

| File | Why it matters |
|---|---|
| `bmad/SKILL.md`, `bmad/references/help.md` | How `/bmad` answers: routes only from module help |
| `bmad/scripts/knowledge.py` | What counts as a module record and a help document |
| `_bmad/scripts/setup.py` | What setup checks: `update_source`, the scripts folder, retired names |
| `bmod-*/retired.toml` | Skill names setup will delete |
| `bmad-ux/SKILL.md`, `bmad-ux/customize.toml` | bmad-ux's plug-in points: `creative_tools`, `design_handoffs`, `external_handoffs` |
| `bmad-agent-po/customize.toml` | Sofia's plug-in points (`[agent].persistent_facts`) |

## How to test a change

Always test on a throwaway copy, never in a real project.

1. **Make a copy of a BMad install** in a temporary folder, then run `git init` and commit, so you can see what changes.
2. **Run the prompt headless:** `claude -p "$(awk 'f;/^---$/{f=1}' SETUP-PROMPT.md)" --allowedTools "Bash(uv run:*)" "Bash(ls:*)" "Bash(mkdir:*)" Read Glob Grep Write Edit`. Add a line saying approval is given, since a headless run can't ask you.
3. **Check setup:** `uv run _bmad/scripts/setup.py --project-root . --skill .claude/skills/bmad --root .claude/skills --status` must show `"problems": []`.
4. **Check the help is found:** `uv run _bmad/scripts/knowledge.py --root .claude/skills` must list `sdlc`.
5. **Check the routing:**
   - Set `active_initiative` in `_bmad/custom/config.user.toml`.
   - Make `_bmad-output/<initiative>/<initiative>.md` with a `Workflow:` line.
   - Add the outputs of the steps you want to count as done.
   - Run `claude -p "/bmad what's next? Recommend only; do not run anything."` and check it names the right step.
6. **Record the result** in [TESTS.md](TESTS.md).

## Open items

- **Not tested:** bmad-ux actually making a Claude Design canvas and publishing a Design System. The routing works, but the artifact side hasn't been run.
- **Not tested:** the `existing` and `product` templates' routing.
- **Not tested:** the `existing` template's step 2, building a Design System from code. The Design System type's instructions describe a route from a code repo through `/design-sync` in Claude Code. Whether that keeps working after standalone Claude Design closes on 14 December 2026 is unknown.
- **Idea:** offer the templates upstream to BMAD-METHOD as a module, once they've been used on a real project.
