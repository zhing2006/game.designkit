# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

GameDesignKit is a Spec-Driven Development toolkit for game designers, adapted from GitHub's Speckit. It transforms game design documents into executable, verifiable, and iterative design systems. **This toolkit generates design documents (gameplay, numerical frameworks, balance analysis), not source code.**

## Key Principles

- **Experience over implementation**: Focus on "what" and "why", never "how" (no game engines, programming languages, database details)
- **Pillar-driven validation**: All design decisions must align with design pillars in `.game.design/memory/pillars.md`
- **Dual spec types**: Global specs (dk-000-*) for game vision/worldbuilding; Feature specs (dk-001+) for specific systems
- **Technology-agnostic**: Output is readable by game designers, not programmers

## Workflow Commands

Execute in order: `pillars` → `specify` → `clarify` → `plan` → `tasks` → `analyze` → `implement`

| Command | Purpose |
|---------|---------|
| `/game.designkit.pillars` | Create/update design pillars (creative constitution) |
| `/game.designkit.specify` | Write game vision (000) or feature spec (001+) from natural language |
| `/game.designkit.clarify` | Resolve ambiguities with max 5 targeted questions |
| `/game.designkit.plan` | Generate plan.md + research.md + quickstart.md |
| `/game.designkit.tasks` | Break down into prioritized tasks (P1/P2/P3 user stories) |
| `/game.designkit.analyze` | Read-only consistency analysis (pillar alignment, conflicts) |
| `/game.designkit.implement` | Execute tasks to produce final docs in `docs/` |

## Directory Structure

```
.game.design/
├── memory/pillars.md          # Design pillars (must exist before feature specs)
├── scripts/bash/              # Linux/Mac scripts
├── scripts/powershell/        # Windows scripts
└── templates/                 # Spec, plan, tasks templates

gamedesigns/
├── dk-000-game-vision/           # Global spec (vision, worldbuilding)
│   ├── spec.md, plan.md, tasks.md, checklists/
└── dk-001-feature-name/          # Feature specs (auto-numbered)

docs/                          # Final output design documents
```

## Script Execution

Scripts output JSON for path resolution. Always parse the JSON output:

## Validation Rules

- Max 3 `[NEEDS CLARIFICATION]` tags per spec
- Feature specs must reference dependencies (at least dk-000-game-vision)
- Checklists are generated in `checklists/` for each spec
- Quality checks: pillar alignment, no implementation details, testable requirements

## Multi-AI Tool Support

Commands in `.claude/commands/` are symlinked to:
- `.codex/` (OpenAI Codex CLI)
- `.cursor/` (Cursor)
- `.gemini/` (Gemini CLI - uses `.toml` format)
- `.kilocode/` (Kilo Code)

## Language

Primary documentation is in Chinese (中文). English README available at `README_EN.md`.
