# Documentation

This directory documents the `agent-skills` repository: what it ships, how each agent loads it, and how to add a new skill.

The canonical skill content lives in the root-level skill folders (`deep-documentation/`, `truthful-coder/`). Everything in this directory describes how that content is packaged, distributed, and extended — it does not redefine the skills themselves.

## Pages

| Page | Covers |
|------|--------|
| [install.md](install.md) | All ways to install or load these skills: Claude Code plugin marketplace, Claude Code project skills, Codex plugin marketplace, Codex repository skills, Gemini CLI, and direct clone. |
| [architecture.md](architecture.md) | Repo layout, the canonical-folder + adapter pattern, the symlink/wrapper strategy per agent, and a Mermaid diagram of how the distribution paths share one source of truth. |
| [contributing.md](contributing.md) | Step-by-step guide for adding a new skill: root folder, `SKILL.md` frontmatter, multi-agent wiring, plugin manifest, and marketplace entry. |

## Top-level artifacts

| File | Purpose |
|------|---------|
| [../README.md](../README.md) | Project overview, skill catalog, and short install snippets. |
| [../CHANGELOG.md](../CHANGELOG.md) | Append-only history of changes to the skills, distribution layout, and marketplace. |
| [../llms.txt](../llms.txt) | Concise LLM-oriented index pointing at the documentation set. |
| [../llms-full.txt](../llms-full.txt) | Single-file bundle of the canonical docs and skill content for ingestion. |

## Source-of-truth files

| File | Role |
|------|------|
| [../deep-documentation/SKILL.md](../deep-documentation/SKILL.md) | Canonical content of the `deep-documentation` skill. |
| [../truthful-coder/SKILL.md](../truthful-coder/SKILL.md) | Canonical content of the `truthful-coder` skill. |
| [../.claude-plugin/marketplace.json](../.claude-plugin/marketplace.json) | Claude Code marketplace manifest (`adamdroberts-skills`). |
| [../deep-documentation/.claude-plugin/plugin.json](../deep-documentation/.claude-plugin/plugin.json) | Plugin manifest for `deep-documentation`. |
| [../truthful-coder/.claude-plugin/plugin.json](../truthful-coder/.claude-plugin/plugin.json) | Plugin manifest for `truthful-coder`. |
| [../.agents/plugins/marketplace.json](../.agents/plugins/marketplace.json) | Codex marketplace manifest (`adamdroberts-skills`). |
| [../plugins/deep-documentation/.codex-plugin/plugin.json](../plugins/deep-documentation/.codex-plugin/plugin.json) | Codex plugin manifest for `deep-documentation`. |
| [../plugins/truthful-coder/.codex-plugin/plugin.json](../plugins/truthful-coder/.codex-plugin/plugin.json) | Codex plugin manifest for `truthful-coder`. |
