# Test record, 2026-11-01

Run on throwaway copies of a BMad 6.13.0-next install (core-tools, method, toolsmith), in Claude Code, headless (`claude -p`).

## Results

Tests 1–23 ran while the module was called `bmod-sdlc` (code `sdlc`). It's now `bmod-claude-design-workflows`. Tests 1–24 installed it with the pasted setup prompt that tests 25–31 replace.

| # | Test | Result |
|---|---|---|
| 1 | `knowledge.py` finds the module's help | ✅ `sdlc` listed in `documents`, no problems |
| 2 | `setup.py --status` accepts the module | ✅ after adding `update_source = "file:.claude/skills"`. Without it, setup reports a problem: `bmod.update_source must be a non-empty string` |
| 3 | Full `bmad setup` | ✅ creates `_bmad/sdlc/scripts/`, `"problems": []` |
| 4 | `npx skills update -p -y` leaves the module alone | ✅ 16 BMad skills updated; `bmod-sdlc/` untouched |
| 5 | `/bmad what's next?` on `Workflow: poc` with the forge step done | ✅ recommended `bmad-ux`, Fast path, and the Design System publish at Finalize |
| 6 | Sofia keeps the `Workflow:` line when she writes the initiative's requirements | ✅ kept, even without the override. The override in the prompt makes it a rule rather than luck |
| 7 | The setup prompt itself, pasted headless into a fresh copy | ✅ all 5 steps done; the help file matches the prompt byte for byte; an existing `bmad-agent-po.toml` was merged, not overwritten |
| 8 | bmad-ux picks up the override | ✅ `resolve_customization.py` shows the added creative tool, handoff and fact |
| 9 | `/bmad what's next?` on `Workflow: design-led` with nothing done | ✅ recommended making the Design System first, not bmad-ux |
| 10 | `/bmad what's next?` on `Workflow: product` with forge and brief done | ✅ recommended `bmad-prd` |
| 11 | `/bmad what's next?` on `Workflow: product` with forge, brief and PRD done | ✅ recommended `bmad-ux`, Coaching path with Design canvases, and the Design System publish at Finalize |
| 12 | `/bmad what's next?` on `Workflow: existing` with nothing done | ✅ recommended `bmad-project-context` |
| 13 | `/bmad what's next?` on `Workflow: existing` with the BMad block in `AGENTS.md` | ✅ recommended building the Design System from the code's styles, not bmad-ux |

Tests 10–13 were run on a fresh BMad install from the `main` branch, with the module files taken straight from the setup prompt (SETUP-PROMPT.md, since replaced by `skills/`). In tests 12 and 13, `/bmad` also warned that the test repo had no application code to learn from. That's correct, since the test project was empty.

## bmad-ux and Claude Design

One headless `/bmad-ux` run (intent `update`, run Finalize) on a `Workflow: poc` initiative with the forge step done and a small draft DESIGN.md and EXPERIENCE.md. Creative tools were turned on in the request, because bmad-ux leaves them off when headless. The Reviewer Gate and the polish pass were skipped to keep the run short. It took 29 turns, about 8 minutes and $3.35.

| # | Test | Result |
|---|---|---|
| 16 | bmad-ux picks up the Claude Design overrides | ✅ `resolve_customization.py` lists the added creative tool, design handoff and external handoff |
| 17 | Finalize publishes DESIGN.md as a Design System artifact | ✅ published, and both spines set to `status: final`. `project/tokens.json` uses the list shape (`{"tokens":[…]}`, with `color.themes`). It holds 8 colours, 4 text styles, 4 spacing steps and 2 radii, with a README, 3 components and a cover. The link was written to `design-system-tally/design-system-tally.md` |
| 18 | Key screens are made on a Design canvas built on that system | ✅ a canvas with Home and Add bill. Its `canvas.json` names the Tally Design System artifact and the version it copied. An exported copy was saved under `.working/design-canvas/`, and HTML mocks were promoted to `mockups/` |
| 19 | `/bmad what's next?` afterwards | ✅ counted UX and the Design System as done and recommended `bmad-architecture`, Fast path |

bmad-ux also added a contrast table to DESIGN.md and logged its assumptions and five open questions in the memlog, as its Finalize step should.

## Design System from code (`existing`, step 2)

One headless `/bmad what's next? Then run that step.` on a `Workflow: existing` initiative, with the BMad block already in `AGENTS.md`. The code was [shadcn-ui/taxonomy](https://github.com/shadcn-ui/taxonomy) at `298a885`, a Next.js app with light and dark themes as CSS variables and 37 shadcn/ui components. The request gave the answers the Design System type would normally ask for: the repo and commit, Button, Card and Badge only, static previews, and the system's name. It didn't mention the link file. The checkout was in a separate folder, read but never run, and was unchanged afterwards. It took 29 turns, about 9 minutes and $5.95.

| # | Test | Result |
|---|---|---|
| 20 | `/bmad` runs step 2 and builds the Design System from the code | ✅ published, with all 19 colour tokens in both themes at their exact values. The repo's bare HSL values (`0 0% 100%`) were written as `hsl(0 0% 100%)`, which the page can read. Also the radius, 2 font families (Cal Sans's file copied), 7 text styles, a README with a "Not synced" note, and Button, Card and Badge with static previews. `meta` records `shadcn-ui/taxonomy`, `main@298a885`. It flagged 4 contrast failures in the repo's colours and kept the values as they are |
| 21 | The link file is written without being asked, and `/bmad` moves on | ✅ `design-system-taxonomy/design-system-taxonomy.md` written, then `/bmad what's next?` counted step 2 done and recommended `bmad-ux` |

The link file was named after the system (`taxonomy`), not the initiative (`billing`). `/bmad` still matched it, because the template only asks for `design-system-<slug>/`.

## Interactive run and the Finalize canvas

| # | Test | Result |
|---|---|---|
| 22 | A person runs the `product` template interactively: bmad-ux on the Coaching path, then architecture and Sofia (`ux-test`, 1–2 Nov, 7 sessions) | ✅ Design System published at Finalize (20 components), link file saved, and updated in place in a later session. `/bmad` routed on to architecture. The `Workflow:` line survived, and 4 of 5 epics cite the Design System. ❌ No Design canvas. bmad-ux showed 4 directions, colour themes and 12 key screens as local HTML pages and never used the canvas creative tool |
| 23 | Fix: the canvas becomes a second Finalize external handoff, run after the Design System publish. Tested headless on a copy of the test 22 project, with creative tools not asked for | ✅ One Design canvas, "Tally key screens", with 12 boards, one per accepted mockup, in journey order. Its `canvas.json` names the Tally Design System artifact. The link was written to the memlog and as a `Canvas:` line in `ux-tally.md`. The existing Design System was reused unchanged. 27 turns, about 5 minutes, $2.84 |

Test 23 found two limits of the canvas: the mockups' styles came across as page-wide rules, so the canvas's properties panel can only edit some of them, and board heights are estimates.

## Module renamed to `bmod-claude-design-workflows`

| # | Test | Result |
|---|---|---|
| 24 | The renamed prompt, pasted headless into a fresh BMad install, then `/bmad what's next?` on a `poc` initiative with the forge step done | ✅ setup `"problems": []` with `claude-design-workflows` scripts `created`; `knowledge.py` lists `claude-design-workflows`; routing recommended `bmad-ux`, Fast path. On an empty folder, the prompt stopped before step 1 and changed nothing, as its rules say |

## Installed as an npx module (2 Nov)

The module record and the setup skill were moved from the setup prompt into `skills/` and pushed to the `npx-module-test` branch. Each test ran in a fresh folder with a stock BMad install (6.13.0-next). The override text is the prompt's, byte for byte, with a `[claude-design-workflows]` prefix on each entry.

| # | Test | Result |
|---|---|---|
| 25 | `npx skills add 'jheyworth/bmad-claude-design-workflows#npx-module-test' --skill bmod-claude-design-workflows -y` | ✅ only the record installed, not the setup skill |
| 26 | `setup.py` | ✅ `created`, `"problems": []`, `current: true`, `absent_skills: []`; the `post_install_message` is shown |
| 27 | `knowledge.py` | ✅ `claude-design-workflows` listed in `documents` |
| 28 | `npx skills use '…#npx-module-test@claude-design-workflows-setup'` | ✅ printed the setup skill from the branch and installed nothing |
| 29 | That output run headless, with a line asking for setup | ✅ both `_bmad/custom/` files created; `resolve_customization.py` shows each entry once. 6 turns, $0.28 |
| 30 | A team entry added to each file, then the same run again | ✅ each entry still once, the team entries kept, and the files otherwise byte-identical to test 29's. 5 turns, $0.31 |
| 31 | Plain `npx skills add` from the branch, without `--skill` | ✅ no problems, but the setup skill is installed too and stays. The Quick start keeps `--skill` for this reason |

After merging to `main` and making the repo public:

| # | Test | Result |
|---|---|---|
| 32 | The Quick start as written, from `main`, in a fresh project | ✅ only the record installed, setup `"problems": []`, and the `skills use` pipe wrote each override once (8 turns, $0.32). While the repo was private, the version check got a 404 from `raw.githubusercontent.com`; once public, it reported `current` at 0.4.0 |

Before the branch, a hand-written record listing the setup skill as a member, with that skill not installed, gave `absent_skills: ["claude-design-workflows-setup"]` and `absent_install: "npx skills add jheyworth/bmad-claude-design-workflows --skill claude-design-workflows-setup"`. That's why the setup skill is kept out of the record's `skills` list.

## To run yourself

These need a person at the keyboard.

**Interactive bmad-ux on the Coaching path, with Design canvases.**

Takes 30–45 minutes, mostly waiting: about 5 for setup, 15–25 for the conversation, and about 10 for Finalize and publishing.

1. Make an empty folder, open Claude Code in it, and paste this.

   ```text
   Set up a throwaway test project in this folder for testing bmad-ux interactively. Do the setup, then tell me exactly what to type.

   1. Install BMad: clone https://github.com/bmad-code-org/BMAD-METHOD.git (depth 1) into .bmad-install-tmp, copy its skills/ folder contents into .claude/skills/, then run:
      uv run --no-cache .claude/skills/bmad/scripts/setup.py --project-root "$PWD" --skill "$PWD/.claude/skills/bmad" --root "$PWD/.claude/skills"
      Then delete .bmad-install-tmp.
   2. Install the templates: run `npx skills add jheyworth/bmad-claude-design-workflows --skill bmod-claude-design-workflows -y`, run the setup.py command above again, then run `npx skills use jheyworth/bmad-claude-design-workflows@claude-design-workflows-setup` and follow what it prints. Approval is given.
   3. Create a test initiative called "tally":
      - _bmad/custom/config.user.toml containing [core] and active_initiative = "tally"
      - _bmad-output/tally/tally.md with frontmatter title: Tally, then the line "Workflow: product", then: "Tally lets flatmates log shared bills and see who owes whom, settled monthly."
      - _bmad-output/tally/forge-tally/forge-tally.md, brief-tally/brief-tally.md and prd-tally/prd-tally.md, each with 5–10 realistic lines about that app (phone-first web app, add a bill, see balances, settle up at month end).
   4. Check setup reports no problems, then stop and tell me:
      "Setup done. Now start a NEW session in this folder and type: /bmad-ux lets create UX design — choose the Coaching path."
   ```

2. Start a new session in that folder and type `/bmad-ux lets create UX design`. Choose the **Coaching path**.
3. Answer its questions with anything reasonable: colours, fonts, one or two screens. Short answers keep it moving.
4. It shows design options as HTML pages in your browser. That's expected.
5. Let it Finalize, and say yes to publishing. It should publish the Design System, then put the accepted mockups on a Claude Design canvas.
6. Paste this to check the result:

   ```text
   Check this test: is there a Claude Design canvas link in _bmad-output/tally/ux-tally/.memlog.md and a Canvas: line in ux-tally.md, is there a design-system-tally/design-system-tally.md with a Design System link, and does /bmad what's next? now recommend bmad-architecture? Answer pass or fail for each.
   ```

   Three passes and it's done.

## Not tested

- `/bmad update` finding a new version of the module. Its version check reads `bmod.toml` from GitHub `main`, so on the branch it reports `could-not-check` (404).
- The `/bmad` agent itself on the new install: tests 25–31 called BMad's scripts directly, so they don't show `/bmad setup` relaying the `post_install_message`.
- A full template run with the module installed through npx. The help and override text are unchanged from tests 22–24.

- Building a Design System from code with built, live component previews. Test 20 used the read-only route.
- How the published Design System and canvas look on the page. Tests 17–18 checked the files, not the rendering.
- Harnesses other than Claude Code.
- A UX Finalize that ends without the Design System publish. In `poc` and `product`, `/bmad` would then send you back to bmad-ux, which runs in Update mode. That's expected, but not tried.

## Side effects seen

- `bmad setup` deletes committed `__pycache__/*.pyc` files under `_bmad/scripts/` when it repairs BMad's shared scripts. That is BMad's own behaviour, not this prompt's.
