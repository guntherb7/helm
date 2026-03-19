# Business Operations Workspace

This is the active workspace for daily business operations and client work. It integrates your calendar, project pipeline, financial tracking, and decision-making.

## How This Works

**Two-Tier Memory:**
- **CLAUDE.md (this file) = Working memory:** Current priorities, active projects, decision frameworks, recent patterns. You read this before standup/planning sessions.
- **memory/ directory = Deep storage:** Historical patterns, client profiles, pricing data, energy/productivity trends. Updated weekly by retrospective skill. Not read directly—synthesized into working memory.

## Skills

This workspace loads these skills:
- **/standup** – Morning planning (energy check, inbox, landscape, daily priorities, daily ops files)
- **/wrap** – End-of-day review (what happened, carried forward, tomorrow setup)
- **/weekly-plan** – Monday strategy (3-5 weekly objectives, resource planning)
- **/retrospective** – Friday review (four-lens analysis: revenue, energy, avoidance, growth)
- **/new-proposal** – Guided proposal generation with voice matching
- **/new-contract** – Contract generation from accepted proposal, uses reviewer agent
- **/follow-up** – Checks waiting items, flags communication gaps
- **/new-project** – Full project scaffolding (ops file, repo, CLAUDE.md, hooks, GitHub)
- **/done** – Project completion tracking, scope detection, fallback status updater
- **/update** – Quick one-liner status append (no conversation)

## Agents

- **writer** – Drafts emails, proposals, scope docs in your voice (reads voice-guide.md, clients.md)
- **reviewer** – Reviews contracts and proposals against playbook positions

## Memory Structure

### business/memory/patterns.md
Updated by /retrospective weekly. Contains:
- Energy Patterns (what times you're productive, what kills focus)
- Productivity Patterns (solo vs. collaborative, context switching cost)
- Avoidance Patterns (what you procrastinate, real blockers vs. surface resistance)
- Client Patterns (communication rhythms, decision timelines, scope management)
- Seasonal Patterns (Q4 intensity, August slowdown, etc.)

### business/memory/clients.md
Client reference for voice/proposal generation:
- Contact and communication preference
- Response time and decision-maker info
- Historical patterns and notes

## Standup Conditional Context

When you run /standup, these ops files are automatically loaded IF they exist:
- `~/ops/daily/staging/` – Overnight prep context (agenda, blockers, setup)
- `~/ops/daily/instructions/` – Per-day special instructions
- `~/personal/plan.md` – Your weekly values/goals (for alignment check)
- Project-specific daily instructions from `~/ops/projects/{project}/daily/`

This creates a consistent "remember the landscape" check before you plan your day.

## Reference

All relative paths use `~` notation (your home directory), not hardcoded paths. Adjust based on your actual clone location.

- Notion integration: /standup syncs to your business Notion workspace after planning
- Revenue tracking: Pulling from ~/ops/projects/ via /weekly-plan and /retrospective
- Deep work: /standup breaks items into micro-steps; daily project files guide focus
- Waiting items: /follow-up surfaces from ops/projects/{project}/waiting_on and flags response gaps

---

**Philosophy:** This system treats your business as a single integrated operating system. Daily ops are sand—the skill invocations are water that shapes the sand. Weekly retrospective is the geology that understands long-term patterns. Memory keeps you from re-learning the same lessons.
