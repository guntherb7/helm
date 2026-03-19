---
name: follow-up
type: skill
description: Check waiting_on items past interval, detect communication gaps, draft follow-up messages in your voice.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
disable-model-invocation: false
---

# Follow-Up Skill

Detects stalled items and communication gaps, then drafts follow-up messages in your voice.

## What It Checks

### 1. Waiting On Items Past Interval

Look through all project files `~/ops/projects/*.md`:

For each `waiting_on` entry, check:
```yaml
waiting_on:
  - what: Design assets from client
    who: Acme Corp
    since: 2026-03-10
    follow_up_interval_days: 7
    last_follow_up: 2026-03-15
```

**Is it overdue?** If (today - last_follow_up) > follow_up_interval_days, it's overdue.

Example:
- Since: March 10
- Interval: 7 days
- Last follow-up: March 15
- Today: March 19
- Days since last follow-up: 4 days
- Status: Not overdue yet, but close (follow up on March 22)

### 2. No Communication in 7+ Days

Check all projects for communication_log (if it exists):

```yaml
communication_log:
  - date: 2026-03-12
    type: email
    to: jane@acme.com
    subject: "Phase 1 status update"
```

If project is active and last communication was 7+ days ago → suggest follow-up.

### 3. Milestones Near Completion

Check for milestones that are in "almost done" status:

```yaml
contract:
  milestones:
    - name: Phase 1
      status: in-progress
      deliverables:
        - Feature A: complete
        - Feature B: complete
        - Feature C: 95% complete
```

If all deliverables are done or nearly done → prepare for delivery communication.

## Report Format

Display what needs follow-up:

```
FOLLOW-UP ITEMS

[Project A] – Acme Corp
  Status: Awaiting design assets
  Since: March 10 (9 days)
  Last follow-up: March 15 (4 days ago)
  Action: Send friendly follow-up request

[Project B] – Example Client
  Status: No communication in 7 days
  Last contact: March 12 (7 days ago)
  Action: Send status check-in

[Project C] – Other Corp
  Status: Phase 1 nearly complete
  Complete: 95%
  Action: Prepare delivery notification
```

## Draft Messages

For each item needing follow-up, draft a message using the writer agent:

```
Invoke writer agent with:
- Project name and client
- Current status
- What you're asking for
- Timeline if relevant
- Tone: friendly, professional, not pushy

Agent reads:
- ~/business/memory/clients.md (client communication preference)
- ~/ops/templates/voice-guide.md (your voice)
- Drafted message should match your writing style
```

### Message Types

**Waiting on deliverable:**
```
Hi [Contact],

Quick check-in on the [Deliverable] we discussed—we're ready to integrate
your [deliverable type] into Phase 1 as soon as you can share it.

If there's anything blocking this on your end, just let me know. I'm happy
to adjust the timeline if needed.

Best,
[Your name]
```

**Status check-in (no recent communication):**
```
Hi [Contact],

Hope things are going well on your end. Just wanted to touch base on
[Project Name]—where we stand on [current phase/milestone].

Happy to jump on a quick call if you have questions or want to discuss
next steps.

Best,
[Your name]
```

**Delivery notification (milestone nearly complete):**
```
Hi [Contact],

Great news—Phase [X] is nearly complete. We're on track for delivery on
[target date].

I'll have [specific deliverables] ready for your review by [date]. When
you've had a chance to test, let's schedule a brief walkthrough.

Best,
[Your name]
```

## Save Drafts

Save drafted messages to a follow-up folder:

```bash
mkdir -p ~/business/communications/follow-ups/
# File location: ~/business/communications/follow-ups/YYYY-MM-DD-{project-name}.md
```

Include:
- Who you're following up with
- Current status
- What you're asking
- Drafted message
- When to send (immediate, or after [date])

## Update Project File

After sending, update the project ops file:

```yaml
waiting_on:
  - what: Design assets
    who: Acme Corp
    since: 2026-03-10
    follow_up_interval_days: 7
    last_follow_up: 2026-03-19  # Updated today
    status: follow-up-sent
```

And add to communication_log:

```yaml
communication_log:
  - date: 2026-03-19
    type: email
    to: jane@acme.com
    subject: "Check-in: Design assets for Phase 1"
    status: awaiting-response
```

## Conversation Flow

This skill should:

1. **Report** what's overdue
2. **Ask** if each follow-up is still needed
   - "Should we still wait on the design assets, or have they changed their timeline?"
   - "Is Project B still active, or should we archive it?"
3. **Draft** messages for items you confirm
4. **Ask** if you want to send now or queue them
5. **Save** drafts and update project files

Let you have control. Don't auto-send messages. Draft, review, then you send.

## Follow-Up Cadence

Suggested intervals by situation:

| Situation | Interval | Reminder |
|:----------|:---------|:---------|
| Waiting on client decision | 7 days | Every week until responded |
| Waiting on deliverable | 5 days | More frequent (blocks you) |
| Waiting on feedback | 3 days | Very blocking |
| Long-term project (monthly retainer) | 14 days | Every other week |
| Inactive project (on hold) | 30 days | Monthly check-in |

Update follow_up_interval_days in your project files to match your cadence preference.

## When NOT to Follow Up

Skip follow-up if:
- Client explicitly said "we'll reach out when ready" (respect their timeline)
- You're waiting on third party (API provider, etc.) and follow-up won't help
- It's a passive waiting situation (their decision, not blocking you)
- Item is marked "blocked indefinitely" (acknowledge reality)

Update project file with explicit status instead:

```yaml
waiting_on:
  - what: API documentation from Stripe
    who: Stripe (third-party)
    since: 2026-03-01
    status: blocked-on-vendor
    note: Waiting for Stripe to publish docs; not actionable on our end
```

## Revenue-Weighted Follow-Up

Prioritize follow-ups that move revenue:

1. **Highest priority:** Waiting on client approval to bill a completed milestone
2. **High priority:** Waiting on deliverable that unblocks your milestone
3. **Medium priority:** Status check-in on active project
4. **Lower priority:** Checking on project on hold or back-burner

The skill should surface revenue-blocking items first.

---

**Reads:**
- ~/ops/projects/*.md (all project ops files, especially waiting_on sections)
- ~/business/memory/clients.md (client communication preferences)
- ~/ops/templates/voice-guide.md (your voice)

**Writes:**
- ~/business/communications/follow-ups/ (drafted messages)
- Updates ~/ops/projects/*.md (last_follow_up date, status)

**Calls:** writer agent (to draft messages in your voice)

**Scope:** Detection + drafting. You handle sending.

**Cadence:** Run manually as needed, or could be called as part of daily standup to surface overdue items.
