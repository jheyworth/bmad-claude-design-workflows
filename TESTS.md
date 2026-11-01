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

## Not tested

- bmad-ux actually creating a Claude Design canvas or publishing a Design System artifact. The tests show the instructions reach it, not that it carries them out.
- The `existing` and `product` templates' routing (same mechanism as tests 5 and 9).
- Harnesses other than Claude Code.

## Side effects seen

- `bmad setup` deletes committed `__pycache__/*.pyc` files under `_bmad/scripts/` when it repairs BMad's shared scripts. That is BMad's own behaviour, not this prompt's.
