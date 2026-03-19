---
name: wrap
type: skill
description: End-of-day review. Writes what actually happened and carried forward. Pre-stages tomorrow context.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
  - Task
disable-model-invocation: false
---

# Wrap Skill

End-of-day review and reflection. Captures what happened, what carries forward, and stages tomorrow's context.

## Opening

Warm, reflective tone: "Let's close out today. What actually happened?"

## Load Context

In this order:
1. **Today's plan** – `~/ops/daily/staging/YYYY-MM-DD.md` (what you aimed for)
2. **All project ops files** – `~/ops/projects/*.md` (to check status changes)
3. **Today's project daily files** – `~/ops/projects/{project}/daily/YYYY-MM-DD.md` (what you actually did per project)
4. **Patterns** – `~/business/memory/patterns.md` (to spot today's themes)

## Conversation

Ask briefly (not an interrogation):
- "What did you actually get done?"
- "What got in the way?"
- "Anything you want to carry forward?"
- "How are you feeling about tomorrow?"

Listen for:
- Accomplishments (celebrate them, even small ones)
- Friction (is it systematic? one-off? self-imposed?)
- Energy state at end of day
- Psychological load (worry, unfinished feeling, relief)

## Write Wrap File

Create `~/ops/daily/staging/YYYY-MM-DD-wrap.md`:

```yaml
---
date: YYYY-MM-DD
closing-energy: [1-10 assessment]
---

# End-of-Day Wrap

## What Actually Happened

### Completed
- [Task/milestone with time spent if tracked]
- [Task with blockers encountered]
- [Unexpected work that showed up]

### In Progress (not finished)
- [What's staged for tomorrow]
- [Why it didn't finish (time/blocker/energy/scope)]

### Context Switching
[If you multitasked: what switched, why, impact]

## Carried Forward

### To Tomorrow
- [Unfinished item with state]
- [Task with new information]
- [Decision that needs morning clarity]

### To Later
- [Item that moved to someday pile]
- [Item dependent on response]

## Pattern Observations

### Energy
- Started: [State from standup]
- Ended: [Current state]
- Dips: [When/why energy dropped]
- Peaks: [What created momentum]

### Productivity
- Deep work windows: [When, how long, what enabled it]
- Context switching cost: [Disruptions, impact]
- Flow state: [Did you reach it? When?]

### Friction
- Self-imposed obstacles: [What you resisted]
- External blockers: [What you waited on]
- Decision paralysis: [Any decision you avoided]

## Project Status Updates

[For each project you touched today:]
```
### [Project Name]
- Milestone: [Current milestone]
- Status: [On track / At risk / Blocked] (updated from plan)
- What changed: [Progress or new blocker]
- Tomorrow's focus: [Next micro-step]
```

## Tomorrow's Pre-Stage

Create `~/ops/daily/instructions/YYYY-MM-DD.md` (for tomorrow morning):

```yaml
---
date: YYYY-MM-DD (tomorrow)
prep: [What you're pre-staging]
---

# Tomorrow's Context

## Carried Forward
[Items from "To Tomorrow" section above, as context]

## Overnight Blockers
[Anything that needs resolution before tomorrow's standup]

## Client Response Windows
[Any emails you're waiting on overnight]

## First Thing
[What should tomorrow morning focus on]

## Notes from Today
[Patterns or decisions for morning self]
```

## Close

Acknowledge what happened today:
- "You [specific accomplishment]. That matters."
- "Tomorrow's staged. You've got [carried-forward items] to address."
- "You're carrying [energy state]. That's information for tomorrow planning."

Brief, honest, grounded.

## Philosophy

**Why this works:**
- Writing "what actually happened" creates retrieval for retrospective analysis
- Separated "What Happened" and "Carried Forward" prevents overwhelm
- Pattern observations get fed to /retrospective (energy, friction, flow)
- Pre-staging tomorrow prevents tomorrow's first context-build delay
- Energy check creates data for weekly patterns

**What this prevents:**
- "I did nothing today" spiral (you probably did, just not what you planned)
- Carried-forward anxiety (it's written, it won't be forgotten)
- Tomorrow confusion (context is already staged)
- Pattern blindness (writing observations makes them visible)

**Neurodivergent defaults:**
- Don't judge the day, describe it
- All carry-forward is externalized
- Tomorrow is pre-contextualized (reduces morning startup cost)
- Energy tracking is normalized (not a weakness)
- Friction is pattern-tracked (helps identify real blockers vs. resistance)

---

**allowed-tools:** Read, Write, Bash, Glob, Grep, Task
