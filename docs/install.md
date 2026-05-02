# Install

The same canonical skills are exposed through several distribution paths. Pick the one that matches your agent.

## Claude Code — plugin marketplace (recommended)

Install remotely with no clone. Inside Claude Code:

```text
/plugin marketplace add adamdroberts/agent-skills
/plugin install deep-documentation@adamdroberts-skills
/plugin install truthful-coder@adamdroberts-skills
```

To pull updates later:

```text
/plugin marketplace update adamdroberts-skills
```

The marketplace name is `adamdroberts-skills` (the literal `agent-skills` name is reserved by Anthropic for official use). The two plugins use the same names as the skills they ship.

Manifests:

- Marketplace catalog: [`.claude-plugin/marketplace.json`](../.claude-plugin/marketplace.json)
- Per-plugin manifests: [`deep-documentation/.claude-plugin/plugin.json`](../deep-documentation/.claude-plugin/plugin.json) and [`truthful-coder/.claude-plugin/plugin.json`](../truthful-coder/.claude-plugin/plugin.json)

Each plugin manifest sets `"skills": ["./"]` so the `SKILL.md` at the plugin root is loaded directly. The `name` field in the SKILL.md frontmatter determines the invocation name.

Versioning: neither manifest pins a `version`, so Claude Code uses the git commit SHA. Every commit on `main` is treated as a new version, and `/plugin marketplace update` will pick it up.

## Claude Code — project skills

Useful when you want the skills available in a single project without going through the marketplace.

### Option A: clone and add this repo as a directory

```bash
git clone https://github.com/adamdroberts/agent-skills.git
claude --add-dir /path/to/agent-skills
```

Claude Code then discovers the skills via `.claude/skills/<name>/SKILL.md` (the entries in [`.claude/skills/`](../.claude/skills/) are symlinks to the canonical root folders).

### Option B: copy or symlink into your own project

From your other project:

```bash
mkdir -p .claude/skills
ln -s /path/to/agent-skills/deep-documentation .claude/skills/deep-documentation
ln -s /path/to/agent-skills/truthful-coder .claude/skills/truthful-coder
```

If your environment does not preserve symlinks, copy the root skill folder instead:

```bash
cp -R /path/to/agent-skills/deep-documentation .claude/skills/deep-documentation
```

### Option C: install personally

Drop the same symlink/copy under `~/.claude/skills/<name>/` to make a skill available across all your Claude Code sessions.

See [`.claude/README.md`](../.claude/README.md) for a focused Claude Code-only summary.

## Codex

Codex discovers repository skills from `.agents/skills/<name>/SKILL.md` and follows symlinks. The repo wires this for you: [`.agents/skills/`](../.agents/skills/) contains symlinks to the canonical root folders.

To use the skills:

- Launch Codex from a checkout of this repo, or
- Copy/symlink individual root skill folders into a Codex skill location for another project.

To enable or disable a skill without removing the symlink, copy the relevant snippet from [`.codex/config.example.toml`](../.codex/config.example.toml) into `~/.codex/config.toml`:

```toml
[[skills.config]]
path = "/path/to/agent-skills/.agents/skills/deep-documentation/SKILL.md"
enabled = true
```

## Gemini CLI

Gemini CLI does not natively load `SKILL.md` files. The repo ships compatibility wrappers in [`.gemini/agents/`](../.gemini/agents/): each `<name>.md` file is a Gemini agent that, when invoked, locates and reads the matching canonical `SKILL.md` and follows it.

To use these:

1. Copy or symlink the wrapper files into your Gemini agent directory:
   ```bash
   cp /path/to/agent-skills/.gemini/agents/deep-documentation.md ~/.gemini/agents/
   cp /path/to/agent-skills/.gemini/agents/truthful-coder.md ~/.gemini/agents/
   ```
2. Make sure Gemini can read the canonical `SKILL.md`. The wrapper expects to find `deep-documentation/SKILL.md` (or `truthful-coder/SKILL.md`) somewhere readable — typically by launching Gemini from this repo, adding it to the workspace, or copying the canonical skill folder into the active project.

For optional per-agent overrides (e.g. `maxTurns`), see [`.gemini/settings.example.json`](../.gemini/settings.example.json).

## Direct use (any agent)

The root skill folders are self-contained. Any agent that can read a markdown file with YAML frontmatter can use them directly:

```bash
git clone https://github.com/adamdroberts/agent-skills.git
# point your agent at agent-skills/deep-documentation/SKILL.md or agent-skills/truthful-coder/SKILL.md
```

## Choosing a path

| If your agent is… | Use |
|-------------------|-----|
| Claude Code, multi-machine | Plugin marketplace (auto-update via git) |
| Claude Code, one project only | Project skills via `--add-dir` or symlink |
| Codex | `.agents/skills/` (already wired) |
| Gemini CLI | `.gemini/agents/` wrappers |
| Anything else | Read `<skill>/SKILL.md` directly |
