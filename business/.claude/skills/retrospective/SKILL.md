---
name: retrospective
type: skill
description: Friday four-lens review. Analyzes revenue, energy, avoidance, and growth. Updates patterns. Reflects on what worked.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
disable-model-invocation: false
---

# Retrospective Skill

Friday end-of-week analysis through four lenses. Updates your behavioral patterns file so the system learns how you work.

## Setup

Load all files from the week:
1. Daily files: `~/ops/daily/staging/YYYY-MM-DD*.md` (all days this week)
2. Weekly plan: `~/ops/daily/weekly/YYYY-WNN-plan.md`
3. Project files: `~/ops/projects/*.md` (all active projects)
4. Financial calendar: `~/financial/financial-ops.md` (look for this week's financial events)
5. Existing patterns: `~/business/memory/patterns.md` (update this)

## Four-Lens Analysis

Conduct the conversation through these lenses. Each lens should take 5-10 minutes.

### Lens 1: Revenue

"What moved toward payment this week?"

Look at:
- Which milestones progressed?
- What got invoiced?
- What's still blocked on client decisions?
- Any unexpected costs or delays that impacted margins?
- Did we close anything?
- Payment received on any invoices?

**Output**: Note patterns about what generates revenue (recurring work? project work? certain client types?) and what blocks it (scope creep? client delays? your process?).

### Lens 2: Energy

"What correlated with your best work?"

Look at:
- Which days were most productive?
- When did you get into flow?
- What killed your energy?
- When did you have the most focus?
- Did anything unexpected affect energy (weather, sleep, exercise)?
- Days when you resisted work—what was the pattern?

**Output**: Note which conditions, times of day, or activities correlate with high output. Update patterns.md with energy observations.

### Lens 3: Avoidance

"What kept appearing in carried-forward?"

Look at:
- Tasks that didn't move all week
- Items in your inbox that you keep deferring
- Projects where you're stuck in a waiting game
- Work you actively resist
- For each: Is it genuinely low priority, or are you avoiding it for a reason?

**Output**: Distinguish between:
- Legitimately low priority (can drop or defer to later)
- Actual blockers (waiting on someone, missing information, unclear next step)
- Resistance (something about this work repels you—unfamiliar tech, interpersonal friction, uncertainty)

For resistance items: What's the smallest next step? What would make it feel less overwhelming?

### Lens 4: Growth

"What got easier? What did you learn?"

Look at:
- Skills you practiced this week
- Processes you improved
- Things that were hard before but felt smoother now
- New technology or technique you tried
- Client relationships that strengthened
- Anything that surprised you (in a good way)
- Personal goals you made progress on

**Output**: Note what's becoming automatic and what still needs attention. Celebrate small wins.

## Conversation Flow

Have a real conversation, not a form:

1. **Opening** (2 min): "It's Friday. Let's look at the week. How are you feeling about it?"
2. **Revenue lens** (8 min): "What moved toward payment?"
3. **Energy lens** (8 min): "What conditions helped you do your best work?"
4. **Avoidance lens** (8 min): "What didn't move this week? What's the real blocker?"
5. **Growth lens** (8 min): "What got easier? What did you learn?"
6. **Closing** (5 min): "If you could design next week, what would it look like?"

Listen. Push back gently on rationalizations. Help them see patterns they might miss.

## Update patterns.md

After the conversation, update `~/business/memory/patterns.md` with new observations:

```markdown
## Energy Patterns

### Updated Friday [DATE]
- Morning energy (9am-12pm): [What you learned this week]
  - Note: [Any changes from before]
- Context switching: [If you learned something new about this]
- Focus window: [What you discovered]

## Avoidance Patterns

### Updated Friday [DATE]
- [New chronic avoidance pattern noted]
- [Blocker type observed]
- [Workaround that helped]

## Client Patterns

### Updated Friday [DATE]
- [Communication rhythm observation]
- [Scope creep pattern if relevant]

## Productivity Patterns

### Updated Friday [DATE]
- [Time-of-week pattern]
- [Condition that enabled output]
```

Be specific. "Better energy on Tuesdays" is vague. "High focus Tue-Wed mornings, usually crashes Thu, correlates with Monday call stress" is useful.

## Write the Retrospective File

Create `~/ops/daily/weekly/YYYY-WNN-retro.md`:

```yaml
---
week: YYYY-WNN
date_range: "2026-03-17 to 2026-03-21"
---

# Retrospective Week [WNN]

## Revenue Lens

**Milestones completed this week:**
- [Project A] Phase 2 - invoiceable

**Money received:**
- [Amount from which invoices]

**Blocked on client decisions:**
- [Client X] waiting on scope approval (since [date])

**Margins notes:**
- [What you noticed about profitability]

## Energy Lens

**Peak productivity:**
- Tuesday 9-12pm: Deep focus, completed Phase 2 buildout
- Wednesday: High momentum, integration testing

**Energy killers:**
- Thursday: Pollen high, energy crashed by 3pm
- All-day meeting Monday: Context switching overhead

**Correlation findings:**
- Good sleep = productive next day (obvious, but worth noting)
- [Other patterns you noticed]

## Avoidance Lens

**Items in carried-forward (didn't move):**
- Client follow-up for Project B (since Monday)
  - Real blocker: Waiting for their design assets (since 2026-03-10)
  - Action: Set auto-follow-up for [date], it's been 10+ days

- Bookkeeping for Feb (deferred again)
  - Resistance type: Procrastination despite low effort
  - Actual blocker: Receipt scanning is tedious
  - Workaround: Breaking into 15-min sessions instead of "do all bookkeeping"

**New insights:**
- [What you learned about your avoidance patterns]

## Growth Lens

**Skills practiced:**
- Salesforce custom object integration (got clearer with each attempt)
- New discovery process with Acme Corp (smoother by Friday)

**Processes improved:**
- Daily standup now takes 10min vs 20min (getting the hang of quick decisions)

**Relationships strengthened:**
- Acme Corp happy with progress, already discussing next phase

**Personal wins:**
- Ran 4 times this week (vs target 3)
- Started blog post (first paragraph done)

## Next Week

**3-5 objectives:**
1. [Outcome-focused goal]
2. [Outcome-focused goal]
3. [Outcome-focused goal]

**What you want next week to feel like:**
[Your response to closing question]

**Key dates:**
- [Milestone due dates]
- [Financial deadlines]

**Focus areas:**
- [What gets priority]
```

## Closing Question

End with: "If you could design next week, what would it look like? What would make it successful?"

Listen for:
- Realistic energy levels
- Balance between client work and personal priorities
- Any recurring frustrations that need addressing
- What energizes them

This becomes input for Monday's `/weekly-plan` skill.

## Tone & Philosophy

- **Curious, not judgmental:** Why didn't something move? Assume there's a valid reason.
- **Pattern-focused:** One week is noise. Over weeks, patterns emerge. Help see the longer arc.
- **Permission-granting:** It's okay if not everything moved. What mattered? What didn't?
- **Growth-oriented:** What's getting easier? What skills are sharpening?

This isn't performance review. It's learning. The system gets smarter by understanding how you actually work, not how you think you should work.

---

**Generated file location:** `~/ops/daily/weekly/YYYY-WNN-retro.md`
**Updates:** `~/business/memory/patterns.md`
**Precedes:** Monday `/weekly-plan` skill
**Output:** Weekly patterns in memory for /standup to use next week
