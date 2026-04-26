# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# AI Software Studio — Agent Architecture for Software Development

A multi-agent coordination framework for general software development using Claude Code. Contains specialized agents for Requirements, Product, Engineering, QA, Operations, and Legal departments with clear delegation paths and quality gates.

## Template Purpose

This is a **template repository** for bootstrapping software development studios. It provides:
- ~45 pre-configured agents covering requirements analysis, product design, engineering, testing, DevOps, and legal compliance
- ~30 slash commands for common workflows
- Path-based coding rules for multiple programming languages
- Document templates for PRD, architecture, and compliance

**For new projects**: Run `/start` to begin the guided onboarding flow.

## Quick Start

```bash
# Clone as template
git clone https://github.com/your-org/ai-software-studio.git my-project
cd my-project

# Open Claude Code
claude

# Begin onboarding (selects stack, defines project, creates initial structure)
/start
```

## Department Structure

### Six Core Departments

| Department | Focus | Key Agents |
|------------|-------|------------|
| **Requirements** | Business analysis, PRD, user stories | requirements-lead, business-analyst, systems-analyst |
| **Product** | UX design, feature planning | product-lead, ux-designer, product-strategist |
| **Engineering** | Development, architecture, security | engineering-lead, backend-dev, frontend-dev, fullstack-dev |
| **QA** | Testing strategy, automation, quality | qa-lead, qa-engineer, test-automation-eng |
| **Operations** | DevOps, SRE, infrastructure | devops-lead, sre, infrastructure-eng |
| **Legal** | Compliance, contracts, IP | legal-lead, compliance-officer, legal-counsel |

## Template Architecture

### Agent Hierarchy

```
                    [Human Developer]
                           |
        +------------------+------------------+------------------+
        |                  |                  |                  |
   product-director   technical-director   legal-director   operations-director
        |                  |                  |                  |
   requirements-lead   engineering-lead   compliance-lead   devops-lead
        |                  |                  |                  |
      qa-lead        security-lead     legal-counsel    infrastructure-lead
                                                                  |
                                                            platform-lead
```

**Level 1 (Opus)** — Directors: product-director, technical-director, legal-director, operations-director
**Level 2 (Sonnet)** — Leads: requirements-lead, product-lead, engineering-lead, qa-lead, devops-lead, legal-lead
**Level 3 (Sonnet/Haiku)** — Specialists: developers, designers, testers, SREs, compliance officers

### Coordination Rules

1. **Vertical delegation**: Directors → Leads → Specialists
2. **Horizontal consultation**: Same-level agents may negotiate but not make binding cross-domain decisions
3. **Conflict resolution**: Product conflicts escalate to `product-director`, technical conflicts to `technical-director`, legal conflicts to `legal-director`
4. **Change propagation**: Cross-domain changes coordinated by `engineering-lead`
5. **Domain boundaries**: Agents must not modify files outside their designated domain without explicit delegation

### Collaboration Protocol

Every task follows: **Question → Options → Decision → Draft → Approval**

- Agents must ask "Can I write to [path]?" before using Write/Edit
- Agents must present drafts before requesting approval
- Multi-file changes require explicit approval of the entire changeset
- No commits without user instruction

## OpenSpec-Inspired Change Workflow

AI Software Studio uses a **change-based workflow** inspired by OpenSpec. Every modification gets its own folder with planning artifacts.

### Core Change Workflow (opsx commands)

```bash
# 1. Create change with planning artifacts (proposal → specs → design → tasks)
/opsx:propose add-user-authentication

# 2. Implement tasks from tasks.md
/opsx:apply

# 3. Verify implementation matches specs
/opsx:verify

# 4. Archive completed change
/opsx:archive
```

### When to Use Each Command

| Command | When to Use |
|---------|-------------|
| `/opsx:propose` | Clear requirements — generates all planning docs at once |
| `/opsx:explore` | Unclear requirements — investigate first, then propose |
| `/opsx:apply` | Execute tasks from the change folder |
| `/opsx:verify` | Validate implementation against specs before archiving |
| `/opsx:archive` | Finalize and merge specs after verification |

### Change Folder Structure

```
production/
├── changes/
│   ├── add-user-authentication/     # Active change
│   │   ├── proposal.md              # Why and what
│   │   ├── specs/
│   │   │   └── auth/
│   │   │       └── spec.md         # Delta spec (Given/When/Then)
│   │   ├── design.md               # Technical approach
│   │   └── tasks.md                # Implementation checklist
│   └── archive/
│       └── 2026-01-15-add-auth/   # Archived change
└── specs/                          # Main specs (source of truth)
    └── auth/
        └── spec.md
```

## Common Commands

### Change-Based Workflow (OpenSpec-inspired)
```bash
/opsx:propose    # Create change with planning artifacts
/opsx:explore    # Investigate before proposing
/opsx:apply      # Implement tasks
/opsx:verify     # Validate implementation
/opsx:archive    # Finalize and merge
/project-memory  # Cross-session context
```

### Traditional Workflow
```bash
/start            # Onboard new project
/requirements     # Requirements analysis
/design          # System design
/sprint-plan     # Sprint planning (Scrum)
/kanban-board    # Kanban management
/code-review     # Code review
/test-plan       # Test planning
/deploy          # Deployment
/incident        # Incident response
/compliance-check # Compliance check
```

### Team Orchestration
```bash
/team-engineering  # Coordinate engineering team
/team-qa          # Coordinate QA team
/team-devops      # Coordinate DevOps team
/team-release     # Coordinate release process
```

### Template Customization
```bash
# Add new agent — create .claude/agents/[name].md with YAML frontmatter
# Add new skill — create .claude/skills/[name]/SKILL.md
# Add new rule — create .claude/rules/[name].md with paths frontmatter
```

## Key Files

| File | Purpose |
|------|---------|
| `.claude/settings.json` | Hooks, permissions, security rules |
| `.claude/agents/*.md` | Agent definitions with YAML frontmatter |
| `.claude/skills/*/SKILL.md` | Slash command definitions |
| `.claude/rules/*.md` | Path-based coding standards |
| `.claude/hooks/*.sh` | Validation scripts |
| `.claude/docs/templates/*.md` | Document templates |

## Path-Based Rules

Rules auto-apply based on file location:

| Path Pattern | Enforcement |
|-------------|-------------|
| `src/backend/**` | Language-specific backend standards, API design, database patterns |
| `src/frontend/**` | Framework standards (React/Vue/Angular), accessibility, i18n |
| `src/infrastructure/**` | IaC standards (Terraform/K8s), cloud patterns |
| `src/security/**` | Secure coding, OWASP Top 10, secrets management |
| `tests/**` | Test naming, Arrange/Act/Assert structure |
| `docs/requirements/**` | PRD format, user story templates |
| `docs/design/**` | Architecture decision records, API contracts |
| `ops/**` | Deployment scripts, runbooks |

## Multi-Language Support

AI Software Studio supports multiple programming languages:

| Language | Primary Domain | Key Standards |
|----------|---------------|---------------|
| Python | Backend, ML, Scripts | PEP 8, type hints, docstrings |
| TypeScript | Frontend, Node.js | ESLint, strict mode, interfaces |
| Go | Backend, Cloud | gofmt, error wrapping, context propagation |
| Rust | Systems, Performance | clippy, ownership rules, Result types |
| Java | Enterprise Backend | Checkstyle, JUnit, Spring patterns |
| C# | .NET Backend, Games | StyleCop, NUnit, async patterns |

## Project Structure (created by /start)

```
my-project/
├── CLAUDE.md                    # This file
├── .claude/                     # Template configuration
│   ├── agents/                  # Agent definitions
│   ├── skills/                  # Slash commands
│   ├── rules/                   # Coding standards
│   ├── hooks/                   # Validation scripts
│   └── docs/templates/          # Document templates
├── .project-memory/             # Cross-session memory (optional)
│   ├── PROJECT_CONTEXT.md
│   ├── CURRENT_STATE.md
│   ├── DECISIONS.md
│   └── session-journal/
├── src/                         # Source code
│   ├── backend/                 # Backend services
│   ├── frontend/                # Frontend applications
│   ├── infrastructure/          # IaC, K8s manifests
│   └── shared/                  # Shared libraries
├── production/
│   ├── specs/                   # Main specs (source of truth)
│   │   └── auth/
│   │       └── spec.md
│   ├── changes/                 # Active changes
│   │   └── add-auth/
│   │       ├── proposal.md
│   │       ├── specs/
│   │       ├── design.md
│   │       └── tasks.md
│   └── sprints/                 # Sprint plans
├── tests/                       # Test suites
├── ops/                         # Operational scripts
└── docs/                       # Architecture docs, ADRs
```

## Context Management

- Session state saved to `production/session-state/active.md` at each milestone
- After context compression, read `active.md` first to restore state
- Incremental file writing: create skeleton immediately, write sections as approved
- Proactive compaction at ~60-70% context usage

## This Template vs. Actual Software Projects

This repository IS the template. When you clone it for a new project:
1. The template's `.claude/` configs become your project's configs
2. You customize agents/skills/rules to fit your stack and process
3. Your actual code goes in `src/`, `tests/`, `ops/`
4. The template's docs/examples stay as reference material
