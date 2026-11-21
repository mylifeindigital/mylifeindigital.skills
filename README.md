# mylifeindigital.skills

Versioned, reusable skills shared across my projects, for both Codex and
Claude Code.

## Available Skills

### `manage-issues`

Manages planned work as GitHub issues with the `gh` CLI, from a captured idea
through reconnaissance, decisions, board status, and closing with an outcome.
Defaults to the `idea`/`change`/`decision` labels and the
[My Life In Digital board](https://github.com/users/mylifeindigital/projects/4).
Project-local instructions override the bundled defaults. Board status changes
need `gh auth refresh -s project` once.

It replaces `manage-change-requests`. Old `change-requests/` folders are a
read-only archive; open CRs moved to issues that keep `CR-xxx` in the title.
If you symlinked the old skill, remove that link and link `manage-issues`.

## Install For Claude Code

Claude Code can consume this repository two ways. Both read the same `skills/`
directory that Codex uses, so nothing needs to be duplicated or restructured.

### Option A: Symlink (single machine)

Claude Code discovers personal skills under `~/.claude/skills`, using the same
symlink approach as the Codex setup below:

```bash
mkdir -p ~/.claude/skills
ln -s /absolute/path/to/mylifeindigital.skills/skills/manage-issues \
  ~/.claude/skills/manage-issues
```

Claude Code follows the symlink, so pulls and local edits apply immediately.
Restart Claude Code to pick up a newly linked skill.

### Option B: Plugin (any machine, versioned)

The repository root doubles as a Claude Code plugin and a single-plugin
marketplace, declared in `.claude-plugin/`. Inside any Claude Code session:

```
/plugin marketplace add mylifeindigital/mylifeindigital.skills
/plugin install mylifeindigital-skills@mylifeindigital
```

Every skill under `skills/` is discovered automatically; no per-skill
registration is required. Update with `/plugin update mylifeindigital-skills`
and restart to apply.

Use Option A while iterating locally, because it needs no reinstall. Use Option
B on machines that should track the published `main`.

## Install For Codex

Codex discovers personal skills under `~/.agents/skills`. Clone this repository,
then symlink each skill you want to make available globally:

```bash
mkdir -p ~/.agents/skills
ln -s /absolute/path/to/mylifeindigital.skills/skills/manage-issues \
  ~/.agents/skills/manage-issues
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
  skills/manage-issues
```

Adding a skill requires no manifest edit for Claude Code, but bump `version` in
`.claude-plugin/plugin.json` when publishing a change, and validate both
manifests:

```bash
claude plugin validate .
```
