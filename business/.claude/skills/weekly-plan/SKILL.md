---
name: weekly-plan
type: skill
description: Monday strategic planning. Sets 3-5 weekly objectives. Loads retro, project statuses, financial calendar, personal goals.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
  - Task
disable-model-invocation: false
---

# Weekly Plan Skill

Strategic planning for the week ahead. Sets direction, allocates resources, identifies dependencies.

Run this: **Monday morning, before daily standup**

## Opening

"Let's set the week. What's the landscape look like?"

Conversational, not prescriptive. You're thinking together.

## Load Context

In this order:
1. **Friday retrospective** – `~/ops/daily/weekly/retrospective.md` (what you learned last week)
2. **All project statuses** – `~/ops/projects/*.md` (current focus, blockers, waiting_on, payment proximity)
3. **Financial calendar** – `~/financial/financial-ops.md` (invoices due, tax dates, payment schedules)
4. **Personal goals** – `~/personal/plan.md` (weekly values, health goals, personal projects)
5. **Energy patterns** – `~/business/memory/patterns.md` (what weeks tend to look like, seasonal factors)
6. **Client calendar** – Extract from project ops files: meetings, deliverable dates, decision deadlines

## Analyze the Week

Ask:
- "What's the constraint this week?" (deadline? client decision? team availability?)
- "What sets us up for next month?" (planning, scoping, delivery prep?)
- "What's broken that we should fix?" (from retro friction list)
- "What needs deep focus?" (what can't be interrupted or delegated)
- "What's a quick win?" (high impact, low friction)

Show the landscape with **three lenses**:

### Revenue Lens
```
REVENUE PRIORITY (weighted by payment proximity + milestone impact):
1. [Project A] – $15k on milestone delivery (due Wed)
   Impact: Unblock next $8k milestone
   Risk: [If any]

2. [Project B] – $5k retainer invoice (due Fri)
   Impact: Monthly cash flow
   Risk: [If any]

3. [Project C] – Proposal due Mon (worth $25k if won)
   Impact: Pipeline for Q2
   Risk: [If any]
```

### Capacity Lens
```
TIME AVAILABLE:
- Mon-Wed: Deep work available (planned project time)
- Thu: Admin + meetings (reviews, planning, sync calls)
- Fri: Light (retro, planning for next week)

CONTEXT SWITCHING COST:
- If you're context switching between [Project A] and [Project B], cost is [time + friction]
- Recommendation: Batch by project where possible
```

### Pattern Lens
```
ENERGY & PATTERNS:
- Your energy typically [pattern from memory]
- This week's constraint is [weather, personal commitments, seasonal factor]
- You tend to avoid [avoidance pattern] under deadline pressure
- Recommendation: [Preventive structure]
```

## Set 3-5 Weekly Objectives

Based on landscape analysis, define weekly OKRs. Format:

```
WEEKLY OBJECTIVES

1. [Objective Name] – [Revenue/Capacity/Growth/Health]
   Key result: [Measurable completion state]
   Why this week: [What makes it urgent/important]
   Micro-allocation:
     - Mon: [Specific work]
     - Tue: [Specific work]
     - Wed: [Specific work]
     - Thu: [Admin/other]
     - Fri: [Buffer/catch-up]
   Blockers or dependencies: [What needs to not get in the way]
   Success = [Concrete, checkable state]

2. [Objective Name] – [Type]
   Key result: [Measurable completion state]
   ...
```

**Criteria for 3-5 objectives:**
- Each is achievable in one week (not "finish project," but "complete milestone")
- Each has clear completion state (not "work on")
- Together they move revenue, fix friction, or build towards next phase
- They're realistic given energy, capacity, and constraints

## Resource Allocation

Ask: "Where should the best hours go?"

Create a rough weekly schedule:

```
WEEKLY SCHEDULE (rough)

DEEP WORK BLOCKS (protect these):
- Mon-Tue: 9-12, 2-5 → [Project A] work
- Wed: 9-12 → [Project A] work (if continuation needed)
- Wed: 2-4 → [Project B] async review

COMMUNICATION/ADMIN:
- Mon 1-2pm → Client sync [Project A]
- Tue 4-5pm → Follow-up calls
- Thu → Review, admin, checklists
- Fri → Retro, next-week planning

BLOCKS/CONSTRAINTS:
- [Any meetings already scheduled]
- [Any personal commitments]
- [Any energy dips to plan around]
```

This is loose—daily standup will adjust. But it prevents "I'll figure it out" scrambling.

## Dependency Mapping

For each weekly objective, ask:
- "What do I need from others?" (client decision, team delivery, external resource)
- "Who's waiting on me?" (what blocks them)
- "What's my wait time?" (how long until response)

Create a simple map:

```
DEPENDENCIES & WAITING

I'm waiting on:
- [Client X] for [decision/feedback] – Needed by [date] for [objective]
  Follow-up strategy: [When to ping if no response]
- [Team member Y] for [deliverable] – Needed by [date]
  Backup: [What if it's late]

Others are waiting on:
- Me for [deliverable] → [Who needs it] → Due [date]
  Status: [On track / At risk]
- Me for [decision] → [Who needs it] → Due [date]
  Status: [Ready to decide / Need more info]

Waiting time:
- Client typically responds in [days] (from patterns)
- If I don't hear by [day], I'll follow up on [day]
```

## Write Weekly Plan File

Create `~/ops/daily/weekly/plan-{week-number}.md`:

```yaml
---
week: YYYY-W## (ISO week number)
date-range: Mon YYYY-MM-DD to Fri YYYY-MM-DD
objectives: [Number of objectives]
---

# Weekly Plan

## Landscape Analysis
[Copy revenue/capacity/pattern lenses from above]

## Weekly Objectives
[Copy all 3-5 objectives with allocations]

## Resource Allocation
[Copy weekly schedule]

## Dependency Mapping
[Copy dependencies and waiting items]

## Weekly Checkpoints

### Monday
- All objectives clear
- Deep work blocks protected
- Daily standup: Align to first week objective

### Wednesday (mid-week)
- Check: On track for weekly OKRs?
- Adjust if: New information, blocker emerged, misestimated time
- Accelerate if: Ahead of schedule
- De-risk if: At-risk items detected

### Friday (retrospective)
- Weekly retro uses this as reference
- What did we actually achieve?
- What blocked progress?
- What patterns showed up?
```

## Close

"You've got [number] clear objectives. Let's start with Monday's standup and make it real."

## Philosophy

**Why this works:**
- Three-lens analysis prevents tunnel vision (revenue OR capacity OR growth—actually all three)
- 3-5 objectives is constraint; forces prioritization
- Micro-allocation prevents "I'll figure it out" thrashing
- Dependency mapping surfaces hidden blockers
- Protective language ("deep work blocks") makes it explicit what matters
- Mid-week checkpoint enables course correction

**What this prevents:**
- "What was the week for?" (objectives are written)
- Missed deadlines from dependency blindness (waiting list is explicit)
- Energy waste on low-impact work (landscape is weighted)
- Context-switching thrashing (schedule is rough but intentional)
- Friday surprise ("I thought we'd have that done")

**Neurodivergent defaults:**
- Objectives prevent vague "be productive" overwhelm
- Schedule is loose (not rigid) but structured
- Energy patterns are accounted for
- Deep work blocks are protected language
- Capacity is realistic (not aspirational)

---

**allowed-tools:** Read, Write, Bash, Glob, Grep, Task
