---
name: standup
type: skill
description: Morning planning conversation. Loads overnight context, inbox, landscape, extracts structured check-in, surfaces priorities, writes daily ops files, syncs to Notion.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
  - Task
disable-model-invocation: false
---

# Standup Skill

Morning planning and prioritization conversation. Creates your daily operations structure.

## Opening

Greet with warmth: "Good morning. How are you doing today?"

Wait for response. This isn't a formality—listen for energy, mood, and what's actually on their mind.

## Structured Check-In

Extract (conversationally, not like a form):
- **Energy**: 1-10 scale. What's your baseline today? Any physical constraints?
- **Mood**: What's the emotional weather? Any lingering friction from yesterday?
- **Focus**: Can you do deep work today? Are you context-switching?
- **Physical**: Sleep? Hunger? Movement? Caffeine status?
- **Creative**: Are you in a generative or maintenance headspace?

These are used to weight which items surface and how to frame them.

## Load Context

In this order:
1. **Overnight staging** – `~/ops/daily/staging/` for any prepared context (blockers, urgent items, setup notes)
2. **Daily instructions** – `~/ops/daily/instructions/` for special directives for today
3. **Weekly plan** – `~/personal/plan.md` for weekly values/goals (alignment check)
4. **Project status** – All files in `~/ops/projects/*.md` (current focus, blockers, waiting_on status)
5. **Patterns** – `~/business/memory/patterns.md` (energy/productivity insights)
6. **Inbox** – `~/ops/inbox.md` if it exists (unprocessed items)

## Process Inbox

Read inbox items. Ask: "What's actually urgent? What's noise? What needs a decision?"

Sort into:
- **Today**: Must do today (payment due, client response, blocker removal)
- **This week**: Should do before Friday
- **Waiting**: Depends on someone else (flag for /follow-up)
- **Someday**: Nice-to-have, low priority
- **Trash**: Outdated, no longer relevant

Update `~/ops/inbox.md` to reflect processing. Clear processed items.

## Surface Landscape

Show all projects weighted by:
1. **Payment proximity** (client paying in next 5 days? top)
2. **Deadline** (delivery due soon?)
3. **Blocker status** (things you're waiting on that unblock others)
4. **Energy fit** (what matches today's energy state)

Format:
```
LANDSCAPE (weighted by payment proximity + deadline + energy fit):
1. [Project A] – Milestone delivery due Fri. $8k on completion. Energy fit: deep work. Blocker: design feedback.
2. [Project B] – Waiting on client response for contract. $5k next milestone. Energy fit: async. No blockers.
3. [Project C] – Weekly check-in. $2k/month retainer. Energy fit: communication. Low urgency.
```

## Suggest 2-4 Daily Items

Based on:
- Landscape prioritization
- Energy state match
- Time available today
- Micro-step feasibility

For each item, break into **micro-steps** (15-30 min each):
```
[Project A] – Move milestone to completion
  → Step 1: Integrate design feedback into codebase (30 min)
  → Step 2: Run test suite, document failures (20 min)
  → Step 3: Prepare delivery package and email (15 min)
  → Step 4: Sync with client on timeline (10 min)
```

The micro-steps prevent "I'll work on Project A" paralysis. They create momentum.

## Write Daily File

Create `~/ops/daily/staging/YYYY-MM-DD.md` with:

```yaml
---
date: YYYY-MM-DD
energy: [1-10 assessment]
mood: [description]
focus: [deep work / shallow / mixed]
---

# Daily Plan

## Check-In
- Energy: [summary]
- Mood: [summary]
- Physical: [summary]
- Constraints: [if any]

## Today's Landscape
[Copy landscape section from above]

## Today's Priorities
1. [Item + micro-steps]
2. [Item + micro-steps]
3. [Item + micro-steps, if time]

## Project-Specific Daily Instructions
[See below]

## Carried Forward
- [From yesterday's /wrap, if any]

## Waiting On
[Items flagged as blocked; these go to /follow-up]

## Notes
[Any context for afternoon self-review]
```

## Generate Per-Project Instruction Files

For each project in today's plan, create:
`~/ops/projects/{project-name}/daily/YYYY-MM-DD.md`

Content:
```yaml
---
project: [Project Name]
date: YYYY-MM-DD
milestone: [Current milestone name]
---

# Daily Instruction: [Project Name]

## Today's Focus
[From standup plan, project-specific details]

## Micro-Steps
[Copy from daily plan, project-specific version]

## Milestone Checkpoint
- Milestone: [Name]
- Status: [On track / At risk / Blocked]
- Deliverables due: [Date]
- What unblocks this: [Key dependency or decision]

## Reference
- Scope: [Link or reference to ops/projects/{project-name}.md]
- Recent activity: [Last 2-3 entries from project's communication_log]
```

These files are read by project-level /done skill at end of day.

## Sync to Notion

After planning:
- Write plan to your Notion Business database
- Create task items for each priority
- Update project status cards
- Sync inbox status

If Notion sync fails (plugin not configured), note it but don't block—daily file still created.

## After Planning

Leave the session open. Ask: "What should we handle first?"

Sit with the user through:
- Biggest or scariest task (break it down further if needed)
- Unfamiliar technical setup (pair through first steps)
- Decision requiring context (explore it together)
- Context switching (help find focus window)

Don't script the work—be present as thinking partner for the first 15-30 minutes of their day.

## Philosophy

**Why this works:**
- Check-in aligns work to actual state (not aspirational state)
- Landscape shows what matters (payment, deadline, blocker-removal)
- Micro-steps prevent cognitive shutdown
- Daily file creates retrieval cue for afternoon /wrap
- Per-project files keep project context from leaking into daily context
- Session stays open because first work session often needs support

**Neurodivergent defaults:**
- All lists are externalized (not in head)
- Micro-steps prevent planning paralysis
- Energy check prevents "why am I resisting?" frustration
- Physical/creative state matters as much as task list
- Session stays open to prevent stuck-starting
- Weight by payment proximity (concrete motivation)

---

**allowed-tools:** Read, Write, Bash, Glob, Grep, Task
