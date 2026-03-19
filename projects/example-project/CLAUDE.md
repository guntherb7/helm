# Project Workspace – Example Project

This is the project-specific workspace for a single client project. It tracks everything about that project: scope, deliverables, timeline, client communication, and integration with the larger ops system.

## Structure

```
projects/[project-name]/
  CLAUDE.md                Project overview & context
  instruction.md           Setup guide for new projects
  README.md                Shared with the client (or reference only)
  project.md               Full project details (same as ~/ops/projects/[name].md reference)
  status-log.md            Weekly status updates and progress
  .claude/
    skills/
      done/
        SKILL.md           Project wrap-up & closure workflow
```

## How It Works

**Daily Standup Integration:**

The `/standup` skill reads the parent `~/ops/projects/example-project.md` file and shows:
- Current phase and completion percentage
- Blockers and waiting_on items
- Recent activity
- Next immediate action

**Weekly Update:**

Update `status-log.md` with:
- What was completed this week
- What's in progress
- Blockers encountered
- Plan for next week

**Project Completion:**

When project is done, run:
```bash
claude skill done
```

This triggers the project-specific `/done` skill, which:
- Marks project as complete in ops system
- Archives deliverables
- Captures lessons learned
- Updates pricing-history with pricing and patterns

## Key Concepts

### Two-Tier Memory

**Working Memory (this folder):**
- Current status and progress
- Weekly updates and notes
- Client communication (if not in email)
- Current blockers and dependencies
- This week/next week tasks

**Reference Memory (~/ops/projects/):**
- Full project definition with metadata
- Contract details, timeline, milestones
- Waiting_on tracking and follow-ups
- Communication log

**Deep Storage (~/business/memory/):**
- Lessons learned (pricing, what worked, what didn't)
- Client patterns (communication style, decision speed)
- Seasonal patterns (revenue, team availability)

### Project Status Tracking

Each project file has status for each milestone:

```yaml
milestones:
  - name: Phase 1 - Design & Setup
    status: in-progress
    deliverables:
      - WordPress setup: complete
      - Custom theme design: 95%
      - Mobile responsive testing: in-progress
      - Admin panel customization: not-started
    closing:
      review_complete: false
      client_approved: false
      invoice_sent: false
      payment_received: false
```

Status progression:
1. `not-started` → `in-progress` → `complete`
2. Once complete → `review_complete` → `client_approved` → `invoice_sent` → `payment_received`

### Waiting On Tracking

For each blocker waiting on external input:

```yaml
waiting_on:
  - what: Design assets from design firm
    who: Acme Corp (they're sourcing)
    since: 2026-03-12
    follow_up_interval_days: 7
    last_follow_up: 2026-03-17
    status: open
    note: Expected by March 19
```

The `/follow-up` skill detects items overdue for follow-up and drafts messages.

## Integration with Core System

**Retropsective (Weekly):**
- Reads project status
- Asks: "What was the biggest blocker this week?"
- Updates `~/business/memory/patterns.md` with project learnings

**Standup (Daily):**
- Shows project status and next actions
- Suggests tasks based on phase and blockers
- Highlights waiting_on items needing follow-up

**Follow-up Skill (Automatic):**
- Detects waiting_on items overdue for follow-up
- Drafts reminder emails to clients
- Updates last_follow_up timestamp

**Done Skill (Project End):**
- Marks project complete
- Archives deliverables
- Records lessons learned
- Updates pricing-history.md

## Project Lifecycle

### 1. Kickoff (New Project)

```bash
claude skill new-project
```

Creates:
- Project folder structure
- Project metadata file
- Initial CLAUDE.md
- GitHub repo (if needed)
- Slack channel (if applicable)

### 2. Active Execution

- Daily standup reads project status
- Weekly updates to status-log.md
- Track blockers and waiting_on items
- Manage client communication
- Monitor timeline against deadlines

### 3. Milestone Completion

For each milestone:
1. Mark deliverables complete
2. Client review and approval
3. Send invoice
4. Record payment
5. Advance to next phase

### 4. Project Closure

When final milestone is done:

```bash
claude skill done
```

This:
- Captures final status
- Records lessons learned
- Updates pricing history
- Moves project to archive
- Sends closure email to client

## Related Files

- `~/ops/projects/example-project.md` – Full project definition (read by standup)
- `~/business/memory/clients.md` – Client reference (communication style, history)
- `~/business/memory/pricing-history.md` – What pricing worked, lessons learned
- `~/projects/example-project/status-log.md` – Weekly progress updates

## Philosophy

Project workspaces are temporary. They exist for the life of the project, then move to deep storage (lessons learned, pricing, client patterns).

The project folder is your daily workspace: current tasks, blockers, status. The ops file is what the broader system reads for context and scheduling.

When the project ends, the learning moves to memory; the workspace can be archived.
