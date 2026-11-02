# Claude Design workflows for BMad

*Workflow templates that take a BMad project from idea to build, with the look published to Claude Design.*

**One BMad module turns a standard BMad install into a guided product workflow.** You pick a template, `/bmad what's next?` walks you through it step by step, and the product's look is published to Claude Design for every later step to build on.

## Which Claude plans it works on

| Your Claude plan | Templates and `/bmad` routing | Design published to Claude Design |
|---|---|---|
| **Pro, Max, Ultra** | ✅ | ✅ |
| **Team, Enterprise** | ✅ | ✅ unless your admin has turned artifacts off |
| **Claude API** (an API key, a company gateway, Amazon Bedrock, Google Cloud or Microsoft Foundry) | ✅ | ❌ |

**❌ means you still get the whole workflow, without the publishing.** The design lives in DESIGN.md and local HTML mockups, and every later step uses those. The `design-led` template is the exception: it starts in Claude Design, so it needs a ✅ plan.

**Two catches on Pro to Enterprise:** sign in to Claude Code with your Claude account, not an API key, and check that your organization hasn't switched artifacts off. Some Enterprise setups can't have them at all. [Full details](#claude-plans-and-claude-design).

## Quick start

0. **Check your plan** in [Which Claude plans it works on](#which-claude-plans-it-works-on). On a Claude API plan, everything below still works, but nothing is published to Claude Design.
1. **Install BMad** in your project: `npx skills add bmad-code-org/BMAD-METHOD --skill '*' -y`, then `/bmad setup`.
2. **Install the templates:** `npx skills add jheyworth/bmad-claude-design-workflows --skill bmod-claude-design-workflows -y`, then `/bmad setup` again.
   - Keep `--skill bmod-claude-design-workflows`. Without it, the setup skill from step 3 is installed too and stays installed.
3. **Add the Claude Design steps** to bmad-ux and Sofia. This runs the setup skill straight from GitHub, without installing it:

   ```
   { npx skills use jheyworth/bmad-claude-design-workflows@claude-design-workflows-setup; echo "Set up Claude Design workflows in this project. Approval to save is given."; } | claude -p --permission-mode acceptEdits --allowedTools "Bash(uv run:*)" "Bash(ls:*)" "Bash(cat:*)"
   ```

   Run it again whenever the module's version changes. It replaces its own earlier entries and keeps everything else.
4. **Start an initiative** with `/bmad`, and add a line below its frontmatter naming a template, for example `Workflow: poc`. The templates are in [Pick a template](#pick-a-template).
5. **Ask `/bmad what's next?`** whenever you finish a step.

**Maintainers:** see [Working on this](#working-on-this) at the bottom.

## The problem it solves

- **Standard BMad leaves the route to you**
  - **`/bmad what's next?` suggests a step** from the skills you have installed, using its own judgement each time.
  - **Nothing fixes the order**, so two projects of the same kind can take different routes.
- **The design stays on your machine**
  - **bmad-ux writes DESIGN.md and EXPERIENCE.md** and keeps its mockups as local HTML files.
  - **Nothing is published** for tickets, builds or decks to point to.

## Pick a template

Each initiative names one template on a line in its initiative file, for example `Workflow: poc`.

| Template | Use it when | The flow |
|---|---|---|
| `poc` | You're testing one idea alone, fast | forge idea → UX (fast) → Design System → architecture (fast) → tickets → build → retro → Slides |
| `product` | Several people must agree; several epics | forge idea → brief or PRFAQ → PRD → UX → Design System → architecture → epics and stories → build + code review → retro |
| `existing` | You're adding to code that already exists | project context → Design System from code → UX → architecture → tickets → build + code review |
| `design-led` | The look comes first | Design System → Design canvas → UX → architecture → tickets → build → Slides |

Each template's plain-English summary and step table are in [help/help.md](skills/bmod-claude-design-workflows/help/help.md).

## What happens on a project

Using `product` as the example:

1. **You start an initiative** and add `Workflow: product` to its file.
2. **You ask `/bmad what's next?`** after every step.
   - **It reads the template** and checks which steps' output files already exist.
   - **It recommends the first missing step**, says why, and offers to run it.
3. **You work through the early steps**: forge the idea, write the brief, then the PRD.
4. **bmad-ux designs the look and behaviour with you.**
   - **While you design**, it shows options as local HTML pages in your browser.
   - **At Finalize**, it publishes DESIGN.md as a **Claude Design System**.
   - **Then it puts every accepted mockup on one Claude Design canvas** built on that system.
   - **Both links are saved in the project**, where later steps find them.
5. **Architecture, then tickets.** Sofia lists the Design System in every ticket that builds UI.
6. **Build and review.** Each story is built against the same Design System.

```
 forge → brief → PRD → bmad-ux ──Finalize──▶ Design System ──▶ Design canvas
                                                  │
                         ┌────────────────────────┼─────────────────────┐
                         ▼                        ▼                     ▼
                   architecture          tickets cite it         Slides deck
                                          (Sofia)              (poc, design-led)
```

## Claude Design at each step

**Claude Design here means three Claude artifact types, made from inside Claude Code.** Each is a claude.ai page, private to you until you share it. The standalone Claude Design app, which closes on 14 December 2026, isn't used.

- **The three artifact types**
  - **Design System:** tokens (colours, type, spacing, radius), components with live previews, and a brand README. It's the look every later step builds on.
  - **Design:** a canvas of screens. It holds the accepted key screens.
  - **Slides:** a 16:9 deck. It's the demo deck at the end of `poc` and `design-led`.
- **Where the Design System comes from, per template**
  - **`poc` and `product`:** bmad-ux publishes it from DESIGN.md at Finalize, then makes the canvas.
  - **`existing`:** Claude builds it first, from the code's real styles and components, so new screens match old ones. bmad-ux makes the canvas at Finalize.
  - **`design-led`:** you make the Design System and a canvas in Claude first. bmad-ux then imports them.
- **Who owns the look**
  - **`design-led`:** the Design System is the master, and DESIGN.md is copied from it.
  - **Every other template:** DESIGN.md is the master, and the Design System is published from it.
- **Where the links live**
  - **Design System:** `design-system-<slug>/design-system-<slug>.md` in the initiative's folder. `/bmad` counts that step as done once the file exists.
  - **Canvas:** a `Canvas:` line in `ux-<slug>/ux-<slug>.md`, and the memlog.
- **Some steps are Claude, not a BMad skill**
  - **Which ones:** the Design System from code (`existing`), the first Design System and canvas (`design-led`), and the Slides deck.
  - **How they run:** `/bmad` routes you to them like any other step. Claude makes the artifact and saves its link where the step says.

## Claude plans and Claude Design

The Claude Design steps publish claude.ai artifacts. Artifacts are built into Claude Code, so there's nothing to install or connect. They're on when all of these hold ([Claude Code artifacts docs](https://code.claude.com/docs/en/artifacts)):

- **Your plan** is Pro, Max, Ultra, Team or Enterprise.
- **You sign in to Claude Code with your Claude account.** Run `/login` and choose your claude.ai account, usually through your company's single sign-on on Team and Enterprise. Claude Code signed in with an API key, a gateway token, or Amazon Bedrock, Google Cloud or Microsoft Foundry credentials can't publish, even if your company also has an Enterprise plan.
- **On Team and Enterprise, your organization allows them.**
  - An Owner hasn't turned artifacts off in [Organization settings → Artifacts](https://claude.ai/admin-settings/artifacts). They're on by default.
  - The organization doesn't use customer-managed encryption keys (CMEK), HIPAA or Zero Data Retention. Artifacts aren't available with any of these.
  - Claude Code can reach `api.anthropic.com` to load the organization's policy. If it can't, artifacts are off.
- **Claude Code is v2.1.281 or later.** Check with `claude --version`, upgrade with `claude update`.
- **Artifacts aren't switched off on your machine.** The **Artifacts** row in `/config` is on, and neither `CLAUDE_CODE_DISABLE_ARTIFACT` nor `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` is set.
- **If artifacts are missing in a script, run the design steps in a normal Claude Code session.** The docs say artifacts are off by default under `claude -p` and the Agent SDK, though they were on in this project's `claude -p` tests.

**What happens without artifacts**

- **bmad-ux works as usual.** It writes DESIGN.md and EXPERIENCE.md and shows its mockups as local HTML pages.
- **Each Claude Design step writes its link file with a note instead of a link:** `Not published: Claude artifacts aren't available in this session. DESIGN.md is the source of the look.` That counts as done, so `/bmad what's next?` moves on to architecture.
- **Tickets cite DESIGN.md** instead of the Design System. The canvas and the Slides deck are skipped.
- **You can publish later.** On a plan with artifacts, run the step again and the note is replaced with a link.
- **`design-led` isn't available.** It starts in Claude Design, so `/bmad` suggests `product` or `poc` instead.

## What it changes in your project

- **One module is installed** with `npx skills add`, like any other BMad module.
  - **`bmod-claude-design-workflows/`** (`SKILL.md`, `bmod.toml` and `help/help.md`): the templates. A module record with no skills and no code.
- **Two override files are written** by the setup skill. It never overwrites a file. Where one exists, it adds its entries to it, and on a second run it replaces only its own.
  - **`_bmad/custom/bmad-ux.toml`:** has bmad-ux publish the Design System and canvas at Finalize, and adds Claude Design as a handoff option alongside Google Stitch.
  - **`_bmad/custom/bmad-agent-po.toml`:** has Sofia keep the `Workflow:` line and list the Design System in UI tickets.
  - **Every entry starts with `[claude-design-workflows]`**, so you can see which lines came from this module.
- **Everything else stays the same**
  - **No BMad skill is edited**, and BMad updates don't remove the templates.
  - **`/bmad update` updates the templates** along with BMad. It doesn't touch the override files, so re-run step 3 of [Quick start](#quick-start) when the module's version changes.
  - **bmad-ux still shows options as local HTML pages** while you design.

## What's been tested

- **Install:** `npx skills add` from this repo registers the module, and the setup skill run with `npx skills use` writes each override once, even when run twice.
- **Routing:** `/bmad what's next?` picks the right step in all four templates.
- **A full interactive run:** a person took `product` through UX, architecture and tickets. The Design System was published and the epics cite it.
- **The canvas at Finalize:** all 12 accepted mockups went onto one canvas built on the Design System.
- **Design System from code:** built from a real public codebase for `existing`, in both themes, at the code's exact values.
- **Details, and what's not tested yet:** [TESTS.md](TESTS.md).

---

# Working on this

Everything you need to change the templates and test them again.

## How it works

```
/bmad what's next?
   │
   ├─ knowledge.py reads every module's help/help.md ── finds the module's templates
   ├─ reads the active initiative file ──────────────── finds "Workflow: poc"
   ├─ lists the initiative's folders ────────────────── forge-x/ exists, ux-x/ does not
   └─ recommends the first step whose output is missing ─ "bmad-ux, Fast path"
                                                              │
                                    bmad-ux reads _bmad/custom/bmad-ux.toml
                                    ── at Finalize: Design System publish, then canvas
```

- **The templates are a help document, not code.** `/bmad` reads them and reasons over the tables. Change the wording and you change its behaviour.
- **"Done when" drives everything.** `/bmad` works out where you are only from which output files exist. Each step's "Done when" must name a file or folder the skill really writes.
- **The other skills are steered by override files.** bmad-ux and Sofia read `_bmad/custom/<skill>.toml` when they start. In those files, lists are added to the defaults and single values replace them.
- **Required steps go in `external_handoffs`, not `creative_tools`.** bmad-ux always runs its Finalize handoffs, but in a live session it always picked its own HTML tools over an added creative tool (test 22).

## Why a module and not only customisations

`/bmad` has no customisation file, so bmad-customize can't change how it picks the next step. Its rules say to take routes only from module help and to treat every other file as evidence, not instructions. A module record with only a help file is the smallest thing it will follow.

The overrides can't ship inside the module. A skill's customisation comes from exactly three files: its own `customize.toml`, then `_bmad/custom/<skill>.toml`, then `_bmad/custom/<skill>.user.toml`. `/bmad setup` doesn't write customisations, so a setup skill writes them into the project's `_bmad/custom/`.

## Where the source of truth lives

**The [skills/](skills/) folder is the only source.** Projects install from it on GitHub `main`. Don't edit an installed copy in a project and forget to bring the change back here.

| To change | Edit |
|---|---|
| A template, a step, or what "what's next" does | [skills/bmod-claude-design-workflows/help/help.md](skills/bmod-claude-design-workflows/help/help.md) |
| How bmad-ux uses Claude Design | The `bmad-ux.toml` block in [skills/claude-design-workflows-setup/SKILL.md](skills/claude-design-workflows-setup/SKILL.md) |
| How Sofia treats the `Workflow:` line or Design System links | The `bmad-agent-po.toml` block in the same file |
| Another skill's behaviour | Add a section for a new `_bmad/custom/<skill>.toml` to the same file |

On every release, bump `version` in [skills/bmod-claude-design-workflows/bmod.toml](skills/bmod-claude-design-workflows/bmod.toml). `/bmad update` only sees a new release when that number changes. It reads the number from GitHub `main`, which can take about 5 minutes to show a push, and only while the repo is public.

## Rules to keep

- **Keep `update_source` in `bmod.toml`** pointing at this repo's `skills/` folder. Without it, `bmad setup` reports a problem, and `/bmad update` can't find new versions.
- **Keep the setup skill out of the record's `skills` list** and without a `bmod.toml` of its own. It's run with `npx skills use`, not installed, and listing it would make `/bmad setup` offer to install it.
- **Start every override entry with `[claude-design-workflows]`.** The setup skill removes entries with that prefix before adding its own, which is what keeps a second run from adding them twice.
- **Never name the module after a retired BMad skill.** Setup deletes any skill listed in a module's `retired.toml`. `bmod-claude-design-workflows` is safe.
- **Only use output names the skills really write.** Check the skill's own files before adding a step. For example, bmad-retrospective writes `epic-<slug>-retrospective.md` in the epic folder.
- **Keep the setup skill merge-safe.** It must add to existing `_bmad/custom/*.toml` files, never replace them.
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

1. **Push the change to a branch**, then make a fresh BMad install in a temporary folder (`npx skills add bmad-code-org/BMAD-METHOD -y`). Run `git init` and commit, so you can see what changes.
2. **Install from the branch:** `npx skills add 'jheyworth/bmad-claude-design-workflows#<branch>' --skill bmod-claude-design-workflows -y`, then run [Quick start](#quick-start) step 3 with `#<branch>` after the repo name, before `@`.
3. **Check setup:** `uv run _bmad/scripts/setup.py --project-root . --skill .claude/skills/bmad --root .claude/skills --status` must show `"problems": []`.
4. **Check the overrides:** `uv run _bmad/scripts/resolve_customization.py --skill .claude/skills/bmad-ux --project-root . --key workflow` must show each `[claude-design-workflows]` entry once. Run step 2's setup command again and check it still does.
5. **Check the help is found:** `uv run _bmad/scripts/knowledge.py --root .claude/skills` must list `claude-design-workflows`.
6. **Check the routing:**
   - Make `_bmad-output/initiative-<slug>/initiative-<slug>.md` with frontmatter `type: initiative`, `title` and `parent: none`, then a `Workflow:` line below it. That's the name and folder `/bmad` uses when it creates an initiative.
   - Set `active_initiative = "initiative-<slug>"` under `[core]` in `_bmad/custom/config.user.toml`. It names the folder.
   - Add the outputs of the steps you want to count as done, inside that folder.
   - Run `claude -p "/bmad what's next? Recommend only; do not run anything."` and check it names the right step.
7. **Record the result** in [TESTS.md](TESTS.md).

## Open items

- **Don't use `/design-sync` for the `existing` template's step 2.** It syncs to standalone Claude Design projects (claude.ai/design), which close by 14 December 2026. Use the route tests 20–21 used instead: Claude reads the code and publishes a Design System artifact directly.
