# AI Software Studio

<p align="center">
  <img src="https://img.shields.io/badge/agents-23-blueviolet" alt="23 Agents">
  <img src="https://img.shields.io/badge/skills-16-green" alt="16 Skills">
  <img src="https://img.shields.io/badge/rules-4-red" alt="4 Rules">
  <img src="https://img.shields.io/badge/open-spec-inspired-orange" alt="OpenSpec Inspired">
</p>

<p align="center">
  Multi-agent coordination framework for software development using Claude Code.
  <br>
  Inspired by <a href="https://github.com/Donchitos/Claude-Code-Game-Studios">Claude Code Game Studios</a> and <a href="https://github.com/Fission-AI/OpenSpec">OpenSpec</a>.
</p>

---

## Why This Project

Developing software with AI assistants is powerful—but a chat session lacks structure. Nothing stops you from hardcoding magic numbers, skipping design docs, or writing spaghetti code. No QA review, no design review, no one asks "does this really fit the product vision?"

**AI Software Studio** solves this by giving your AI session a real studio structure. You get not a generic assistant, but 23 specialized agents organized in a studio hierarchy—directors guard the vision, department heads lead their domains, specialists execute their work. Each agent has clear responsibilities, escalation paths, and quality gates.

The result: you still make every decision, but now you have a team that asks the right questions, catches mistakes early, and keeps your project organized from brainstorming to deployment.

---

## Features

### 23 Specialized Agents

```
Level 1 — Directors (Opus)
  product-director    technical-director    legal-director    operations-director

Level 2 — Leads (Sonnet)
  requirements-lead   product-lead   engineering-lead   qa-lead   devops-lead   legal-lead

Level 3 — Specialists (Sonnet/Haiku)
  business-analyst    systems-analyst    backend-dev    frontend-dev    fullstack-dev
  security-engineer   ux-designer       sre           infrastructure-eng
  qa-engineer        test-automation-eng compliance-officer
```

### OpenSpec-Inspired Change Workflow

Every modification gets its own folder with structured planning artifacts:

```bash
/opsx:propose    # Create change → proposal → specs → design → tasks
/opsx:apply      # Implement tasks from tasks.md
/opsx:verify     # Validate implementation against specs
/opsx:archive     # Finalize and merge specs
```

### Six Core Departments

| Department | Focus | Key Agents |
|------------|-------|------------|
| **Requirements** | Business analysis, PRD, user stories | requirements-lead, business-analyst, systems-analyst |
| **Product** | UX design, feature planning | product-lead, ux-designer |
| **Engineering** | Development, architecture, security | engineering-lead, backend-dev, frontend-dev, fullstack-dev |
| **QA** | Testing strategy, automation, quality | qa-lead, qa-engineer, test-automation-eng |
| **Operations** | DevOps, SRE, infrastructure | devops-lead, sre, infrastructure-eng |
| **Legal** | Compliance, contracts, IP | legal-lead, compliance-officer |

### Multi-Language Support

| Language | Primary Domain |
|----------|---------------|
| Python | Backend, ML, Scripts |
| TypeScript | Frontend, Node.js |
| Go | Backend, Cloud |
| Rust | Systems, Performance |
| Java | Enterprise Backend |
| C# | .NET Backend |

---

## Quick Start

### Prerequisites

- [Git](https://git-scm.com/)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)

### Installation

1. **Clone or use as template:**
   ```bash
   git clone https://github.com/jackyang97504/ai-software-studio.git my-project
   cd my-project
   ```

2. **Open Claude Code:**
   ```bash
   claude
   ```

3. **Begin onboarding:**
   ```bash
   /start
   ```

---

## Workflow

### Change-Based Development (Recommended)

```
/opsx:propose add-user-authentication
    ↓
Creates: production/changes/add-user-authentication/
├── proposal.md      # Why and what
├── specs/          # Requirements (Given/When/Then format)
├── design.md       # Technical approach
└── tasks.md       # Implementation checklist

/opsx:apply
    ↓
Implements tasks, marks them complete

/opsx:verify
    ↓
Validates: Completeness + Correctness + Coherence

/opsx:archive
    ↓
Merges specs to production/specs/, archives change
```

### Traditional Workflow

```bash
/start              # Onboard new project
/requirements       # Requirements analysis
/design             # System design
/sprint-plan new    # Sprint planning (Scrum)
/kanban-board       # Kanban management
/code-review       # Code review
/test-plan          # Test planning
/deploy             # Deployment
/incident           # Incident response
/compliance-check  # Compliance check
```

---

## Project Structure

```
my-project/
├── CLAUDE.md                    # This file
├── .claude/
│   ├── agents/                 # 23 agent definitions
│   ├── skills/                 # 16 slash commands
│   ├── rules/                  # 4 path-based coding rules
│   └── settings.json           # Hooks, permissions
├── .project-memory/            # Cross-session context (optional)
│   ├── PROJECT_CONTEXT.md
│   ├── CURRENT_STATE.md
│   └── DECISIONS.md
├── src/
│   ├── backend/               # Backend services
│   ├── frontend/              # Frontend applications
│   ├── infrastructure/        # IaC, K8s
│   └── shared/                # Shared libraries
├── production/
│   ├── specs/                  # Main specs (source of truth)
│   ├── changes/               # Active changes
│   └── sprints/               # Sprint plans
├── tests/                      # Test suites
├── ops/                        # Operational scripts
└── docs/                       # Architecture docs
```

---

## OpenSpec Integration

AI Software Studio adopts OpenSpec's spec-driven development philosophy:

### Spec Format (Given/When/Then)

```markdown
## Requirements

### Requirement: User Authentication
The system SHALL issue a JWT token upon successful login.

#### Scenario: Valid credentials
- GIVEN a user with valid credentials
- WHEN the user submits login form
- THEN a JWT token is returned
- AND the user is redirected to dashboard

#### Scenario: Invalid credentials
- GIVEN invalid credentials
- WHEN the user submits login form
- THEN an error message is displayed
```

### RFC 2119 Keywords
- **SHALL/MUST** — absolute requirement
- **SHOULD** — recommended, but exceptions exist
- **MAY** — optional

---

## Documentation

- [CLAUDE.md](CLAUDE.md) — Main configuration for Claude Code
- [.claude/agents/](.claude/agents/) — Agent definitions
- [.claude/skills/](.claude/skills/) — Slash command definitions
- [.claude/rules/](.claude/rules/) — Coding standards

---

## License

MIT License. See [LICENSE](LICENSE).

---

<p align="center">
  Built with Claude Code • Inspired by OpenSpec
</p>
