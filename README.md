# agent-skills

A small collection of [Agent Skills](https://github.com/anthropics/skills) shipped through Claude Code, Codex, and Gemini CLI from one canonical source. Each skill lives in its own root folder with `SKILL.md` as the source of truth; per-agent adapters point back at it so there is no drift across runtimes.

## Skills

| Skill | Path | Summary |
|-------|------|---------|
| **deep-documentation** | [deep-documentation/SKILL.md](deep-documentation/SKILL.md) | Create, expand, reorganize, and maintain repository documentation with deep coverage: READMEs, browsable docs, API/reference pages, `llms.txt` / `llms-full.txt`, changelogs, and repo-local agent skills. Use when you want comprehensive docs, LLM-friendly indexes, documentation that stays in sync with code, or guidance for future models on how to use the project. |
| **truthful-coder** | [truthful-coder/SKILL.md](truthful-coder/SKILL.md) | Strict change transparency while editing code, notebooks, configs, or running commands: announce intent, track what actually changed, and disclose every delta (including formatters, lockfiles, and incidental edits). Use when you want no "hidden" repo or environment changes. |

## Install

The recommended path for Claude Code is the plugin marketplace:

```text
/plugin marketplace add adamdroberts/agent-skills
/plugin install deep-documentation@adamdroberts-skills
/plugin install truthful-coder@adamdroberts-skills
```

The marketplace name is `adamdroberts-skills` (the literal `agent-skills` is reserved by Anthropic). Updates ride on git commits — re-run `/plugin marketplace update adamdroberts-skills` to pull the latest.

Codex can also add this repository as a plugin marketplace:

```bash
codex plugin marketplace add adamdroberts/agent-skills
```

Then use the Codex TUI Plugins panel to install `deep-documentation` or `truthful-coder`. For Gemini CLI, project-scoped installs, and direct use without an agent runtime, see **[docs/install.md](docs/install.md)**.

## Agent support

| Agent | Adapter | Notes |
|-------|---------|-------|
| Claude Code (marketplace) | [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json) + per-plugin [plugin.json](deep-documentation/.claude-plugin/plugin.json) | Each skill is its own plugin under marketplace `adamdroberts-skills`; `"skills": ["./"]` loads the root `SKILL.md` directly. |
| Claude Code (project) | [.claude/skills/](.claude/skills/) | Symlinks to the canonical root skill folders. Loaded via `--add-dir` or copied into another project's `.claude/skills/`. |
| Codex (plugin marketplace) | [.agents/plugins/marketplace.json](.agents/plugins/marketplace.json) + [plugins/](plugins/) | Codex-native plugin metadata; each plugin exposes a `skills/` symlink back to the canonical root folder. |
| Codex (repository skills) | [.agents/skills/](.agents/skills/) | Symlinks to the canonical root skill folders. Codex discovers repository skills from `.agents/skills` and follows symlinks. |
| Gemini CLI | [.gemini/agents/](.gemini/agents/) | Markdown wrappers (not symlinks) because Gemini does not natively load `SKILL.md`. The wrapper reads the canonical `SKILL.md` at runtime. |

## Documentation

- **[docs/](docs/)** — browsable documentation set.
  - [docs/install.md](docs/install.md) — every install path, per agent.
  - [docs/architecture.md](docs/architecture.md) — repo layout, the canonical-folder + adapter pattern, Mermaid diagram of how the distribution paths share one source.
  - [docs/contributing.md](docs/contributing.md) — step-by-step guide for adding a new skill end-to-end (canonical folder, five adapters, plugin manifests, marketplace entries, validation).
- **[CHANGELOG.md](CHANGELOG.md)** — append-only history.
- **[llms.txt](llms.txt)** and **[llms-full.txt](llms-full.txt)** — LLM-oriented index and bundled documentation set for ingestion.

## Status

Stable. Two skills, five install paths across Claude Code, Codex, and Gemini CLI, with no breaking changes planned. New skills follow the contribution checklist in [docs/contributing.md](docs/contributing.md).
