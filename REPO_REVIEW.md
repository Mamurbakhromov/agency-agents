# Repository Review: agency-agents

## What this project is

`agency-agents` is a curated library of role-specific AI agent definitions. The repository organizes agents by functional division (engineering, design, marketing, testing, etc.), and each agent is authored as Markdown with YAML frontmatter and a repeatable prompt structure.

Beyond static prompts, the repo includes:

- strategy documents for multi-agent orchestration,
- examples showing cross-agent collaboration,
- conversion scripts that transform agent files into tool-specific formats,
- an installer that deploys the generated assets into local tool environments.

## High-level architecture

### 1) Source-of-truth content

The primary artifacts are Markdown agent files in top-level domain folders (for example `engineering/`, `design/`, `marketing/`, `specialized/`). These are the canonical definitions used for all downstream integrations.

### 2) Strategy + operating model

The `strategy/` docs define a phase-based execution framework with quality gates, handoffs, and orchestration guidance. This gives teams a process layer above individual agent prompts.

### 3) Integration build pipeline

`scripts/convert.sh` converts source agent files into target formats for supported tools (Claude Code, Gemini CLI, Antigravity, Cursor, OpenCode, Aider, Windsurf). This is effectively a content build step.

### 4) Installation workflow

`scripts/install.sh` installs converted artifacts into tool-specific destinations (global or project-scoped depending on tool), and can run interactively or non-interactively.

### 5) Quality linting

`scripts/lint-agents.sh` validates required frontmatter and checks recommended sections. It enforces basic content quality and metadata consistency.

## Strengths

- **Clear product concept**: Strong positioning as a reusable "AI agency" rather than isolated prompt snippets.
- **Good content organization**: Category folders map to business functions and are easy to browse.
- **Operational maturity**: Strategy docs, handoff templates, and runbooks move this beyond a prompt collection.
- **Tooling pragmatism**: Converters + installer lower adoption friction across ecosystems.
- **Contributor on-ramp**: Contributing/linting guidance gives contributors a clear path.

## Gaps / risks observed

1. **Heavy shell implementation surface**
   - Conversion and install logic are Bash-heavy. This keeps dependencies low, but testability and maintainability can degrade as complexity grows.

2. **Partial schema enforcement**
   - Linting checks required fields and section presence, but not deeper schema correctness (e.g., strict heading contracts, semantic validation, duplicate names/slugs).

3. **Potential drift between source and integrations**
   - Generated integration artifacts can become stale if contributors forget to rerun conversion before release.

4. **No automated CI gate visible in repo root**
   - A local lint script exists, but repository-level CI policy is not obvious from top-level docs.

## Suggested next steps

1. **Add CI checks for content and generation drift**
   - Run `scripts/lint-agents.sh` in CI.
   - Add a "generated files up to date" check (e.g., run convert and fail on diff).

2. **Define a stricter agent schema contract**
   - Document required headings and optional sections in a machine-checkable form.
   - Expand lint rules to validate heading structure and frontmatter value quality.

3. **Add golden tests for converters**
   - For a representative subset of agents, snapshot expected outputs per tool and compare in CI.

4. **Version and release policy for integrations**
   - Introduce a changelog/release notes convention so tool users can track agent/content updates predictably.

5. **Improve discoverability for newcomers**
   - Add a concise architecture diagram in `README.md` showing the flow: source agents -> convert -> integrations -> install.

## Overall assessment

This is a well-structured and unusually comprehensive prompt/agent repository with real operational scaffolding. The core value is strong and differentiated. The most important improvements now are around automation safeguards (CI + generation drift) and schema rigor to keep quality high as the library scales.
