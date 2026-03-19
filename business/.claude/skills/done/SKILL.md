---
name: done
type: skill
description: Manual project update fallback. Reads project ops file, asks what changed, updates milestone/activity status, checks completion.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
disable-model-invocation: false
---

# Done Skill

Manual fallback for updating project status. Use this when you want to manually log what happened on a project, or when the SessionEnd hook doesn't capture context.

## Phase 1: Select Project

Ask: "Which project are we updating?"

Show list of active projects from `~/ops/projects/`:

```bash
ls ~/ops/projects/*.md | xargs -I {} basename {} .md
```

Load selected project file and display:
- Current status
- Active milestone
- Deliverables and their status
- Waiting_on items
- Recent activity

## Phase 2: What Changed?

Conversation: "What actually happened on this project?"

Listen for these categories of changes:

**Code/Deliverable Completed:**
- "Finished the payment integration"
- "All tests passing"
- "Feature is ready for review"

**Deliverable Status Updates:**
- "Feature A is 80% done"
- "Waiting on their design assets before I can proceed"

**Client Communication:**
- "Got approval for scope change"
- "They asked to add X feature"
- "Clarified their timeline for Phase 2"

**Blockers:**
- "Stuck on API documentation"
- "Their server is down, can't test"
- "Need decision on technical approach"

**Scope Changes:**
- "They want to add X (not in contract)"
- "We agreed to descope Y"

## Phase 3: Update Milestone Status

For the active milestone, ask about each deliverable:

```
Current Milestone: Phase 1 – Integration & Setup

Deliverables:
- Database schema: [current status?]
- API endpoints: [current status?]
- Admin interface: [current status?]
- Deployment setup: [current status?]
```

Status options: not-started, in-progress, complete, blocked, needs-review

Update the ops file:

```yaml
milestones:
  - name: Phase 1
    status: in-progress  # or complete if all deliverables done
    deliverables:
      - Database schema: complete
      - API endpoints: in-progress
      - Admin interface: not-started
      - Deployment setup: blocked
```

## Phase 4: Check Milestone Completion

If all deliverables for the active milestone are "complete":

Ask: "Is this milestone ready to send to the client for review?"

If yes, update status:

```yaml
status: ready-for-review
closing:
  review_complete: false
  client_approved: false
  invoice_sent: false
  payment_received: false
```

## Phase 5: Update Waiting Items

Ask: "Are you waiting on anything from the client or elsewhere?"

For each waiting item:

```yaml
waiting_on:
  - what: [What you're waiting for]
    who: [Who has it]
    since: [When you started waiting]
    follow_up_interval_days: [How often to check]
    status: [open, follow-up-sent, escalated]
    last_follow_up: [Last time you reached out]
```

Examples:
- "Waiting on design assets from client" → waiting_on
- "Waiting for payment on Phase 1" → waiting_on (high priority)
- "Waiting on API provider to fix bug" → waiting_on (external blocker)

## Phase 6: Add Recent Activity

Log what you did:

```yaml
recent_activity:
  - date: 2026-03-19
    update: Built payment integration using Stripe API, all tests passing
  - date: 2026-03-18
    update: Schema design complete, reviewed with client, awaiting approval
  - date: 2026-03-17
    update: Initial database setup, user authentication scaffolding
```

Be specific: "Built payment integration" is better than "worked on Phase 1"

## Phase 7: Detect Scope Creep

Ask: "Did they ask for anything beyond the original contract?"

If yes:
- Document it in the ops file under "Notes"
- Example: "Client requested SMS notifications (out of scope, added to Phase 2 proposal)"
- Decide: Include in current milestone, log as change order, or defer?

```yaml
notes:
  scope_requests:
    - request: SMS notifications feature
      proposed_by: jane@acme.com
      date: 2026-03-18
      status: deferred to Phase 2
      impact: ~15 hours of development
```

## Phase 8: Payment Pipeline Check

If a milestone is "ready-for-review", ask about payment progress:

```yaml
milestones:
  - name: Phase 1
    status: ready-for-review
    closing:
      review_complete: false       # Has client confirmed it works?
      client_approved: false       # Have they signed off?
      invoice_sent: false          # Have you billed them?
      payment_received: false      # Have you got money?
```

Update the closing checklist:

```
Phase 1 Status:
- [ ] Review complete – You tested it
- [ ] Client approved – They've confirmed it works
- [ ] Invoice sent – You've billed them
- [ ] Payment received – Money is in your account
```

When all boxes are checked, milestone is truly done.

## Phase 9: Commit Changes

After updating the ops file:

```bash
cd ~/ops
git add projects/{project-name}.md
git commit -m "[Project] {Quick summary of what changed}"
```

## Phase 10: Communication Log (Optional)

If there was client communication, add it:

```yaml
communication_log:
  - date: 2026-03-19
    type: email
    to: jane@acme.com
    subject: "Phase 1 progress update"
    summary: "Shared demo of payment integration, got approval to proceed"
```

## Question Prompts

Use these to draw out information:

**On progress:**
- "Where's the code at right now?"
- "What's the hardest part left?"
- "What would someone need to test this?"

**On blockers:**
- "What's stuck?"
- "Who has the thing you need?"
- "How long will this probably take to resolve?"

**On scope:**
- "Did they ask for anything new?"
- "Is it worth including, or should it wait?"
- "How much would it cost if we do it?"

**On timeline:**
- "Are you on track for the milestone deadline?"
- "Need to adjust the date?"
- "What's the dependency you need from them?"

## Tone

This is a conversation, not a form. Be casual, direct, curious.

"Tell me what's actually going on with this project."

Listen more than you ask. Let them vent if something is frustrating. Sometimes the blocker is real, sometimes it's avoidance—both are valid information.

## Output

After the conversation, the ops file is updated with:
- Milestone status and deliverable completion
- Waiting items and follow-up schedule
- Recent activity log
- Any scope requests or changes
- Payment pipeline status

The `/standup` skill reads this next morning and adjusts priorities accordingly.

---

**When to use:**
- End of a work session (manual alternative to SessionEnd hook)
- Project status isn't reflecting reality
- You want to log a specific milestone achievement
- Client communication happened and needs recording

**What it updates:**
- `~/ops/projects/{project-name}.md` (project status)

**Next step:** If milestone is complete, maybe run `/follow-up` to draft delivery notification
