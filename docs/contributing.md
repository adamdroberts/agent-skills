# Adding a new skill

Use this checklist when you want to ship a new skill through every distribution channel this repo supports. The example uses a hypothetical skill named `my-skill`.

## 1. Create the canonical folder

```bash
mkdir my-skill
```

Add `my-skill/SKILL.md` with valid frontmatter:

```markdown
---
name: my-skill
description: One-sentence trigger. Start with what to do, end with when to use it.
---

# My Skill

Body of the skill goes here. Keep it operational and procedural, not aspirational.
```

The `name` field must match the folder name and is what the user types to invoke the skill. The `description` is what the agent reads to decide whether to load the skill, so make it precise.

If the skill needs supporting files (reference docs, scripts, examples), put them as siblings of `SKILL.md` inside the same folder. They will be copied as part of the plugin.

## 2. Wire each agent

### Claude Code — project skill symlink

```bash
ln -s ../../my-skill .claude/skills/my-skill
```

### Codex symlink

```bash
ln -s ../../my-skill .agents/skills/my-skill
```

### Codex plugin marketplace wiring

Create a Codex plugin root that points back to the canonical skill folder:

```bash
mkdir -p plugins/my-skill/.codex-plugin plugins/my-skill/skills
ln -s ../../../my-skill plugins/my-skill/skills/my-skill
```

Create `plugins/my-skill/.codex-plugin/plugin.json`:

```json
{
  "name": "my-skill",
  "version": "0.1.0",
  "description": "<same one-line summary as SKILL.md>",
  "homepage": "https://github.com/adamdroberts/agent-skills",
  "repository": "https://github.com/adamdroberts/agent-skills",
  "license": "MIT",
  "author": { "name": "Adam Roberts", "url": "https://github.com/adamdroberts" },
  "skills": "./skills/",
  "interface": {
    "displayName": "My Skill",
    "shortDescription": "<short TUI subtitle>",
    "longDescription": "<longer TUI details text>",
    "developerName": "Adam Roberts",
    "category": "Coding",
    "capabilities": ["Interactive", "Read", "Write"],
    "websiteURL": "https://github.com/adamdroberts/agent-skills",
    "defaultPrompt": ["Use my-skill in this repository."],
    "brandColor": "#2563EB",
    "screenshots": []
  }
}
```

Append the plugin to [`.agents/plugins/marketplace.json`](../.agents/plugins/marketplace.json):

```json
{
  "name": "my-skill",
  "source": {
    "source": "local",
    "path": "./plugins/my-skill"
  },
  "policy": {
    "installation": "AVAILABLE",
    "authentication": "ON_INSTALL"
  },
  "category": "Coding"
}
```

### Gemini CLI wrapper

Create `.gemini/agents/my-skill.md` modeled on the existing wrappers:

```markdown
---
name: my-skill
description: <copy or rephrase the SKILL.md description>
kind: local
---

You are the Gemini CLI compatibility wrapper for the `my-skill` Agent Skill in this repository.

Gemini CLI does not natively load `SKILL.md` Agent Skills. Before doing the task, locate and read `my-skill/SKILL.md` from this repository or another included workspace directory. Treat that file as the source of truth for the workflow.

If the root skill file is not available, report that the `my-skill/SKILL.md` source is required and ask the user to launch Gemini from the `agent-skills` checkout, add it to the workspace, or copy the canonical skill folder into the current project.

Follow the skill exactly once loaded.
```

### Codex example config

If users may want to enable/disable the skill from `~/.codex/config.toml`, append a snippet to `.codex/config.example.toml`:

```toml
[[skills.config]]
path = "/path/to/agent-skills/.agents/skills/my-skill/SKILL.md"
enabled = true
```

## 3. Publish to the Claude Code plugin marketplace

### Add the per-plugin manifest

Create `my-skill/.claude-plugin/plugin.json`:

```json
{
  "name": "my-skill",
  "description": "<same one-line summary as SKILL.md>",
  "homepage": "https://github.com/adamdroberts/agent-skills",
  "repository": "https://github.com/adamdroberts/agent-skills",
  "license": "MIT",
  "author": { "name": "Adam Roberts" },
  "skills": ["./"]
}
```

`"skills": ["./"]` is the magic line — it tells Claude Code that the SKILL.md is at the plugin root. The skill name comes from the SKILL.md frontmatter `name` field.

### Add the marketplace entry

Append to the `plugins` array in [`.claude-plugin/marketplace.json`](../.claude-plugin/marketplace.json):

```json
{
  "name": "my-skill",
  "source": "./my-skill",
  "description": "<same one-line summary>",
  "category": "<pick one>",
  "keywords": ["…"],
  "homepage": "https://github.com/adamdroberts/agent-skills",
  "repository": "https://github.com/adamdroberts/agent-skills",
  "license": "MIT",
  "author": { "name": "Adam Roberts" }
}
```

### Validate

```bash
claude plugin validate .
```

Resolve any errors before continuing.

### Test locally

```bash
claude plugin marketplace add /path/to/agent-skills --scope local
claude plugin install my-skill@adamdroberts-skills --scope local
# verify the skill loads, then clean up
claude plugin uninstall my-skill@adamdroberts-skills --scope local
claude plugin marketplace remove adamdroberts-skills
```

## 4. Document the skill

- Add a row for the new skill in the catalog table in [../README.md](../README.md).
- Add an entry under the agent support / project files in [README.md](../README.md) and [docs/architecture.md](architecture.md) if the layout changes.
- Refresh [../llms.txt](../llms.txt) and [../llms-full.txt](../llms-full.txt) to mention the new skill.
- Append a `CHANGELOG.md` entry that names the new skill, summarizes its purpose, and notes any user-visible install steps.

## 5. Verify

Final pre-commit checklist:

- [ ] `SKILL.md` frontmatter has `name` (matching folder) and `description`.
- [ ] All five adapters exist: `.claude/skills/<name>`, `.agents/skills/<name>`, `.gemini/agents/<name>.md`, `<name>/.claude-plugin/plugin.json`, and `plugins/<name>/.codex-plugin/plugin.json`.
- [ ] `.claude-plugin/marketplace.json` and `.agents/plugins/marketplace.json` list the plugin.
- [ ] `claude plugin validate .` passes and the Codex plugin manifest is valid JSON.
- [ ] README catalog table includes the new skill.
- [ ] `llms.txt`, `llms-full.txt`, and `CHANGELOG.md` are updated.

Commit, push, and the skill is installable remotely on the next `/plugin marketplace update adamdroberts-skills`.
