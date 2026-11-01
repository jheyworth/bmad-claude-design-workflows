# Install BMad in Cursor without npm

Use this when you can't run the npm installer (`npx skills add` / `npx bmad-method install`).
It clones the BMad repo, copies its skills into `.cursor/skills/`, runs BMad setup, then deletes the clone.

**You need:** [uv](https://docs.astral.sh/uv/) and git. You don't need Node or npm.

**How to use:** open your project folder in Cursor, start an agent chat, and paste the prompt below.

**What you get:** the v7 preview alpha from the repo's default branch (`main`), not the stable v6.12 release.

**To update later:** run the same prompt again, then ask `/bmad` to run `bmad setup`. It will offer to remove any skills BMad has retired.

**Using Claude Code too?** Cursor also reads `.claude/skills/`. Don't install BMad into both folders, or Cursor will see two copies of every skill.

## Prompt

```text
Install the BMad Method skills into this project for Cursor, without npm, by cloning the
BMad repo, copying its skills, running BMad setup, then deleting the clone.
Run every command from this project's root folder. On Windows, use the PowerShell
equivalents of these commands.

1. Prerequisites
   - Check `uv --version`. If uv is missing, stop and tell me to install it from
     https://docs.astral.sh/uv/ — BMad setup cannot run without it.
   - Check `git --version`. If git is missing, stop and tell me.

2. Clone BMad into a temporary folder inside this project:
     gh repo clone bmad-code-org/BMAD-METHOD .bmad-install-tmp -- --depth 1
   If gh is not installed or not logged in, use instead:
     git clone --depth 1 https://github.com/bmad-code-org/BMAD-METHOD.git .bmad-install-tmp

3. Copy every skill from the clone into Cursor's project skills folder:
     mkdir -p .cursor/skills
     cp -R .bmad-install-tmp/skills/. .cursor/skills/
   Confirm .cursor/skills/bmad/SKILL.md exists.

4. Run BMad setup. The base command is:
     uv run --no-cache .cursor/skills/bmad/scripts/setup.py --project-root "$PWD" --skill "$PWD/.cursor/skills/bmad" --root "$PWD/.cursor/skills"
   a. Run it with --list-config-questions added. If it prints any questions, ask me each one,
      show its default, and pass my answers the way .cursor/skills/bmad/references/setup.md
      describes (a --module-answers file, deleted afterwards).
   b. Run the base command with no extra flag. It creates the _bmad/ folder.
   c. If it prints "error:" anywhere, stop and show me the error. Do not delete anything.

5. Delete the clone, and only the clone:
     rm -rf .bmad-install-tmp

6. Verify: run the base command with --status added. Confirm "current" is true and
   "problems" is empty. Then tell me:
   - how many skills are in .cursor/skills
   - any post_install_message from step 4b, quoted as written
   - that I can start with /bmad in a new Cursor chat

Rules: do not run npx or npm. Do not delete or change anything outside .bmad-install-tmp,
.cursor/skills and _bmad/.
```

## Tested

Tested on 2026-11-01 on macOS, by running the prompt's commands directly rather than through Cursor. Clone, copy, setup, deleting the clone and the status check all worked: 31 skills installed, setup asked no questions, and the status check reported `current: true` with no problems.

The plain `git clone` fallback was tested the same way and gave the same result.

Not yet tested:
- Running the prompt inside Cursor, and Cursor loading the skills.
- Windows.
