# Claude Code Skill Setup

This directory makes the root Agent Skills available to Claude Code as project skills.

Claude Code discovers project skills from:

- `.claude/skills/<skill-name>/SKILL.md` in the current project.
- `~/.claude/skills/<skill-name>/SKILL.md` for personal skills.
- `.claude/skills/` inside a directory passed with `--add-dir`.

The entries in `.claude/skills/` are symlinks to the canonical root skill folders:

- `deep-documentation`
- `truthful-coder`

To use this repository as a shared skill source from another project:

```bash
claude --add-dir /path/to/agent-skills
```

If your environment does not preserve symlinks, copy the root skill folder into `.claude/skills/<skill-name>/` or `~/.claude/skills/<skill-name>/`.
