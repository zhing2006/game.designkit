# GameDesignKit

> Spec-Driven Design Toolkit for Game Designers

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Language: English](https://img.shields.io/badge/Language-English-blue.svg)](README_EN.md)
[![中文](https://img.shields.io/badge/中文-README-green.svg)](README.md)

**One-line pitch**: GameDesignKit transforms game design documents from chaotic Doc files into executable, verifiable, and iterative design systems.

---

## Table of Contents

- [What is Spec-Driven Development](#what-is-spec-driven-development)
- [What is GameDesignKit](#what-is-gamedesignkit)
- [Core Value](#core-value)
- [Quick Start](#quick-start)
- [Core Concepts](#core-concepts)
- [Command Overview](#command-overview)
- [Best Practices](#best-practices)
- [Relationship with Speckit](#relationship-with-speckit)
- [Workflow Example](#workflow-example)
- [Contributing](#contributing)

---

## What is Spec-Driven Development

**Spec-Driven Development** is a methodology that elevates specifications from static documents to executable workflows:

### Problems with Traditional Approach
- ❌ Documents are forgotten after writing
- ❌ Requirements disconnect from implementation
- ❌ Lack of consistency validation
- ❌ No traceability during iteration

### Advantages of Spec-Driven
- ✅ **Executable specs**: Generate deliverables directly from specifications
- ✅ **Iterative refinement**: Multi-round improvement rather than one-shot generation
- ✅ **AI-assisted workflow**: Leverages Claude and other AI to interpret and execute specs
- ✅ **Technology-agnostic**: Same process works across different tech stacks

### Core Philosophy

```
Traditional: Code is king → Specs discarded after development
Spec-Driven: Specs first → Specs drive the entire workflow
```

---

## What is GameDesignKit

**GameDesignKit** is a Spec-Driven Development toolkit adapted for **game design**, forked from GitHub's [Speckit](https://github.com/github/spec-kit).

### Key Features

- 🎮 **Game design specialized**: Adapted from software engineering to game design domain
- 📝 **Generates design documents**: Outputs gameplay design, numerical frameworks, balance analysis (not code)
- 🎯 **Design pillar-driven**: Uses Design Pillars instead of technical constraints
- 🔄 **Dual spec types**:
  - Global specs (dk-000-*): Game vision, worldbuilding, core loop
  - Feature specs (dk-001+): Combat system, equipment system, etc.
- 🤖 **Multiple AI tool support**: Compatible with Claude Code, Codex CLI, and Cursor (zero-cost command reuse via symlinks)

### Use Cases

- Indie game teams: Need structured design process
- Large projects: Multi-person collaboration requiring design consistency
- Design validation: Ensure all features align with design pillars
- Documentation-driven: Make design docs "living documents" instead of outdated Doc files

---

## Core Value

GameDesignKit solves three major pain points for game designers:

### 1. Design Document Chaos
**Problem**: Word/Excel files scattered everywhere, version confusion, can't find the latest
**Solution**: Unified `gamedesigns/` directory structure, Git version control

### 2. Consistency Hard to Guarantee
**Problem**: New features disconnect from game vision, designers work in silos
**Solution**: Design pillars enforce validation, every feature must align with core principles

### 3. Low Collaboration Efficiency
**Problem**: Design reviews rely on verbal communication, feedback hard to trace
**Solution**: Structured design documents + quality checklists + analysis reports

---

## Quick Start

### Prerequisites

**Choose an AI coding tool** (pick one):

| Tool | Command Path | Official Link | Description |
|------|--------------|---------------|-------------|
| **Claude Code** | `.claude/commands/` | [Documentation](https://docs.claude.com/claude-code) | Anthropic official CLI |
| **Codex CLI** | `.codex/prompts/` | [GitHub](https://github.com/openai/codex) | OpenAI official CLI |
| **Cursor** | `.cursor/commands/` | [Website](https://www.cursor.com/) | AI code editor |

**Other requirements**:
- Git
- Node.js (optional, for script automation)

**Symlink notes**:
- `.codex/prompts` and `.cursor/commands` are symlinks pointing to `.claude/commands`
- All tools share the same command files, zero maintenance cost
- Windows users need Developer Mode enabled or admin privileges for symlink support

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/game.designkit.git
cd game.designkit

# Initialize (automatically creates directory structure)
# Claude Code will automatically recognize commands in .claude/commands
```

### 5-Minute Tutorial

#### Step 1: Create Design Pillars

In Claude Code, execute:

```
/game.designkit.pillars
```

Then describe your game's core principles, e.g.:

```
I'm making a roguelike card game with core experiences:
1. Fast-paced: Each run is 15 minutes, playable in short sessions
2. Strategic depth: Deck building has multiple archetypes, clear counters
3. Satisfying feel: High damage numbers, combo effects, instant feedback
```

AI generates `.game.design/memory/pillars.md`.

#### Step 2: Write Feature Spec

```
/game.designkit.specify
```

Input feature description:

```
dk-001-Combat System
Player draws 5 cards per turn, spends energy to play cards, defeats enemies.
Supports card upgrades, relic effects, elemental reactions.
```

Generates `gamedesigns/dk-001-combat-system/spec.md`.

#### Step 3: Clarify Ambiguities

```
/game.designkit.clarify
```

AI asks up to 5 key questions (e.g., "Is energy cap fixed or growable?").

#### Step 4: Generate Design Plan

```
/game.designkit.plan
```

Generates `plan.md` (design deliverable checklist) and `research.md` (design decision rationale).

#### Step 5: Break Down Tasks

```
/game.designkit.tasks
```

Generates `tasks.md`, broken down into independent executable design tasks by user story.

#### Step 6: Analyze Consistency

```
/game.designkit.analyze
```

Checks if specs and plans have gaps, conflicts, or pillar misalignment.

#### Step 7: Execute Design

```
/game.designkit.implement
```

Generates final design documents to `docs/` directory based on `tasks.md`.

---

## Core Concepts

### 1. Design Pillars

Design pillars are the game's "creative constitution" - all features must align with these principles.

**Structure**:
```yaml
- Name: Concise principle name (2-7 words)
- Description: What this pillar represents (≥20 chars)
- Rationale: Why this pillar matters
```

**Quantity Recommendations**:
- Small games: 1-2 pillars
- Indie games: 2-4 pillars
- Large projects: 4-7 pillars

**Example**:
```markdown
### Pillar 1: Fast-Paced Tension

**Description**: Each session controlled within 15 minutes, combat pace is rapid,
decision windows are short, players stay highly focused

**Rationale**: Targets casual time players, avoids the sluggishness of traditional roguelikes
```

### 2. Dual Spec Types

| Type | Numbering | Content | Example |
|------|-----------|---------|---------|
| **Global Spec** | `dk-000-*` | Game vision, worldbuilding, core loop, emotional curve | `dk-000-game-vision.md` |
| **Feature Spec** | `dk-001+` | Specific systems (combat, equipment, dialogue, etc.) | `dk-001-combat-system.md` |

### 3. File Organization

```
game.designkit/
├── .game.design/
│   ├── memory/
│   │   └── pillars.md              # Design pillars (globally shared)
│   └── templates/                  # Spec templates
│
├── gamedesigns/                     # Working directory
│   ├── dk-000-game-vision/            # Global spec
│   │   ├── spec.md
│   │   ├── plan.md
│   │   └── ...
│   ├── dk-001-combat-system/          # Feature spec
│   │   ├── spec.md                 # Input: Feature specification
│   │   ├── plan.md                 # Design plan
│   │   ├── research.md             # Design decisions
│   │   ├── tasks.md                # Task breakdown
│   │   └── checklists/             # Quality checklists
│   └── dk-002-equipment-system/
│
└── docs/                            # Final output
    ├── combat-system/
    │   ├── gameplay-design.md      # Gameplay design
    │   ├── numerical-framework.md  # Numerical framework
    │   └── balance-analysis.md     # Balance analysis
    └── equipment-system/
```

### 4. Workflow

```mermaid
graph LR
    A[Design Pillars] --> B[Write Spec]
    B --> C[Clarify Ambiguities]
    C --> D[Generate Plan]
    D --> E[Break Down Tasks]
    E --> F[Analyze Consistency]
    F --> G[Execute Design]
    G --> H[Design Documents]
```

---

## Command Overview

| Command | Function | Input | Output |
|---------|----------|-------|--------|
| `/game.designkit.pillars` | Create/update design pillars | Natural language description of core principles | `.game.design/memory/pillars.md` |
| `/game.designkit.specify` | Write game/feature spec | Feature description or game vision | `gamedesigns/dk-###-name/spec.md` |
| `/game.designkit.clarify` | Identify and resolve ambiguities | Existing spec.md | Updated spec.md (with clarification answers) |
| `/game.designkit.plan` | Generate design plan | spec.md | plan.md + research.md + quickstart.md |
| `/game.designkit.tasks` | Break down design tasks | plan.md | tasks.md (organized by user story) |
| `/game.designkit.analyze` | Analyze document consistency | All design documents | Analysis report (read-only, no modifications) |
| `/game.designkit.implement` | Execute design tasks | tasks.md | Final design docs in docs/ |

### Command Usage Order

Typical flow: `pillars` → `specify` → `clarify` → `plan` → `tasks` → `analyze` → `implement`

**Flexible Usage**:
- Can run `analyze` anytime to check current state
- `clarify` can be called multiple times after `specify`
- Small features can skip `clarify` and go directly to `plan`

---

## Best Practices

### 1. Design Pillar Recommendations

**✅ Good Pillar**
```markdown
### Pillar: Player Choice Paramount

**Description**: Each level provides at least 3 completion routes (combat/stealth/diplomacy),
player's build determines optimal approach, no single best strategy exists

**Rationale**: Enhances replay value, respects player's gameplay style preferences
```

**❌ Bad Pillar**
```markdown
### Pillar: Fun
**Description**: Game should be fun  ← Too vague, can't be verified
```

### 2. Spec Writing Tips

**✅ Emphasize Experience Goals**
```markdown
Player should feel tense relief when defeating the Boss for the first time,
difficulty curve is steep but fair, failure clearly indicates improvement direction.
```

**❌ Avoid Implementation Details**
```markdown
Use Unity's Animator state machine to implement Boss AI...  ← This is dev doc, not design spec
```

**✅ User Story Prioritization**
```markdown
P1 (MVP): Player can complete basic combat loop (draw-play-resolve)
P2 (Enhancement): Add relic and card upgrade systems
P3 (Nice-to-have): Add daily challenges and leaderboards
```

### 3. MVP Strategy

- **Complete P1 stories first**: Ensure core loop is playable
- **Each story independently verifiable**: Can be tested alone
- **Incremental delivery**: P1 → P2 → P3, avoid designing too much at once

### 4. Team Collaboration

**Design Review Process**:
1. Designer A completes `spec.md` + `plan.md`
2. Run `analyze` to check consistency
3. Team reviews `research.md` (design decision rationale)
4. After approval, execute `implement`

**Version Control**:
- Design pillar changes: Use semantic versioning (1.0.0 → 2.0.0)
- Feature spec iterations: Trace through Git commits

### 5. Common Pitfalls

| Pitfall | Consequence | Avoidance Method |
|---------|-------------|------------------|
| Premature detail | Waste time on features that might be cut | spec.md only writes experience goals, leave numbers to plan stage |
| Pillar conflicts | "Hardcore difficulty" vs "Casual relaxed" coexist | Run `analyze` to detect conflicts |
| Doc out of sync | spec changed but plan not updated | Re-run `plan` after modifying spec |
| Missing user stories | Can't prioritize, everything piled into MVP | At least 3 user stories per spec (P1/P2/P3) |

---

## Relationship with Speckit

GameDesignKit is a game design domain adaptation of GitHub's [Speckit](https://github.com/github/spec-kit).

### What is Speckit?

Speckit is GitHub's open-source "spec-driven development" toolkit, originally for software engineering.

### Core Differences

| Dimension | Speckit (Software Engineering) | GameDesignKit (Game Design) |
|-----------|--------------------------------|-----------------------------|
| **Output** | Source code, APIs, data models | Design docs (gameplay, numbers, balance) |
| **Principle System** | Project Constitution (technical constraints) | Design Pillars (creative principles) |
| **Spec Types** | Single type | Dual types (global dk-000-* / feature dk-001+) |
| **Validation** | Automated tests | Document completeness + pillar alignment |
| **Domain** | Technical implementation | Experience design |
| **Language** | English | Chinese |

### Why Adaptation?

- **Domain difference**: Game designers don't write code, need to generate design documents
- **Thinking difference**: Game design emphasizes "experience goals" not "technical constraints"
- **Deliverable difference**: Software delivers code, game designers deliver design plans

Thanks to GitHub's Speckit team for providing an excellent foundational framework!

---

## Workflow Example

### Scenario: Designing an "Equipment Enhancement System"

```bash
# 1. Assume design pillars already exist (pillars.md)

# 2. Write feature spec
/game.designkit.specify
> "002-Equipment Enhancement System: Players consume materials to upgrade
> equipment attributes. Enhancement has success rate, failure causes downgrade,
> creates tense gambling feel. Needs to integrate with combat system,
> enhanced equipment affects combat strategy."

# Output: gamedesigns/dk-002-equipment-enhance/spec.md

# 3. Clarify ambiguities
/game.designkit.clarify
> AI asks: "Does enhancement failure downgrade by 1 level or reset to zero?"
> Answer: "Downgrade by 1 level, but 3 consecutive failures reset to zero."

# 4. Generate design plan
/game.designkit.plan
# Output: plan.md, research.md, quickstart.md

# 5. Break down tasks
/game.designkit.tasks
# Output: tasks.md, including:
#   - P1 story: Basic enhancement flow
#   - P2 story: Failure penalty mechanism
#   - P3 story: Enhancement animations and effects

# 6. Analyze consistency
/game.designkit.analyze
# Check:
#   - Does it conflict with "Satisfying Feel" pillar? (Failure downgrade might be too frustrating)
#   - Does it match numerical ranges of dk-001-Combat System?

# 7. Execute design
/game.designkit.implement
# Output to docs/equipment-enhance/:
#   - gameplay-design.md (Enhancement flow, success rate curves)
#   - numerical-framework.md (Material costs, attribute growth formulas)
#   - system-integration.md (Integration with combat system)
#   - balance-analysis.md (Risk-reward balance analysis)
```

### Final Output Document Structure

```
docs/equipment-enhance/
├── gameplay-design.md          # How players enhance equipment, operation flow
├── numerical-framework.md      # Success rate formulas, attribute growth curves
├── system-integration.md       # Integration with combat, economy systems
└── balance-analysis.md         # Risk-reward analysis, pillar alignment
```

---

## Contributing

### How to Contribute

We welcome the following contributions:

- 🐛 **Report issues**: Submit bugs in [Issues](https://github.com/yourusername/game.designkit/issues)
- 💡 **Feature suggestions**: Propose new commands or workflow improvements
- 📖 **Documentation improvements**: Enhance README, best practices, examples
- 🔧 **Code contributions**: Optimize scripts, templates, command logic

### Pull Request Guidelines

1. Fork this repository
2. Create feature branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m "Add: your feature"`
4. Push branch: `git push origin feature/your-feature`
5. Submit PR with description of changes

### Issue Reporting

Encountered a problem? Please provide in your Issue:
- Command used
- Input content (sanitized)
- Expected behavior vs actual behavior
- Claude Code version

---

## License

MIT License - See [LICENSE](LICENSE)

---

## Acknowledgments

- Thanks to [GitHub Speckit](https://github.com/github/spec-kit) for providing an excellent foundational framework
- Thanks to [Claude Code](https://docs.claude.com/claude-code) for providing powerful AI workflow capabilities

---

**Start your spec-driven game design journey!** 🎮✨

For questions, please check [Issues](https://github.com/yourusername/game.designkit/issues) or submit a new issue.
