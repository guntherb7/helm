# Personal – Life Operations

This workspace tracks your personal life, goals, health, and non-business tasks. It's read by the `/standup` skill every morning so personal items get weighed against business priorities.

## Structure

```
personal/
  plan.md              Your values, goals, what matters
  personal-ops.md      Active tasks, recurring habits, personal projects
  inbox.md             Quick captures from phone throughout the day
  health/
    tracking.md        Health metrics, exercise, sleep, energy
```

## How It Works

**Daily Standup Integration:**

The `/standup` skill reads:
- `plan.md` – Your goals and values (for alignment check)
- `personal-ops.md` – Active tasks, habits, personal projects
- `health/tracking.md` – Energy patterns, conditions that affect focus

This helps the standup match personal tasks to your energy state and conditions.

**Example:**
- Low energy day? Suggest personal admin tasks instead of deep work
- High pollen day and you track allergies? Suggest indoor tasks
- Running is a personal goal? Suggest it on nice-weather days

## Key Concepts

### Values & Goals

`plan.md` captures what actually matters to you. Not what you think should matter—what genuinely matters to you.

The standup reads this every morning to check: "Are we doing work that aligns with your values?"

Example:
- Value: "Time with family"
- Goal: "Spend 5+ hours with kids this week"
- Standup asks: "What does this week look like for family time?"

### Personal Tasks with Metadata

Personal tasks carry more detail than typical to-do items:

```yaml
- task: Plan cycling routes for spring
  added: 2026-03-05
  goal: Have routes mapped before outdoor season
  window: Before April 15
  effort: medium
  pairs_with: nice weather day, good energy
  status: active
```

The standup uses this metadata to:
- Match effort level to available energy
- Pair outdoor tasks to good weather days
- Suggest tasks in the right season/window

### Inbox Captures

Throughout the day, capture quick thoughts to `inbox.md`:
- Ideas that pop up
- Things you want to do but not right now
- Personal reminders

The `/standup` processes inbox items daily, routing them to the right place.

## Two-Tier Memory

**Working Memory (this workspace):**
- Current goals and values
- Active personal tasks
- This week's focus
- Health/energy status

**Deep Storage (memory/ in business/):**
- Long-term patterns (learned from retrospective)
- Seasonal trends (when you typically have energy, when things get hard)
- Relationship notes (not here, for privacy)

## Philosophy

Personal operations are just as important as business operations. The system doesn't treat them as "extra" or "if you have time."

The standup asks: "What matters to you today?" and plans around that, not after business is done.

This keeps you from burning out and builds a life you actually want to live—not just a business that consumes your time.

---

**How Personal Fits Into HELM:**

- **Daily:** `/standup` loads personal plan + tasks + health
- **Weekly:** `/weekly-plan` includes personal goals in objectives
- **Weekly:** `/retrospective` reviews energy + energy patterns
- **Always:** Personal tasks weighted with business tasks, not separate

Your life is integrated into your operations system, not partitioned off.

---

**Related Files:**
- `~/ops/projects/` – Business projects (read by standup for business context)
- `~/business/memory/patterns.md` – Long-term patterns including personal energy (updated by retrospective)
- `~/financial/financial-ops.md` – Financial health (impacts peace of mind, considered in personal planning)
