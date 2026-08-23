# mylifeindigital.skills

Versioned, reusable Codex skills shared across my projects.

## Available Skills

### `manage-change-requests`

Manages repository-local Markdown change requests from initial backlog note
through reconnaissance, decisions, implementation tracking, and completion.
Project-local instructions and templates override the bundled defaults.

## Install For Codex

Codex discovers personal skills under `~/.agents/skills`. Clone this repository,
then symlink each skill you want to make available globally:

```bash
mkdir -p ~/.agents/skills
ln -s /absolute/path/to/mylifeindigital.skills/skills/manage-change-requests \
  ~/.agents/skills/manage-change-requests
```

Codex follows the symlink to this repository, so pulls and local edits become
available without copying the skill. Restart Codex if a newly installed skill
does not appear immediately.

## Develop A Skill

Create the repository-local validation environment once:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements-dev.txt
```

Follow `AGENTS.md`, keep each skill self-contained under `skills/`, and validate
it before committing:

```bash
.venv/bin/python "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" \
  skills/manage-change-requests
```
