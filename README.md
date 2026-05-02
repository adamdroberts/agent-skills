# agent-skills

A collection of [Agent Skills](https://docs.cursor.com/context/skills) and compatible agent packs. Each canonical skill lives in its own root folder with a `SKILL.md` entry point.

## Skills

| Skill | Path | Summary |
|--------|------|---------|
| **Deep Documentation** | [deep-documentation/SKILL.md](deep-documentation/SKILL.md) | Create, expand, reorganize, and maintain repository documentation with deep coverage: READMEs, browsable docs, API/reference pages, `llms.txt` / `llms-full.txt`, changelogs, and repo-local agent skills. Use when you want comprehensive docs, LLM-friendly indexes, documentation that stays in sync with code, or guidance for future models on how to use the project. |
| **Truthful Coder** | [truthful-coder/SKILL.md](truthful-coder/SKILL.md) | Enforces strict change transparency while editing code, notebooks, configs, or running commands: announce intent, track what actually changed, and disclose every delta (including formatters, lockfiles, and incidental edits). Use when you want no “hidden” repo or environment changes. |

## Agent support

| Agent | Repo-local support | Notes |
|-------|--------------------|-------|
| Codex | [.agents/skills/](.agents/skills/) | Symlinks to the canonical root skill folders. Codex discovers repository skills from `.agents/skills` and follows symlinked skill folders. |
| Claude Code | [.claude/skills/](.claude/skills/) (project), [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json) (remote) | Symlinks load each skill as a project skill. The marketplace publishes both skills as installable plugins under `adamdroberts-skills`. |
| Gemini CLI | [.gemini/agents/](.gemini/agents/) | Gemini does not natively load `SKILL.md` Agent Skills. The checked-in Gemini agents are compatibility wrappers that read and follow the matching root skill. |

## Install remotely (Claude Code plugin marketplace)

This repo is also published as a Claude Code plugin marketplace named `adamdroberts-skills`. From inside Claude Code:

```text
/plugin marketplace add adamdroberts/agent-skills
/plugin install deep-documentation@adamdroberts-skills
/plugin install truthful-coder@adamdroberts-skills
```

The marketplace manifest lives at [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json) and each skill ships its own [.claude-plugin/plugin.json](deep-documentation/.claude-plugin/plugin.json). Updates ride on git commits — re-run `/plugin marketplace update adamdroberts-skills` to pull the latest.

## Use the repo directly

Clone the repository, then point your agent at the repo-local config:

```bash
git clone <this-repo-url> agent-skills
```

For Codex, launch Codex from this repo or copy/symlink individual root skill folders into a Codex skill location. The repo-level `.agents/skills` directory is already wired to the root skills.

For Claude Code, either work directly in this repo, copy/symlink `.claude/skills/<name>` into another project's `.claude/skills/`, or add this checkout as an additional directory:

```bash
claude --add-dir /path/to/agent-skills
```

For Gemini CLI, copy or symlink the wrapper files from `.gemini/agents/` into your project or user agent directory. Make sure Gemini can also read this repo, for example by launching from this checkout or adding it as an included directory.

## Example configs

- Codex: [.codex/config.example.toml](.codex/config.example.toml) shows optional per-skill config snippets for `~/.codex/config.toml`.
- Claude Code: [.claude/README.md](.claude/README.md) explains project, personal, and `--add-dir` use.
- Gemini CLI: [.gemini/settings.example.json](.gemini/settings.example.json) shows optional subagent overrides.

If your environment does not preserve symlinks, copy the root skill folders instead. Keep the root `SKILL.md` files as the source of truth to avoid drift.
