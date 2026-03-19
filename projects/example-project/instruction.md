# Project Setup

Setting up a new project workspace. Do this once per project, right after contract is signed.

## Quick Start (You have a signed contract)

```bash
cd ~/projects
claude skill new-project
# Skill will prompt for:
# - Project name (e.g., "acme-website-rebuild")
# - Client name
# - Contract value
# - Timeline (start/end dates)
# - Phase structure (if milestone-based)
```

This creates everything automatically. You can skip to "Verify Setup" below.

---

## Manual Setup (If you're setting up an existing project)

### 1. Create Project Folder

```bash
mkdir -p ~/projects/[project-name]
cd ~/projects/[project-name]
```

Use kebab-case for project name: `acme-website-rebuild`, `client-app-mvp`, etc.

### 2. Create Core Files

Create these files in the project folder:

**CLAUDE.md** – Project context (what this workspace is for)

**instruction.md** – This file (setup guide)

**README.md** – Client-facing summary (or internal reference)

**status-log.md** – Weekly progress updates

**.claude/skills/done/SKILL.md** – Project closure workflow

### 3. Link to Ops System

Create the master project file in `~/ops/projects/`:

```bash
touch ~/ops/projects/[project-name].md
```

Copy the YAML frontmatter from example-project.md, update:
- `name`, `client`, `type`
- `status`, `start_date`
- Contract values, contacts, payment terms
- Milestones with due dates and deliverables

This is the file your standup reads daily.

### 4. Set Up Git (If Applicable)

For code projects:

```bash
cd ~/projects/[project-name]
git init
git remote add origin [github-repo-url]
git branch -M main
git config user.email "[your-email]"
git config user.name "[your-name]"
```

Create `.gitignore` for sensitive files:

```
.env
.env.local
secrets.json
node_modules/
.DS_Store
.claude/
```

### 5. Create .claude/ Folder

```bash
mkdir -p .claude/skills/done
```

This holds project-specific skills. At minimum, create the `done` skill.

---

## Verify Setup

After creating the project:

1. Check that `~/ops/projects/[project-name].md` exists
2. Check that `~/projects/[project-name]/CLAUDE.md` exists
3. Run standup:
   ```bash
   cd ~/business
   claude skill standup
   ```
   It should mention your project in the daily briefing.

4. If it doesn't appear, check:
   - Project name matches in ops file and project folder
   - Status in ops file is `active` (not `planning` or `completed`)

---

## Structure for Milestone Projects

If your project has multiple phases/milestones:

```yaml
milestones:
  - name: Phase 1 - Design & Setup
    value: 5000
    due: 2026-03-20
    status: not-started
    deliverables:
      - WordPress setup: not-started
      - Custom theme design: not-started
      - Mobile responsive testing: not-started
    closing:
      review_complete: false
      client_approved: false
      invoice_sent: false
      payment_received: false

  - name: Phase 2 - Integration & Features
    value: 5000
    due: 2026-04-20
    status: not-started
    deliverables:
      - Salesforce CRM integration: not-started
      - Contact form with automation: not-started
    closing:
      review_complete: false
      client_approved: false
      invoice_sent: false
      payment_received: false
```

For each milestone, track deliverable status: `not-started`, `in-progress`, `complete`.

Once deliverable is complete, advance milestone through closing pipeline:
1. `review_complete: true` (you review the work)
2. `client_approved: true` (client reviews and approves)
3. `invoice_sent: true` (invoice delivered)
4. `payment_received: true` (payment clears bank)

## Status Log Template

Create `status-log.md` with weekly updates:

```markdown
# Status Log

## Week of March 17, 2026

**Completed:**
- Custom WordPress theme 95% done
- Mobile responsive testing in progress
- Design assets received from client's design firm

**In Progress:**
- Admin panel customization (awaiting final design assets)
- Client review of staging site

**Blockers:**
- Final design asset batch from Acme's design firm (due March 19)

**Next Week:**
- Complete admin panel customization
- Prepare Phase 1 closure review
- Schedule client review call

**Notes:**
- Client very engaged; testing early and often
- No scope creep so far; staying on timeline

---

## Week of March 10, 2026

[Previous week status...]
```

**Update this every Friday** with:
- What got done
- What's in progress
- Any blockers or issues
- Plan for next week
- General notes

## Done Skill Setup

Create `.claude/skills/done/SKILL.md`:

```yaml
---
name: project-done
description: Wrap up and close Acme Corp website rebuild project
model: claude-opus-4-5
allowed-tools:
  - FileWrite
  - FileRead
  - Bash
---

# Project Closure – Acme Corp Website Rebuild

Final project documentation and learning capture.

## Step 1: Verify Project Completion

Checklist before marking done:

- [ ] All deliverables completed
- [ ] Client approval received
- [ ] Final payment received
- [ ] All project files delivered to client
- [ ] Hosting/domain transferred (if applicable)

## Step 2: Update Project Status

Mark in `~/ops/projects/[project-name].md`:

```yaml
status: completed
actual_end_date: 2026-05-10
```

## Step 3: Archive and Document

Capture in `status-log.md`:

```markdown
## Project Complete

**Final Status:** Delivered and closed
**Actual completion:** [Date]
**Final cost vs estimate:** [On budget/under/over]

**Lessons learned:**
- [What went well]
- [What would we do differently]
- [Client feedback]
- [Pricing lesson]
```

## Step 4: Update Pricing History

Update `~/business/memory/pricing-history.md` with:
- Final project value: $15,000
- Actual hours vs estimated
- What pricing worked
- Client payment behavior
- Would we take this project again?

## Step 5: Archive Project

```bash
# Move project to archive
mv ~/projects/[project-name] ~/projects/archive/[project-name]

# Commit to git if applicable
git commit -am "Project complete: [project-name]"
```

## Step 6: Close Communication

Send final closure email to client:

> Hi [Client],
>
> The website rebuild is officially complete and delivered. Here's what's next:
>
> - All project files and access credentials are yours
> - Hosting is set up and ready
> - [Any handoff items]
>
> It was great working with you. If you need anything in the future, let me know.
>
> [Your name]

---

**Everything is documented. You can move on to the next project.**
```

---

## Common Project Flows

### Fixed-Price Project (3 Milestones)

1. Kickoff → Phase 1 work → Phase 1 invoice → Phase 2 work → Phase 2 invoice → Phase 3 work → Phase 3 invoice → Project done

Track each milestone closing pipeline (review → approval → invoice → payment).

### Retainer Project (Ongoing)

1. Kickoff → Monthly work → Monthly invoice → Next month → [Repeat]

When retainer ends, run done skill to archive and capture lessons.

### Discovery / Proposal Stage

Before contract signed:
- Create project in `~/ops/projects/` with status: `proposal`
- Once signed, change status to `active`
- Create project folder in `~/projects/`

### Scope Change Mid-Project

If client requests significant change:
1. Pause current phase
2. Create change order with new scope and pricing
3. Get written approval
4. Update ops file with new deliverables
5. Resume work

Never continue work beyond original scope without documenting it.

---

## Troubleshooting

**Standup doesn't mention my project:**
- Check `~/ops/projects/[name].md` exists
- Check project status is `active` (not `draft`, `proposal`, `completed`)
- Check project start_date is today or earlier

**Can't remember what I did last week:**
- Check `status-log.md` from last Friday
- Check `~/ops/projects/[name].md` communication_log section

**Client is asking for something not in scope:**
- Reference the original contract scope (in ops file)
- Decide: Is it a minor addition (absorb it) or scope change (change order)?
- Document the decision

**Project is overdue:**
- Update milestone due date in ops file
- Note the reason in recent_activity
- Add blocker to waiting_on if external
- Communicate timeline change to client

---

## Philosophy

Project workspaces are temporary. They exist to deliver the work and capture learnings. When the project ends, the learning goes to deep memory (what worked, what didn't) and the workspace gets archived.

Keep this workspace focused on execution: current status, blockers, next actions. The broader ops system handles scheduling, prioritization, and integration with your other work.
