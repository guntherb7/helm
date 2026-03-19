# Personal Setup

Set up your personal operations workspace. This is foundational—take time with it.

## Phase 1: Define Your Plan (30 min)

Create `plan.md` with what actually matters to you.

Not what sounds good on a vision board. What genuinely matters.

**Sections to write:**
1. **This Year's Focus** – What's the one thing? (2-3 sentences)
2. **Values** – 3-5 things that guide your decisions
3. **Health Goals** – What does healthy look like? (physical, mental, emotional)
4. **Personal Goals** – What do you want to accomplish? (not business)
5. **What Success Looks Like** – Define it yourself

**Tips:**
- Write it honestly. No one else reads it.
- Specific is better than vague. "Run 3 times a week" beats "get fit"
- Include what matters beyond achievement (relationships, rest, curiosity)
- Revisit quarterly—your priorities may shift

**Example structure:**

```markdown
# My Operations Plan

## This Year's Focus
Finish my book on personal operations. I've had the idea for 3 years.
Spend quality time with my family weekly, not just in the margins.

## Values
1. **Autonomy** – I want control over my time and decisions
2. **Mastery** – I like getting good at things
3. **Family** – Time with people I love matters most
4. **Health** – Physical health enables everything else
5. **Curiosity** – Learning new things energizes me

## Health Goals
- Exercise 4 times a week (mix of running, strength, flexibility)
- Sleep 7-8 hours consistently (currently 6-6.5)
- Manage seasonal allergies without them controlling my schedule
- Energy management: know my rhythms and work with them, not against them

## Personal Goals
- Write and finish my book (at least 50k words by September)
- Take a week-long trip with family (summer)
- Learn photography better (take a course)
- Read 12 books (currently at 3)

## What Success Looks Like
- Working 30-35 hours a week (not 50+)
- Family time not squeezed into evening exhaustion
- Making progress on the book, even if slow
- Feeling energized, not constantly tired
- Doing work that matters to my values, not just pays the bills
```

## Phase 2: Populate Personal Operations (20 min)

Create `personal-ops.md` with your actual ongoing tasks and habits.

**Sections:**
1. **Active** – Things you're working on right now
2. **Recurring** – Regular habits and commitments
3. **Health** – Exercise, sleep, habits that affect energy
4. **Avoiding** – Things you know you should do but keep putting off

**For each task, include:**
- `task`: What you're doing
- `frequency`: How often (daily, weekly, monthly)
- `effort`: Time/energy required (15min, medium, high)
- `window`: When it needs to happen (before April, weekly Tuesdays)
- `pairs_with`: Best conditions (nice weather, good energy, after coffee)
- `goal`: Why you're doing this
- `status`: active, on-hold, recurring

**Example:**

```yaml
## Active
- task: Blog post on personal operations
  effort: medium
  window: Publish by end of month
  frequency: ongoing
  goal: Share the system with others
  pairs_with: high focus energy (morning, weekday)

- task: Spring garden planning
  effort: medium
  frequency: weekly (Sundays)
  window: Before April 15
  goal: Have garden ready for planting season
  pairs_with: nice weather, outdoor energy

## Recurring
- task: Weekly 1-on-1 with team
  frequency: weekly
  day: Tuesday 10am
  effort: 30min
  goal: Maintain communication and feedback loop

- task: Standup with partner
  frequency: daily (Sundays)
  effort: 30min
  goal: Sync on family, week ahead, blockers

- task: Therapy
  frequency: weekly
  day: Thursday 2pm
  effort: 1hour
  goal: Mental health, processing, clarity

## Health
- task: Running
  frequency: 4x weekly
  effort: 45-90min per run
  days: Mon, Wed, Fri, Sat (or condition-based)
  goal: Fitness, mental health, energy management
  pairs_with: nice weather, morning energy

- task: Strength training
  frequency: 2x weekly
  effort: 30-45min per session
  days: Tue, Thu
  goal: Strength, injury prevention
  pairs_with: after good sleep, moderate-to-high energy

- task: Stretching/mobility
  frequency: daily
  effort: 15min
  goal: Prevent stiffness, reduce shoulder tension
  pairs_with: evening wind-down

- task: Sleep target
  frequency: daily
  window: 7-8 hours
  goal: Support all other health goals
  pairs_with: consistent bedtime, no screens after 10pm

## Avoiding
- task: Organize home office
  status: avoiding (know why: perfectionism, "not important enough")
  effort: high
  actual_blocker: Waiting for new furniture to arrive

- task: Call Mom
  status: recurring (but often delayed)
  effort: 30min
  frequency: weekly
  goal: Stay connected
  pairs_with: No special conditions, just commit the time
```

## Phase 3: Set Up Inbox Capture (10 min)

Create `inbox.md` for quick captures throughout the day:

```markdown
# Inbox

Captures from phone and throughout the day. Emptied each morning standup.

[Items will be added here, routed to appropriate places by standup]
```

**How to use:**
- When an idea pops up: capture it to Apple Notes "Inbox"
- When you remember something: add to inbox
- When you want to do something but not now: add to inbox
- Each morning: standup reads and processes inbox

**Examples of inbox items:**
- "Call the dentist about the crown"
- "Read that article on Obsidian"
- "Plan a date night with partner"
- "Ask boss about the promotion timeline"
- "Buy flour and eggs"

Inbox is a capture-without-judgment space. It's not a todo list—it's a holding pen that gets processed daily.

## Phase 4: Set Up Health Tracking (10 min)

Create `health/tracking.md` with what you actually track:

```markdown
# Health Tracking

## Medications/Supplements
- [List any regular medications or supplements]

## Exercise
- Target: 4x running + 2x strength per week
- Tracking: Use [strava/fitbit/whatever you use]

## Sleep
- Target: 7-8 hours
- Tracking: [How you track sleep]

## Energy Patterns
- Best: Morning, after good sleep
- Worst: Afternoons on high-pollen days
- Seasonal: Struggle with motivation in winter

## Allergy Management
- High season: March-May (tree pollen), August-Sept (ragweed)
- Triggers: Oak, birch, grass, ragweed
- Management: [What helps – medicine, indoor days, etc.]
```

**Don't overcomplicate this.** Track only what's actionable.

If tracking sleep doesn't change your behavior, don't track it. If knowing pollen counts helps you choose indoor days, track it.

## Phase 5: Connect to Standup (5 min)

Verify standup loads personal context:

```bash
cd ~/helm/business
claude skill standup
```

In the conversation, it should ask: "Any personal goals or tasks you want to weigh in today?"

If not, check that personal/plan.md exists and is readable.

## Quarterly Review

Every 3 months, revisit `plan.md`:
- Are these goals still true?
- Should you adjust focus?
- What's changed about your values?

Your plan should evolve as you do.

---

**Quick Checklist:**

- [ ] `plan.md` written with honest goals and values
- [ ] `personal-ops.md` populated with active tasks
- [ ] `inbox.md` created (empty for now)
- [ ] `health/tracking.md` created with what you actually track
- [ ] Run `/standup` and confirm it loads personal context
- [ ] Apple Notes "Inbox" note created on your phone (optional but helpful)

---

**Philosophy:**

Personal operations are as important as business operations. If you're burned out and unhappy, the business doesn't work either. This workspace makes sure personal wellbeing is integrated into daily planning, not an afterthought.
