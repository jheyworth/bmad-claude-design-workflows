---
name: claude-design-workflows-setup
description: Adds the Claude Design steps of the claude-design-workflows module to bmad-ux and Sofia (bmad-agent-po) as project customizations. Use when the user says "set up Claude Design workflows" or the module's install message asks for it.
---

# Claude Design workflows setup

Write this module's overrides of two BMad skills into the project's `_bmad/custom/` files. Change nothing else.

## Rules

- Work only inside the project folder: the nearest folder containing `_bmad/`. If there is none, or `bmod-claude-design-workflows` is not installed beside the `bmad` skill, stop and say to run `npx skills add jheyworth/bmad-claude-design-workflows --skill bmod-claude-design-workflows -y` and `/bmad setup` first.
- Every entry this skill owns is a string that starts with `[claude-design-workflows]`. In each file below, first remove every string in the named lists that starts with that prefix, then add the entries below. That makes a second run replace the first instead of adding to it.
- Keep every other entry, table and comment in the file exactly as it is. If the file does not exist, create it with only the table shown.
- Show the user each merged file before saving it.
- At the end, list every file created or changed, then run `uv run _bmad/scripts/resolve_customization.py --skill <skills-folder>/bmad-ux --project-root . --key workflow` and confirm each entry appears exactly once.

## `_bmad/custom/bmad-ux.toml`, under `[workflow]`

```toml
[workflow]
persistent_facts = [
  "[claude-design-workflows] Read the active initiative's `Workflow:` line. In `design-led`, the Claude Design System artifact owns the look: read its README and tokens, and copy them into DESIGN.md. In every other template, DESIGN.md owns the look and the Design System artifact is published from it at Finalize.",
  "[claude-design-workflows] If you have no tool for publishing claude.ai artifacts in this session, or publishing is refused, don't attempt any Claude Design step. Where a step would write a Design System link, write `design-system-<slug>/design-system-<slug>.md` in the initiative's folder holding only the line `Not published: Claude artifacts aren't available in this session. DESIGN.md is the source of the look.` Skip the canvas. Tell the user in one line that the Claude Design steps were skipped and that DESIGN.md and the local mockups are the design.",
]
design_handoffs = [
  "[claude-design-workflows] Claude Design (only when this session can publish claude.ai artifacts): make a Design artifact in this session from the assembled prompt. Save its link and an exported copy to {doc_workspace}/imports/.",
]
external_handoffs = [
  "[claude-design-workflows] Unless the template is design-led, publish DESIGN.md as a Claude Design System artifact. Put the tokens in project/tokens.json in the list shape the type requires ({\"tokens\":[{name, value, usage}]}, never the nested DTCG map), Brand & Style and Do's and Don'ts in project/README.md, and Components in components/<Name>/. Write its link to design-system-<slug>/design-system-<slug>.md in the initiative's folder and show it to the user.",
  "[claude-design-workflows] Unless the template is design-led, after the Design System is published, put the promoted key-screen mockups (every HTML file in {doc_workspace}/mockups/) on one Claude Design canvas (the Design artifact type) built on that Design System, one board per mockup, named after the screen. Keep each screen's layout and strings as the mockup has them. Write the canvas link to the memlog and as a 'Canvas:' line in {doc_workspace}/ux-<slug>.md, and show it to the user. If mockups/ holds no HTML file, skip this and say so.",
]
```

## `_bmad/custom/bmad-agent-po.toml`, under `[agent]`

```toml
[agent]
persistent_facts = [
  "[claude-design-workflows] An initiative file may hold a line `Workflow: <id>` below its frontmatter. It names the initiative's workflow template. Keep it exactly as it is whenever you edit the file.",
  "[claude-design-workflows] When `design-system-<slug>/design-system-<slug>.md` exists in the initiative's folder, list it in the References of every ticket that builds UI. If it holds a `Not published:` line instead of a link, list `ux-<slug>/DESIGN.md` instead.",
]
```
