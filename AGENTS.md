# AGENTS.md

This repository is the versioned source for reusable skills, shared across
Codex and Claude Code.

## Repository Structure

- Put each skill in `skills/<skill-name>/`.
- Require `SKILL.md` and keep the folder name equal to the frontmatter `name`.
- Add only the `assets/`, `references/`, or `scripts/` directories a skill uses.
- Put UI metadata in `agents/openai.yaml`.
- Keep the Claude Code plugin and marketplace manifests in `.claude-plugin/`
  at the repository root; they expose every skill under `skills/` and need no
  per-skill entry.
- Keep repository-level installation and contribution guidance in `README.md`;
  do not add auxiliary README or changelog files inside skill directories.

## Skill Changes

- Use the `skill-creator` workflow when creating or materially updating a skill.
- Keep skills project-agnostic unless their name and description explicitly
  define a project scope.
- Prefer project-local `AGENTS.md`, workflows, and templates over hardcoded
  assumptions in a reusable skill.
- Keep `SKILL.md` concise and use imperative instructions.
- Prefer instructions over scripts until deterministic automation is justified.
- Treat bundled assets as defaults; do not overwrite project-owned files unless
  the user explicitly requests it.

## Validation

Run the skill validator after every skill change:

```bash
.venv/bin/python "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" \
  skills/<skill-name>
```

If `.venv` does not exist, create it with the development setup documented in
`README.md`.

Validate the Claude Code manifests after changing anything in `.claude-plugin/`,
and bump `version` in `plugin.json` when publishing a change:

```bash
claude plugin validate .
```

Test material workflow changes against a temporary fixture or disposable
repository before using them on a live backlog.

## Git

- Use `skill/<skill-name>` branches for skill changes and `repo/<topic>`
  branches for repository infrastructure.
- Keep one reusable workflow outcome per branch.
- Commit validated skill changes at meaningful checkpoints.
