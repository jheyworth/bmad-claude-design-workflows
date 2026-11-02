# Claude Design workflows for BMad

*See your product's design, not just read about it. Claude Design built into BMad's UX process, with a guided, step-by-step workflow.*

**Claude Design workflows is an add-on for [BMad](https://github.com/bmad-code-org/BMAD-METHOD).** You install it into a project that already has BMad, like any other BMad module.

## Why use it

- **You see the design, not just read it**
  - **Standard BMad** ends UX with written documents: DESIGN.md and EXPERIENCE.md.
  - **With Claude Design workflows**, bmad-ux (BMad's UX skill) publishes the look as a **Claude Design System**: colours, type, spacing and components, with live previews.
  - **The key screens** go on a **Claude Design canvas**, built on that system.
  - **Made for visual people:** product owners, designers and stakeholders can look at it, comment on it and share it.
- **Claude Design is the design tool**
  - **It runs inside Claude Code.** No Google Stitch or other outside tool needed.
  - **In `design-led`**, you start in Claude Design and BMad builds from it.
- **One design, used everywhere**
  - **Tickets:** Sofia (BMad's product-owner agent) cites the Design System in every UI ticket.
  - **Builds:** each story is built against it.
  - **Demo decks:** Claude Slides decks are built on it.
- **A guided workflow:** `/bmad what's next?` tells you the next step, every time, in a fixed order.
- **Four templates**, one per kind of project:
  - **`poc`:** test one idea, fast.
  - **`product`:** a full product several people must agree on.
  - **`existing`:** add to code that already exists. The Design System is built from the code's real styles.
  - **`design-led`:** the look comes first.

## Will it work for you?

| Your Claude plan | Templates and routing | Published to Claude Design |
|---|---|---|
| **Pro, Max, Ultra** | ✅ | ✅ |
| **Team, Enterprise** | ✅ | ✅ unless your admin turned artifacts off |
| **Claude API** (API key, gateway, Bedrock, Google Cloud, Foundry) | ✅ | ❌ |

- **❌ still gets the whole workflow**
  - The design stays in DESIGN.md and local mockups.
  - Every later step uses those instead. [How that works](#if-you-cant-publish-artifacts)
  - **Except `design-led`:** it starts in Claude Design, so it needs ✅.
- **✅ has two catches**
  - **Sign in with your Claude account** (`/login`), not an API key.
  - **Team and Enterprise:** an admin can turn artifacts off. [All conditions](#claude-design-explained)

## Install

- **You need:** Node.js (for `npx`) and [uv](https://docs.astral.sh/uv/) (BMad's setup runs on it).

1. **Install BMad**, then run `/bmad setup` in Claude Code:

   ```
   npx skills add bmad-code-org/BMAD-METHOD -y
   ```

2. **Install Claude Design workflows** (the templates and the Claude Design steps), then run `/bmad setup` again:

   ```
   npx skills add jheyworth/bmad-claude-design-workflows -y
   ```

   - **Setup offers to add the Claude Design steps** to bmad-ux and Sofia.
   - **Say yes.** It shows the two files first and asks before saving.

3. **Start your project.** In Claude Code, type `/bmad start a new initiative called <name>`.
   - **An initiative** is BMad's word for one piece of work, such as a new app or a feature.
4. **Ask `/bmad what's next?`**
   - **The first time**, it describes the four templates and asks which fits. Pick one and it records it for you.
   - **After that**, it tells you the next step every time you ask.

**Changing the add-on itself?** See [For maintainers](#for-maintainers-changing-claude-design-workflows).

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
   - **When it finishes**, it publishes DESIGN.md as a **Claude Design System**.
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

## Claude Design, explained

- **What it is:** three Claude artifact types, made from inside Claude Code.
  - Each is a claude.ai page, private to you until you share it.
  - The standalone Claude Design app (closing 14 December 2026) isn't used.
- **The three types**
  - **Design System:** tokens (colours, type, spacing, radius), components and a brand README. The look every later step builds on.
  - **Design:** a canvas of the key screens.
  - **Slides:** a 16:9 demo deck, at the end of `poc` and `design-led`.
- **Who owns the look**
  - **`design-led`:** the Design System. You make it in Claude first; DESIGN.md is copied from it.
  - **Every other template:** DESIGN.md. The Design System is published from it.
  - **In `existing`**, the Design System is built first, from the code's real styles, so new screens match old ones.
- **Where the links live**
  - **Design System:** `design-system-<slug>/design-system-<slug>.md` in the initiative's folder. `/bmad` counts the step done once it exists.
  - **Canvas:** a `Canvas:` line in `ux-<slug>/ux-<slug>.md`.
- **Some steps are Claude, not a BMad skill:** the Design System from code, the first Design System in `design-led`, and the Slides deck. `/bmad` routes you to them like any other step.

### When you can publish

Artifacts are built into Claude Code: nothing to install. They're on when all of these hold ([docs](https://code.claude.com/docs/en/artifacts)):

- **Plan:** Pro, Max, Ultra, Team or Enterprise.
- **Sign-in:** your Claude account via `/login`, usually your company's single sign-on.
  - **Can't publish:** an API key, a gateway token, or Bedrock, Google Cloud or Foundry credentials, even if your company has an Enterprise plan.
- **Team and Enterprise:** your organization allows them.
  - An Owner hasn't turned them off in [Organization settings → Artifacts](https://claude.ai/admin-settings/artifacts). On by default.
  - No customer-managed encryption keys (CMEK), HIPAA or Zero Data Retention.
  - Claude Code can reach `api.anthropic.com` to load the policy.
- **Version:** Claude Code v2.1.281 or later (`claude --version`, `claude update`).
- **Not switched off locally**
  - The **Artifacts** row in `/config` is on.
  - Neither `CLAUDE_CODE_DISABLE_ARTIFACT` nor `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` is set.
- **Session:** run the design steps in a normal Claude Code session.

## If you can't publish artifacts

- **bmad-ux works as usual.** It writes DESIGN.md and EXPERIENCE.md and shows its mockups as local HTML pages.
- **Each Claude Design step writes its link file with a note instead of a link:** `Not published: Claude artifacts aren't available in this session. DESIGN.md is the source of the look.` That counts as done, so `/bmad what's next?` moves on to architecture.
- **Tickets cite DESIGN.md** instead of the Design System. The canvas and the Slides deck are skipped.
- **You can publish later.** On a plan with artifacts, run the step again and the note is replaced with a link.
- **`design-led` isn't available.** It starts in Claude Design, so `/bmad` suggests `product` or `poc` instead.

## What it adds to your project

- **One module**, installed with `npx skills add` like any other BMad module.
  - **`bmod-claude-design-workflows/`** holds the templates (`help/help.md`), the version (`bmod.toml`) and the Claude Design steps (`migration-1.toml`). It has no skills and no code, and doesn't appear in the `/` menu.
- **Two override files**, written when you say yes during `/bmad setup`. Neither replaces a file: where one exists, its entries are added to it, and a later run replaces only its own.
  - **`_bmad/custom/bmad-ux.toml`:** has bmad-ux publish the Design System and canvas when it finishes, and adds Claude Design as a handoff option alongside Google Stitch.
  - **`_bmad/custom/bmad-agent-po.toml`:** has Sofia keep the `Workflow:` line and list the Design System in UI tickets.
  - **Every entry starts with `[claude-design-workflows]`**, so you can see which lines came from this module.
- **Everything else stays the same**
  - **No BMad skill is edited**, and BMad updates don't remove the templates.
  - **`/bmad update` updates the templates** along with BMad. If the Claude Design steps have changed, it offers them again; say yes.
  - **bmad-ux still shows options as local HTML pages** while you design.

---

# For maintainers: changing Claude Design workflows

You only need this part if you're changing the templates or the Claude Design steps in this repo. Using them doesn't need any of it.

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

## Why a module, and why a migration

`/bmad` has no customisation file, so bmad-customize can't change how it picks the next step. Its rules say to take routes only from module help and to treat every other file as evidence, not instructions. A module record with only a help file is the smallest thing it will follow.

A module can't set another skill's customisation directly. A skill's customisation comes from exactly three files: its own `customize.toml`, then `_bmad/custom/<skill>.toml`, then `_bmad/custom/<skill>.user.toml`. So the module ships a BMad migration (`migration-1.toml`) that writes its entries into `_bmad/custom/`. `/bmad setup` and `/bmad update` check each installed module's migrations, offer the ones whose `detect` matches the project, and on a yes follow its `guide` and `checklist`. Ours matches when the `[claude-design-workflows]` entries are missing or differ from the file's.

The record's `SKILL.md` sets `user-invocable: false` and `disable-model-invocation: true`, so it stays out of the `/` menu and Claude never loads it as a skill. BMad only checks that the file exists.

## Where the source of truth lives

**The [skills/](skills/) folder is the only source.** Projects install from it on GitHub `main`. Don't edit an installed copy in a project and forget to bring the change back here.

| To change | Edit |
|---|---|
| A template, a step, or what "what's next" does | [skills/bmod-claude-design-workflows/help/help.md](skills/bmod-claude-design-workflows/help/help.md) |
| How bmad-ux uses Claude Design | `bmad_ux` in [skills/bmod-claude-design-workflows/migration-1.toml](skills/bmod-claude-design-workflows/migration-1.toml) |
| How Sofia treats the `Workflow:` line or Design System links | `bmad_agent_po` in the same file |
| Another skill's behaviour | Add a block for a new `_bmad/custom/<skill>.toml` to the same file, and name it in `detect`, `guide` and `checklist` |

On every release, bump `version` in [skills/bmod-claude-design-workflows/bmod.toml](skills/bmod-claude-design-workflows/bmod.toml). `/bmad update` only sees a new release when that number changes. It reads the number from GitHub `main`, which can take about 5 minutes to show a push, and only while the repo is public.

## Rules to keep

- **Keep `update_source` in `bmod.toml`** pointing at this repo's `skills/` folder. Without it, `bmad setup` reports a problem, and `/bmad update` can't find new versions.
- **Keep `migration-1.toml` valid.** `knowledge.py` only lists a migration with `module`, `from`, `to`, `title`, `summary`, `detect`, `guide` and a non-empty `checklist`, and `module` must be `claude-design-workflows`. Change the blocks in place; existing projects are offered it again because `detect` compares their entries with the file's.
- **Start every override entry with `[claude-design-workflows]`.** The migration removes entries with that prefix before adding its own, which is what keeps a second run from adding them twice.
- **Never name the module after a retired BMad skill.** Setup deletes any skill listed in a module's `retired.toml`. `bmod-claude-design-workflows` is safe.
- **Only use output names the skills really write.** Check the skill's own files before adding a step. For example, bmad-retrospective writes `epic-<slug>-retrospective.md` in the epic folder.
- **Keep the migration merge-safe.** It must add to existing `_bmad/custom/*.toml` files, never replace them.
- **Write Design System tokens as a list.** The Design System type only reads tokens in the shape `{"tokens":[{name, value, usage}]}`. A nested token map shows up empty.

## BMad files worth knowing

Paths are inside a BMad install (`.claude/skills/` or `.agents/skills/`).

| File | Why it matters |
|---|---|
| `bmad/SKILL.md`, `bmad/references/help.md` | How `/bmad` answers: routes only from module help |
| `bmad/scripts/knowledge.py` | What counts as a module record, a help document and a migration |
| `bmad/references/migrate.md` | How `/bmad` runs a module's migration |
| `_bmad/scripts/setup.py` | What setup checks: `update_source`, the scripts folder, retired names |
| `bmod-*/retired.toml` | Skill names setup will delete |
| `bmad-ux/SKILL.md`, `bmad-ux/customize.toml` | bmad-ux's plug-in points: `creative_tools`, `design_handoffs`, `external_handoffs` |
| `bmad-agent-po/customize.toml` | Sofia's plug-in points (`[agent].persistent_facts`) |

## How to test a change

Always test on a throwaway copy, never in a real project.

1. **Push the change to a branch**, then make a fresh BMad install in a temporary folder (`npx skills add bmad-code-org/BMAD-METHOD -y`). Run `git init` and commit, so you can see what changes.
2. **Install from the branch:** `npx skills add 'jheyworth/bmad-claude-design-workflows#<branch>' -y`. Then run `claude -p "/bmad setup" --output-format json`, take `session_id` from its output, and answer its offer with `claude -p "yes" --resume <session_id>`, and the same for its save question.
3. **Check setup:** `uv run _bmad/scripts/setup.py --project-root . --skill .claude/skills/bmad --root .claude/skills --status` must show `"problems": []`.
4. **Check the overrides:** `uv run _bmad/scripts/resolve_customization.py --skill .claude/skills/bmad-ux --project-root . --key workflow` must show each `[claude-design-workflows]` entry once. Run `/bmad setup` again and check it offers nothing.
5. **Check the help is found:** `uv run _bmad/scripts/knowledge.py --root .claude/skills` must list `claude-design-workflows`.
6. **Check the routing:**
   - Make `_bmad-output/initiative-<slug>/initiative-<slug>.md` with frontmatter `type: initiative`, `title` and `parent: none`, then a `Workflow:` line below it. That's the name and folder `/bmad` uses when it creates an initiative.
   - Set `active_initiative = "initiative-<slug>"` under `[core]` in `_bmad/custom/config.user.toml`. It names the folder.
   - Add the outputs of the steps you want to count as done, inside that folder.
   - Run `claude -p "/bmad what's next? Recommend only; do not run anything."` and check it names the right step.
7. **Record the result** in [TESTS.md](TESTS.md).

## Open items

- **Don't use `/design-sync` for the `existing` template's step 2.** It syncs to standalone Claude Design projects (claude.ai/design), which close by 14 December 2026. Use the route tests 20–21 used instead: Claude reads the code and publishes a Design System artifact directly.
