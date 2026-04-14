---
name: deep-documentation
description: >-
  Create, expand, reorganize, and maintain repository documentation with deep
  coverage across README files, browsable docs, API/reference pages, llms.txt,
  llms-full.txt, changelogs, and repo-local agent skills. Use whenever the
  user asks for comprehensive project documentation, documentation refreshes,
  LLM-friendly docs, documentation synchronization after code changes, or
  agent guidance that teaches future LLMs how to use the project docs.
---

# Deep Documentation

Use this skill when the task is to produce or maintain serious repository documentation, not a light README pass. The target quality bar is a repo where humans and LLMs can both understand how to use the system, what changed, and where to find the exact surface they need.

Documentation is part of delivery. If the code, API, setup, workflow, or behavior changes, the docs change in the same task.

## Core rules

- Ground everything in the real repo. Read code, config, routes, schemas, tests, and existing docs before writing.
- Do not write vague marketing prose. Write operational, exact documentation.
- Keep the documentation layered: overview for orientation, guides for workflows, reference for exact interfaces, internals for maintainers.
- Use Mermaid diagrams in architectural documentation and anywhere business logic, workflow, or system behavior is easier to understand visually.
- Treat LLM-facing documentation as first-class project artifacts, not an afterthought.
- Treat changelog updates as required for meaningful changes.
- Create or update repo-local agent skills when the repo has distinct workflows or tool surfaces that future LLMs should follow.

## Documentation standard

Documentation at this level should usually include most or all of these surfaces:

- `README.md` for the current product story, setup, quickstart, and top-level links.
- `docs/README.md` or equivalent index that maps the full documentation set.
- Guide pages for how to build, use, run, or operate the system.
- Reference pages for public APIs, endpoints, tools, config fields, schema shapes, environment variables, and commands.
- Internal architecture or subsystem pages for maintainers.
- Testing or verification docs when the repo has a meaningful test surface.
- Agent-skills documentation when the project wants LLMs to use the codebase in a specific way.
- `llms.txt` as the concise LLM index.
- `llms-full.txt` as the single-file documentation bundle for ingestion.
- `CHANGELOG.md` as the append-only historical record.

For architecture docs and business-logic-heavy workflows, include Mermaid charts that make the structure or execution flow explicit. Prefer diagrams for request lifecycles, service boundaries, state transitions, data flow, training or job pipelines, and multi-step business rules.

For large repos, mirror the codebase structure in the docs. If the repo has separate SDK, API, CLI, editor, server, or MCP/tooling surfaces, document them separately and index them together.

## How to work

### 1. Map the repo before writing

Inspect:

- top-level entrypoints and package manifests
- source directories and subsystem boundaries
- routers, handlers, schemas, models, settings, config types
- tests that define expected behavior
- existing docs, changelog, and LLM artifacts
- existing repo-local skills such as `.cursor/skills/`, `.codex/skills/`, or similar

Build a documentation map before editing:

- what the product is
- who uses each surface
- which files define public behavior
- which docs already exist
- which docs are stale or missing

Do not ask the user questions that the repo can answer.

### 2. Write layered docs, not one giant summary

Use layers with distinct jobs:

- Overview docs explain what exists and where to go next.
- Guide docs explain how to accomplish real tasks end to end.
- Reference docs enumerate exact interfaces and shapes.
- Internal docs explain architecture, data flow, persistence, background jobs, routing, or editor/store/service internals.

When documenting architecture or business logic, include Mermaid diagrams alongside prose. Use them to show the actual structure or flow, not as decorative summaries.

Cross-link aggressively with relative markdown links so a reader can move between overview, guide, and reference pages.

### 3. Document exact surfaces

When documenting a public surface, include the real details from code:

- Python or library APIs: classes, functions, methods, dataclasses, fields, defaults, and important return values
- HTTP APIs: method, path, auth requirements, request shape, response shape, error behavior
- MCP or tool APIs: tool name, parameters, purpose, workflow position, and common call patterns
- CLI or scripts: command, required flags, outputs, and prerequisites
- Settings: environment variable, default, purpose, and operational impact
- Frontend or editor systems: page/route structure, state containers, API client contracts, component responsibilities

If the repo exposes many public symbols, prefer one reference page per subsystem instead of dumping everything into one page.

### 4. Include runnable and navigable examples

Where useful, include:

- short runnable code examples
- realistic request and response samples
- command sequences for setup and startup
- workflow examples that connect multiple pages or subsystems
- Mermaid flowcharts, sequence diagrams, or graph diagrams for business logic and architecture

Examples should clarify usage, not pad the page.

## Required artifacts

### `README.md`

Keep the top-level README current with:

- what the project is now
- current status or stability notes if relevant
- installation and startup basics
- top-level workflow or quickstart
- links to the full documentation set
- links to `llms.txt`, `llms-full.txt`, and agent-skills docs when those artifacts exist

Do not leave the README as a stale launch announcement once the product has grown.

### `docs/` index and section pages

The docs index should describe the documentation set and route the reader by need, not just by folder name.

Good patterns:

- “Getting started”
- “Architecture”
- “Framework guide” or “How-to guides”
- “API reference”
- “Tool reference”
- “Server internals”
- “Frontend/editor reference”
- “Testing”
- “Agent skills”

Each entry should say what the page covers.

### `llms.txt`

`llms.txt` is the concise LLM-oriented index. It should:

- explain the project in a compact way
- point to the major documentation sections
- help an LLM find the right page quickly
- include links to important guides, references, and agent-skills docs
- point to `llms-full.txt` as the full ingestion artifact

Do not make `llms.txt` a copy of the README. It is an index for machine readers.

### `llms-full.txt`

`llms-full.txt` is the single-file documentation bundle for ingestion. It should:

- contain or aggregate the canonical docs in one place
- follow a stable, readable order
- include enough context that an LLM can answer questions without re-traversing the full tree
- reflect current docs, not an older snapshot

If the repo has a documented generator or build step for `llms-full.txt`, use it. If not, treat `llms-full.txt` as a maintained artifact and update it directly whenever the underlying docs change enough to make it stale.

Do not leave `llms.txt` and `llms-full.txt` out of sync with the browsable docs.

## Automatic documentation updating

Documentation updates are mandatory in the same task whenever changes affect:

- user-facing behavior or workflows
- setup, install, run, or deployment instructions
- configuration or environment variables
- authentication, routing, persistence, background jobs, dataset handling, training, or operational flow
- public library APIs
- REST, RPC, CLI, MCP, or tool interfaces
- agent workflows that depend on repo-specific instructions

A meaningful code change is not complete until the relevant documentation is updated.

Map changed code to changed docs explicitly:

- library/package changes -> matching API reference and any affected guide pages
- endpoint changes -> matching API docs
- tool changes -> matching tool docs and repo-local skills
- config changes -> setup/configuration docs and README if user-visible
- workflow changes -> getting-started or operational guides
- internal architecture changes -> internals docs if maintainers need the new model

Do not hide doc work in a vague “updated docs” line. Update the exact pages that correspond to the changed surface.

## Changelog rules

Maintain an append-only `CHANGELOG.md`.

For meaningful changes, append an entry that includes:

- date or release grouping, following repo convention
- what changed
- why it matters
- migration notes or compatibility notes when relevant
- verification performed

For breaking changes, add a clearly labeled breaking-change note that says:

- what the old behavior or interface was
- what it is now
- who is affected
- what callers, users, or agents must update

Prefer useful changelog entries over terse release notes. The changelog should help a maintainer or agent understand the historical evolution of the repo.

## Agent skills for documentation use

When the repo has distinct subsystems or workflows, create or update repo-local agent skills so future LLMs can use the documentation correctly and stay on the intended path.

Typical skill split:

- core SDK or library usage
- API or MCP/tool usage
- model-building or training workflows
- deployment or operations workflows
- frontend/editor workflows if they are substantial

Agent skills should:

- stay concise and procedural
- trigger on real user intents
- point back to canonical docs instead of duplicating everything
- link to the docs index and `llms-full.txt`
- explain when to use the skill and when not to use it
- include high-signal examples or quick-reference tables only where they materially improve execution

If the repo exposes multiple skills, maintain an index page describing:

- each skill name
- where it lives
- what it should trigger on
- which documentation pages it summarizes or depends on

Do not create giant skills that try to replace the full docs. Skills should route agents into the docs, encode repo-specific workflow rules, and keep context efficient.

## Recommended workflow

Use this sequence:

1. Inspect the current repo and docs.
2. Identify the code-defined surfaces that need documentation.
3. Update `README.md` and the browsable docs.
4. Update or create the matching reference and workflow pages.
5. Sync `llms.txt`.
6. Sync `llms-full.txt`.
7. Append the changelog entry.
8. Create or update repo-local agent skills and any agent-skills index page.
9. Verify links, page coverage, terminology consistency, and examples against code.

## Quality bar

Aim for documentation that lets a reader answer these questions quickly:

- What is this project?
- How do I install and run it today?
- Which docs page covers my task?
- What are the exact public APIs, endpoints, tools, or config keys?
- What changed recently?
- If I am an LLM agent, which skill should I use and which docs should I trust?

If those answers are not available without digging through source code, the docs are not deep enough yet.

## Done criteria

Do not consider the task complete until all of the following are true:

- the top-level docs and detailed docs agree with the current implementation
- `llms.txt` points to the right places
- `llms-full.txt` is current
- the changelog records the meaningful change
- repo-local agent skills are created or updated when needed
- no page obviously contradicts code, config, routes, schemas, or tests

## Avoid

- vague summaries that omit the exact interface
- stale README files that no longer match the product
- updating docs without syncing LLM artifacts
- updating APIs or tools without updating agent skills
- rewriting changelog history instead of appending
- inventing behavior not confirmed by code
