# agent-skills

A collection of [Agent Skills](https://docs.cursor.com/context/skills) (and compatible skill packs) for Cursor and similar tools. Each skill lives in its own folder with a `SKILL.md` entry point.

## Skills

| Skill | Path | Summary |
|--------|------|---------|
| **Deep Documentation** | [deep-documentation/SKILL.md](deep-documentation/SKILL.md) | Create, expand, reorganize, and maintain repository documentation with deep coverage: READMEs, browsable docs, API/reference pages, `llms.txt` / `llms-full.txt`, changelogs, and repo-local agent skills. Use when you want comprehensive docs, LLM-friendly indexes, documentation that stays in sync with code, or guidance for future models on how to use the project. |
| **Truthful Coder** | [truthful-coder/SKILL.md](truthful-coder/SKILL.md) | Enforces strict change transparency while editing code, notebooks, configs, or running commands: announce intent, track what actually changed, and disclose every delta (including formatters, lockfiles, and incidental edits). Use when you want no “hidden” repo or environment changes. |

## Using a skill

Copy or symlink a skill folder into your tool’s skills location (for example `.cursor/skills/<name>/` in a project), or register it according to your client’s documentation. The agent loads instructions from `SKILL.md` when the skill is enabled or matched to the task.
