# Claude Design workflows for BMad

*Four guided BMad workflows, with your design published to Claude Design.*

## TL;DR

- **What's different from standard bmad-ux**
  - **Standard:** DESIGN.md, EXPERIENCE.md and HTML mockups on your machine. Nothing is published.
  - **Claude Design workflows:** the same, then published to Claude Design as a Design System and a canvas of the key screens.
  - **Result:** a design others can see and comment on, and every later step uses it. [Full comparison](#standard-bmad-ux-vs-claude-design-workflows)
- **Claude Design System**
  - Your design is published as a Claude Design System when bmad-ux finishes.
  - Colours, type, spacing and components, with live previews.
  - A real page you can see, not a markdown file.
  - Stakeholders can look at it, comment on it and share it.
  - Tickets, builds and demo decks all use it.
- **Claude Design canvas**
  - The key screens, on one canvas.
  - Built on the Design System.
- **Four workflows**
  - **poc:** test one idea, fast, on your own.
  - **product:** a full product several people must agree on.
  - **existing:** add to code that already exists. The Design System is built from your code.
  - **design-led:** start with the look. Design in Claude Design first.
- **Guided**
  - Ask `/bmad what's next?` after each step.
  - `/bmad` names the next step in your workflow, says why, and offers to run it.
  - You never have to work out the order yourself.
- **A new BMad module**
  - **Workflows:** the four workflows, run step by step by `/bmad what's next?`.
  - **Overrides:** customisation overrides for bmad-ux and Sofia, in `_bmad/custom/`, the same files bmad-customize writes.
  - **No BMad skill is edited.** Two install commands, then `/bmad setup`. See [Install](#install).
- **Why it's here**
  - A working preview of how this could fit into BMad Method.
  - Ahead of a possible PR.

## Standard bmad-ux vs Claude Design workflows

- **Standard bmad-ux**
  - Writes DESIGN.md and EXPERIENCE.md.
  - Shows mockups as HTML pages on your machine.
  - Nothing is published. Others only see the design if you send them files.
- **With Claude Design workflows**
  - The same files and mockups while you design.
  - Then the design is published as a Claude Design System, plus a canvas of the key screens.
  - Anyone you share it with can see it and comment on it.
  - Every later step points to that one Design System.
  - All inside Claude Code. No Google Stitch or other outside design tool needed.

## Will it work for you?

| Your Claude subscription | Workflows and `/bmad what's next?` | Publishing to Claude Design |
|---|---|---|
| **Pro, Max, Ultra** | ✅ | ✅ |
| **Team, Enterprise** | ✅ | ✅ unless your admin turned it off |
| **Claude API** (API key, company gateway, Bedrock, Google Cloud, Foundry) | ✅ | ❌ |

- **On the Claude API**
  - You get every workflow except `design-led`.
  - The design stays in DESIGN.md and local HTML mockups.
  - See [If you can't publish](#if-you-cant-publish).
- **On Pro to Enterprise**
  - Sign in to Claude Code with your Claude account (`/login`), not an API key.
  - On Team and Enterprise, check your admin hasn't turned publishing off. See [When you can publish](#when-you-can-publish).

## Install

- **You need**
  - Claude Code, signed in with your Claude account.
  - Node.js, for the `npx` command. [Download Node.js](https://nodejs.org)
  - uv, which BMad's setup uses. [Install uv](https://docs.astral.sh/uv/getting-started/installation/)
- **In a terminal, in your project's folder**

  ```
  npx skills add bmad-code-org/BMAD-METHOD -y
  npx skills add jheyworth/bmad-claude-design-workflows -y
  ```

- **In Claude Code**
  1. Type `/bmad setup`.
     - It offers to add the Claude Design steps to bmad-ux and Sofia.
     - Say yes. It shows you the two override files first, and asks before saving them.
  2. Type `/bmad start a new initiative called <name>`.
     - An initiative is BMad's word for one piece of work, such as a new app or a feature.
  3. Type `/bmad what's next?`
     - The first time, `/bmad` describes the four workflows and asks which fits.
     - Pick one. `/bmad` writes it into your initiative's file as `Workflow: <name>`.
     - From then on, `/bmad what's next?` gives you the next step.

## The four workflows

| Workflow | Use it when | The steps |
|---|---|---|
| `poc` | You're testing one idea alone, fast | forge idea → UX (quick version) → Design System → architecture (quick version) → tickets → build → retrospective → Slides deck |
| `product` | Several people must agree; several epics | forge idea → brief or PRFAQ (a press release plus FAQ) → PRD → UX → Design System → architecture → epics and stories → build + code review → retrospective |
| `existing` | You're adding to code that already exists | project context → Design System from code → UX → architecture → tickets → build + code review |
| `design-led` | The look comes first | Design System → Design canvas → UX → architecture → tickets → build → Slides deck |

- **Changing workflow**
  - Edit the `Workflow:` line in your initiative's file.
- **Full step tables**
  - In [help/help.md](skills/bmod-claude-design-workflows/help/help.md).

## A project, step by step

Using the `product` workflow as the example.

1. **Start**
   - Start an initiative, and pick `product` when `/bmad` asks.
2. **Ask what's next, after every step**
   - `/bmad` checks which steps' output files already exist.
   - It recommends the first missing step, says why, and offers to run it.
3. **Early steps**
   - Forge the idea, write the brief, then the PRD.
4. **UX with bmad-ux**
   - While you design, bmad-ux shows options as HTML pages in your browser.
   - When it finishes, it publishes DESIGN.md as a Claude Design System.
   - Then it puts every accepted screen on one Claude Design canvas.
   - Both links are saved in the project, for later steps.
5. **Architecture, then tickets**
   - Sofia, BMad's product-owner agent, lists the Design System in every ticket that builds UI.
6. **Build and review**
   - Each story is built to match the Design System.

```
 forge → brief → PRD → bmad-ux ──when it finishes──▶ Design System ──▶ Design canvas
                                                          │
                             ┌────────────────────────────┼─────────────────────┐
                             ▼                            ▼                     ▼
                       architecture              tickets cite it         Slides deck
                                                  (Sofia)              (poc, design-led)
```

## Claude Design, explained

- **What it is**
  - Three kinds of claude.ai page ("artifacts"), made from inside Claude Code.
  - Private to you until you share them.
  - The standalone Claude Design app (closing 14 December 2026) isn't used.
- **Design System**
  - Colours, type, spacing, radius, components and a brand README.
  - The look every later step builds on.
- **Design canvas**
  - The key screens.
- **Slides**
  - A 16:9 demo deck, in the `poc` and `design-led` workflows.
- **Who owns the look**
  - `design-led`: the Design System. You make it first; DESIGN.md is copied from it.
  - Every other workflow: DESIGN.md. The Design System is published from it.
  - `existing`: the Design System is built first, from your code's styles.
- **Where the links are saved**
  - Design System: `design-system-<slug>/design-system-<slug>.md` in the initiative's folder. `<slug>` is your initiative's short name.
  - Canvas: a `Canvas:` line in `ux-<slug>/ux-<slug>.md`.
  - `/bmad` counts the Design System step as done once its file exists.
- **Steps Claude does itself, without a BMad skill**
  - The Design System from code, the first Design System in `design-led`, and the Slides deck.
  - `/bmad` routes you to them like any other step.

### When you can publish

All of these must be true ([Claude Code artifacts docs](https://code.claude.com/docs/en/artifacts)):

- **Subscription**
  - Pro, Max, Ultra, Team or Enterprise.
- **Sign-in**
  - Your Claude account, via `/login`. On Team and Enterprise, usually your company's single sign-on.
  - An API key, gateway token, or Bedrock, Google Cloud or Foundry login can't publish, even if your company has Enterprise.
- **Team and Enterprise settings**
  - An Owner hasn't turned artifacts off in [Organization settings → Artifacts](https://claude.ai/admin-settings/artifacts). They're on by default.
  - Your organisation doesn't use customer-managed encryption keys (CMEK), a HIPAA setup or Zero Data Retention.
  - Claude Code can reach `api.anthropic.com`.
- **Claude Code version**
  - v2.1.281 or later. Check with `claude --version`, upgrade with `claude update`.
- **Not switched off on your machine**
  - The **Artifacts** row in `/config` is on.
  - `CLAUDE_CODE_DISABLE_ARTIFACT` and `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` aren't set.
- **Session**
  - An interactive Claude Code session, not a `claude -p` script.

## If you can't publish

- **bmad-ux**
  - Works as usual: DESIGN.md, EXPERIENCE.md and HTML mockups.
- **Claude Design steps**
  - Each one writes its link file with a note instead of a link: `Not published: Claude artifacts aren't available in this session. DESIGN.md is the source of the look.`
  - That counts as done, so `/bmad what's next?` moves on.
- **Tickets**
  - Point to DESIGN.md instead of the Design System.
- **Canvas and Slides deck**
  - Skipped.
- **Publishing later**
  - On a subscription that can publish, run the step again. The note is replaced with a link.
- **`design-led`**
  - Not available, because it starts in Claude Design. `/bmad` suggests `product` or `poc` instead.

## What it adds to your project

- **One BMad module**
  - `bmod-claude-design-workflows`: the four workflows. No skills, no code, not in the `/` menu.
- **Two customisation override files**, only if you say yes during `/bmad setup`
  - `_bmad/custom/bmad-ux.toml`: bmad-ux publishes the Design System and canvas when it finishes.
  - `_bmad/custom/bmad-agent-po.toml`: Sofia keeps the `Workflow:` line and cites the Design System in UI tickets.
- **No BMad skill is edited**
  - `/bmad update` keeps the module up to date along with BMad.

---

# For maintainers: changing Claude Design workflows

Only needed if you're changing this repo. Using it doesn't need any of this.

## What to edit

- **The source**
  - The [skills/](skills/) folder. Projects install it from GitHub `main`.
  - Don't edit an installed copy and forget to bring the change back here.

| To change | Edit |
|---|---|
| A workflow, a step, or what `/bmad what's next?` does | [skills/bmod-claude-design-workflows/help/help.md](skills/bmod-claude-design-workflows/help/help.md) |
| How bmad-ux uses Claude Design | `bmad_ux` in [skills/bmod-claude-design-workflows/migration-1.toml](skills/bmod-claude-design-workflows/migration-1.toml) |
| How Sofia treats the `Workflow:` line or Design System links | `bmad_agent_po` in the same file |
| Another skill's behaviour | Add a block for a new `_bmad/custom/<skill>.toml` to the same file, and name it in `detect`, `guide` and `checklist` |

- **Releasing**
  - Bump `version` in [skills/bmod-claude-design-workflows/bmod.toml](skills/bmod-claude-design-workflows/bmod.toml) every time.
  - `/bmad update` only sees a new release when that number changes.
  - It reads the number from GitHub `main`, so the repo must stay public. A push can take about 5 minutes to show.

## How it works

```
/bmad what's next?
   │
   ├─ knowledge.py reads every module's help/help.md ── finds the four workflows
   ├─ reads the active initiative's file ───────────── finds "Workflow: poc"
   ├─ lists the initiative's folders ────────────────── forge-x/ exists, ux-x/ does not
   └─ recommends the first step whose output is missing ─ "bmad-ux, Fast path"
                                                              │
                                    bmad-ux reads _bmad/custom/bmad-ux.toml
                                    ── at Finalize: Design System publish, then canvas
```

- **The workflows are a help document, not code**
  - `/bmad` reads them and reasons over the tables. Change the wording and you change its behaviour.
- **"Done when" drives everything**
  - `/bmad` works out where you are only from which output files exist.
  - Each step's "Done when" must name a file or folder the skill really writes.
- **Override files steer the other skills**
  - bmad-ux and Sofia read `_bmad/custom/<skill>.toml` when they start.
  - Lists are added to the defaults; single values replace them.
- **Required steps go in `external_handoffs`, not `creative_tools`**
  - bmad-ux always runs its Finalize handoffs (the steps at the end of UX).
  - In a live session it picked its own HTML tools over an added creative tool. See test 22 in [TESTS.md](TESTS.md).
- **"Fast path"**
  - bmad-ux's shorter route. The `poc` workflow uses it.

## Why a module, and why a migration

- **Why a module**
  - `/bmad` has no customisation file, so bmad-customize can't change how it picks the next step.
  - It takes routes only from module help. A module with only a help file is the smallest thing it follows.
- **Why a migration**
  - A module can't set another skill's customisation directly. Customisation comes only from the skill's own `customize.toml`, then `_bmad/custom/<skill>.toml`, then `_bmad/custom/<skill>.user.toml`.
  - So the module ships `migration-1.toml`, which writes its entries into `_bmad/custom/`.
  - `/bmad setup` and `/bmad update` offer it whenever its `detect` matches: the `[claude-design-workflows]` entries are missing, or differ from those in the file.
  - On a yes, `/bmad` follows its `guide`, then runs its `checklist`.
- **Why it's not in the `/` menu**
  - The module's `SKILL.md` sets `user-invocable: false` and `disable-model-invocation: true`. BMad only checks that the file exists.

## Rules to keep

- **Keep `update_source` in `bmod.toml`**
  - Pointing at this repo's `skills/` folder. Without it, `/bmad setup` reports a problem and `/bmad update` can't find new versions.
- **Keep `migration-1.toml` valid**
  - It needs `module`, `from`, `to`, `title`, `summary`, `detect`, `guide` and a non-empty `checklist`, and `module` must be `claude-design-workflows`.
  - Change the blocks in place. Projects are offered the migration again because `detect` compares their entries with the file's.
- **Prefix every settings entry with `[claude-design-workflows]`**
  - The migration removes entries with that prefix, then adds its own, and keeps everything else.
  - That stops a second run adding them twice, or replacing a project's own entries.
- **Never name the module after a retired BMad skill**
  - Setup deletes any skill listed in a module's `retired.toml`. `bmod-claude-design-workflows` is safe.
- **Only use output names the skills really write**
  - Check the skill's own files first. For example, bmad-retrospective writes `epic-<slug>-retrospective.md` in the epic folder.
- **Write Design System tokens as a list**
  - The Design System type only reads `{"tokens":[{name, value, usage}]}`. A nested token map shows up empty.

## BMad files worth knowing

Paths are inside a BMad install (`.claude/skills/` or `.agents/skills/`).

| File | Why it matters |
|---|---|
| `bmad/SKILL.md`, `bmad/references/help.md` | How `/bmad` answers: routes only from module help |
| `bmad/scripts/knowledge.py` | What counts as a module, a help document and a migration |
| `bmad/references/migrate.md` | How `/bmad` runs a module's migration |
| `_bmad/scripts/setup.py` | What setup checks: `update_source`, the scripts folder, retired names |
| `bmod-*/retired.toml` | Skill names setup will delete |
| `bmad-ux/SKILL.md`, `bmad-ux/customize.toml` | bmad-ux's plug-in points: `creative_tools`, `design_handoffs`, `external_handoffs` |
| `bmad-agent-po/customize.toml` | Sofia's plug-in points (`[agent].persistent_facts`) |

## How to test a change

Always on a throwaway copy, never in a real project.

1. **Fresh BMad install**
   - Push the change to a branch.
   - In a temporary folder: `npx skills add bmad-code-org/BMAD-METHOD -y`, then `git init` and commit, so you can see what changes.
2. **Install from the branch and run setup**
   - `npx skills add 'jheyworth/bmad-claude-design-workflows#<branch>' -y`
   - `claude -p "/bmad setup" --output-format json`, and copy `session_id` from the output.
   - Answer the offer: `claude -p "yes" --resume <session_id>`. Answer the save question the same way.
3. **Check setup**
   - `uv run _bmad/scripts/setup.py --project-root . --skill .claude/skills/bmad --root .claude/skills --status` shows `"problems": []`.
4. **Check the override files**
   - `uv run _bmad/scripts/resolve_customization.py --skill .claude/skills/bmad-ux --project-root . --key workflow` shows each `[claude-design-workflows]` entry once.
   - Run `/bmad setup` again. It should offer nothing.
5. **Check the help is found**
   - `uv run _bmad/scripts/knowledge.py --root .claude/skills` lists `claude-design-workflows`.
6. **Check the routing**
   - Make `_bmad-output/initiative-<slug>/initiative-<slug>.md` with frontmatter `type: initiative`, `title` and `parent: none`, then a `Workflow:` line below it.
   - Set `active_initiative = "initiative-<slug>"` under `[core]` in `_bmad/custom/config.user.toml`.
   - Add the outputs of the steps you want to count as done, inside that folder.
   - Run `claude -p "/bmad what's next? Recommend only; do not run anything."` and check it names the right step.
7. **Record the result**
   - In [TESTS.md](TESTS.md).

## Open items

- **Don't use `/design-sync` for the `existing` workflow's Design System step**
  - It syncs to standalone Claude Design projects (claude.ai/design), which close by 14 December 2026.
  - Instead, Claude reads the code and publishes a Design System artifact directly, as in tests 20–21 in [TESTS.md](TESTS.md).
