# Changelog

Append-only history of this repository. Entries are grouped by date.

## 2026-05-02

### Added — Codex plugin marketplace

- Added a Codex-native marketplace catalog at [.agents/plugins/marketplace.json](.agents/plugins/marketplace.json).
- Added Codex plugin manifests at [plugins/deep-documentation/.codex-plugin/plugin.json](plugins/deep-documentation/.codex-plugin/plugin.json) and [plugins/truthful-coder/.codex-plugin/plugin.json](plugins/truthful-coder/.codex-plugin/plugin.json), each using Codex's string-valued `"skills": "./skills/"` manifest shape.
- Added per-plugin `skills/` symlinks back to the canonical root skill folders so Codex plugin installs and repository skill discovery share the same `SKILL.md` source.
- Why it matters: Codex no longer has to parse Claude Code's array-valued `"skills": ["./"]` plugin manifests, which was causing `plugin/read failed` in the TUI.

### Added — deep documentation pass

- Added `docs/` with: index ([docs/README.md](docs/README.md)), per-agent install guide ([docs/install.md](docs/install.md)), architecture page with Mermaid diagram ([docs/architecture.md](docs/architecture.md)), and contribution guide for adding new skills end-to-end ([docs/contributing.md](docs/contributing.md)).
- Added [llms.txt](llms.txt) and [llms-full.txt](llms-full.txt) so LLM agents can discover and ingest the documentation set without traversing the tree.
- Added this `CHANGELOG.md`.
- Polished [README.md](README.md) to link to the new documentation set and tighten the install snippets.

### Added — Claude Code plugin marketplace

- Published the repo as a Claude Code plugin marketplace named `adamdroberts-skills` ([.claude-plugin/marketplace.json](.claude-plugin/marketplace.json)). The literal `agent-skills` name is reserved by Anthropic and could not be used.
- Added per-skill plugin manifests at [deep-documentation/.claude-plugin/plugin.json](deep-documentation/.claude-plugin/plugin.json) and [truthful-coder/.claude-plugin/plugin.json](truthful-coder/.claude-plugin/plugin.json), each declaring `"skills": ["./"]` so the existing root-level `SKILL.md` files are loaded directly without restructuring.
- Why it matters: skills are now installable remotely with `/plugin marketplace add adamdroberts/agent-skills` and `/plugin install <skill>@adamdroberts-skills`. No `version` field is set, so every commit on `main` is a new version and `/plugin marketplace update` will pick it up.
- Verification: `claude plugin validate .` passes; both plugins were installed locally end-to-end and `SKILL.md` was confirmed at the cached plugin root, then the test install was rolled back.

### Added — agent integration adapters committed

- Committed the per-agent integration directories that the README had been referencing: `.agents/skills/` and `.claude/skills/` (symlinks to canonical root skill folders), `.gemini/agents/` (compatibility wrappers because Gemini does not natively load `SKILL.md`), `.codex/config.example.toml`, `.gemini/settings.example.json`, and [.claude/README.md](.claude/README.md).
- Added `.gitignore` to keep `.claude/settings.local.json` out of version control.

## 2026-04-14

### Added

- Reorganized the README to introduce the skill catalog and per-agent support sections.
- Added the canonical `deep-documentation` skill at [deep-documentation/SKILL.md](deep-documentation/SKILL.md): create and maintain serious repository documentation across READMEs, browsable docs, API references, `llms.txt`, `llms-full.txt`, changelogs, and repo-local agent skills.

## 2026-02-22

### Added

- Initial commit and project description in the README.
- Added the canonical `truthful-coder` skill at [truthful-coder/SKILL.md](truthful-coder/SKILL.md): enforces strict change transparency for code, notebook, config, and command edits so every delta is disclosed.
