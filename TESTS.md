# Test record, 2026-11-01

Run on throwaway copies of a BMad 6.13.0-next install (core-tools, method, toolsmith), in Claude Code, headless (`claude -p`).

## Results

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

Tests 10–13 were run on a fresh BMad install from the `main` branch, with the module files taken straight from SETUP-PROMPT.md. In tests 12 and 13, `/bmad` also warned that the test repo had no application code to learn from. That's correct, since the test project was empty.

## No-npm install for Cursor

The commands in [install-bmad-cursor-without-npm.md](install-bmad-cursor-without-npm.md), run directly on macOS rather than through Cursor:

| # | Test | Result |
|---|---|---|
| 14 | Clone with `gh`, copy the skills to `.cursor/skills/`, run setup, delete the clone | ✅ 31 skills, no setup questions, `current: true`, no problems |
| 15 | The same with plain `git clone` | ✅ same result |

## Not tested

- bmad-ux actually creating a Claude Design canvas or publishing a Design System artifact. The tests show the instructions reach it, not that it carries them out.
- Harnesses other than Claude Code, including running the Cursor install prompt inside Cursor.
- The Cursor install on Windows.
- A UX Finalize that ends without the Design System publish. In `poc` and `product`, `/bmad` would then send you back to bmad-ux, which runs in Update mode. That's expected, but not tried.

## Side effects seen

- `bmad setup` deletes committed `__pycache__/*.pyc` files under `_bmad/scripts/` when it repairs BMad's shared scripts. That is BMad's own behaviour, not this prompt's.
