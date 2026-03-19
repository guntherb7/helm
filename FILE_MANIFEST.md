# HELM Ops Hub – Complete File Manifest

This document lists all files created in the ops-hub-public repository.

## Directory Structure

```
ops-hub-public/
├── README.md                               Main repository overview
├── LICENSE                                 Open source license
├── .gitignore                              Git ignore rules
├── SETUP.md                                Comprehensive setup guide
├── FILE_MANIFEST.md                        This file
│
├── business/                               Business operations system
│   ├── CLAUDE.md                           Business workspace overview
│   ├── instruction.md                      Business setup guide
│   ├── memory/
│   │   ├── patterns.md                     Behavioral patterns (energy, productivity, clients, seasonal)
│   │   ├── clients.md                      Client reference with communication patterns
│   │   ├── pricing-history.md              Historical pricing and lessons learned
│   │   └── terminology.md                  Industry and company terminology
│   └── .claude/
│       ├── agents/
│       │   ├── writer.md                   Proposal/email writing agent
│       │   └── reviewer.md                 Contract/proposal review agent
│       └── skills/
│           ├── standup/SKILL.md            Daily standup (morning planning)
│           ├── wrap/SKILL.md               Daily wrap (evening reflection)
│           ├── weekly-plan/SKILL.md        Weekly planning (Monday)
│           ├── retrospective/SKILL.md      Weekly review (Friday)
│           ├── new-proposal/SKILL.md       Generate new proposals
│           ├── new-contract/SKILL.md       Generate contracts from proposals
│           ├── follow-up/SKILL.md          Auto-detect and draft follow-ups
│           ├── new-project/SKILL.md        Create new project workspace
│           ├── done/SKILL.md               Project completion workflow
│           └── update/SKILL.md             Quick status updates
│
├── ops/                                    Operations system (shared state)
│   ├── CLAUDE.md                           Shared conventions and standards
│   ├── config/
│   │   ├── hub-settings.json               Configuration (paths, schedules, credentials)
│   │   └── repo-map.json                   Repository path mappings
│   ├── projects/
│   │   └── example-project.md              Example project file (Acme Corp website rebuild)
│   └── templates/
│       ├── proposal-structure.md           Proposal template
│       ├── contract-structure.md           Contract template structure
│       ├── voice-guide.md                  Personal writing voice guide
│       └── blocks/
│           ├── payment-terms.md            Reusable contract: payment terms
│           ├── ip-assignment.md            Reusable contract: IP ownership
│           ├── warranty.md                 Reusable contract: warranty
│           ├── liability.md                Reusable contract: liability limits
│           └── termination.md              Reusable contract: termination clause
│
├── personal/                               Personal life operations
│   ├── CLAUDE.md                           Personal workspace overview
│   ├── instruction.md                      Personal setup guide (5 phases)
│   ├── README.md                           Personal ops quick reference
│   ├── plan.md                             Values, goals, what matters (annual)
│   ├── personal-ops.md                     Active tasks, recurring habits, avoiding items
│   ├── inbox.md                            Daily capture (emptied each morning)
│   └── health/
│       └── tracking.md                     Health metrics and energy patterns
│
├── financial/                              Business financial operations
│   ├── CLAUDE.md                           Financial workspace overview
│   ├── instruction.md                      Financial setup guide (4 phases)
│   ├── README.md                           Financial quick reference
│   ├── financial-ops.md                    Invoice tracking and expense calendar
│   ├── accounts.md                         Monthly financial snapshot
│   ├── pricing.md                          Rates, pricing history, lessons learned
│   └── tax-prep.md                         Tax timeline, deductions, forms
│
├── hub/                                    Automation hub (background scripts)
│   ├── CLAUDE.md                           Background automation documentation
│   ├── instruction.md                      Automation setup guide
│   └── scripts/
│       └── README.md                       Script descriptions (8 scripts)
│
└── projects/                               Active client projects
    ├── README.md                           Projects quick reference
    └── example-project/                    Example project workspace (Acme Corp)
        ├── CLAUDE.md                       Project workspace overview
        ├── instruction.md                  Project setup guide
        ├── status-log.md                   Weekly progress updates
        └── .claude/
            └── skills/
                └── done/SKILL.md           Project closure skill
```

## File Count Summary

- **Total Files:** 54
- **Markdown Files:** 50 (.md)
- **JSON Config Files:** 2 (.json)
- **Skill Files:** 12 (SKILL.md in .claude/skills/)
- **Agent Files:** 2 (in .claude/agents/)

## Key Workspaces

### 1. Business (`/business/`)
Daily operations for client work:
- Skills for standup, planning, retrospective
- Agents for writing and reviewing
- Memory system for patterns, clients, pricing
- **Read this:** CLAUDE.md, instruction.md

### 2. Personal (`/personal/`)
Life goals, health, recurring personal tasks:
- Integration with daily standup
- Health tracking and energy patterns
- Quarterly goal review
- **Read this:** CLAUDE.md, instruction.md

### 3. Financial (`/financial/`)
Money, invoicing, taxes:
- Invoice tracking and cash flow
- Pricing strategy and history
- Tax preparation timeline
- **Read this:** CLAUDE.md, instruction.md

### 4. Operations (`/ops/`)
Shared state and templates:
- Configuration (paths, settings)
- Project definition template
- Contract and proposal templates
- Reusable contract blocks
- **Read this:** CLAUDE.md

### 5. Projects (`/projects/`)
Individual client project workspaces:
- Example project (Acme Corp website rebuild)
- Project-specific skills and documentation
- Status tracking per project
- **Read this:** README.md, example-project/

### 6. Hub (`/hub/`)
Background automation:
- Overnight scripts
- Automation configuration
- Logging and context building
- **Read this:** instruction.md

## Quick Navigation

**Starting out?**
1. Start with `/README.md` for high-level overview
2. Read `/SETUP.md` for comprehensive setup guide
3. Choose a workspace to explore:
   - Want to plan business work? → `/business/instruction.md`
   - Want to set up personal goals? → `/personal/instruction.md`
   - Want to track finances? → `/financial/instruction.md`

**Setting up a new project?**
1. Read `/projects/README.md`
2. Review `/projects/example-project/` as template
3. Read `/projects/example-project/instruction.md` for setup steps
4. Run `claude skill new-project` to create new workspace

**Creating proposals and contracts?**
1. Review `/ops/templates/proposal-structure.md`
2. Review `/ops/templates/contract-structure.md`
3. Review reusable blocks in `/ops/templates/blocks/`
4. Run `claude skill new-proposal` to generate proposal

**Understanding the system?**
1. Read `/ops/CLAUDE.md` for shared conventions
2. Read each workspace's `CLAUDE.md` for overview
3. Read each workspace's `instruction.md` for setup

## Core Concepts

### Two-Tier Memory
- **Working Memory:** This workspace (current tasks, status, plans)
- **Deep Storage:** Memory files (patterns, lessons learned, historical data)

### Skill-Based Architecture
- **Skills:** Reusable automation workflows (standup, retrospective, etc.)
- **Agents:** AI helpers that generate content (writer, reviewer)

### Integration Points
- **Daily:** `/standup` reads personal and business plans
- **Weekly:** `/retrospective` reviews energy, revenue, avoidance patterns
- **Per-project:** `/new-project` creates workspace, ops file, GitHub repo
- **On completion:** `/done` archives project, captures lessons

### Metadata-Driven
- Projects tracked with YAML frontmatter (contract, milestones, waiting_on)
- Personal tasks include effort, conditions, pairs_with metadata
- Financial tracking includes invoice status, payment terms, recurring expenses

## Version & Setup

**Current Version:** HELM Ops Hub Public v1.0
**Status:** Complete template system (example data, no real clients)
**License:** [See LICENSE file]
**Last Updated:** March 2026

## What's Missing / What You'll Add

These files are templates. You'll customize:
- `/ops/config/hub-settings.json` – Your paths, credentials, preferences
- `/personal/plan.md` – Your actual values and goals
- `/personal/personal-ops.md` – Your actual tasks and habits
- `/business/memory/clients.md` – Your actual client information
- `/business/memory/terminology.md` – Your industry and business terminology
- `/financial/accounts.md` – Your actual financial data
- `/projects/*/` – Your actual projects (example is template)

## Help & Documentation

Each workspace includes:
- **CLAUDE.md** – What this workspace is and how it works
- **instruction.md** – Step-by-step setup guide
- **README.md** – Quick reference

Most template files include examples, explanations, and philosophy sections.

## License & Attribution

This is a public template system. You can fork, modify, and use it for your own operations.

See LICENSE file for full terms.

---

**Questions?** See the relevant CLAUDE.md file for that workspace.
**Getting started?** Read SETUP.md for guided walkthrough.
