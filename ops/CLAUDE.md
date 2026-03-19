# Ops – Shared State

This is the central shared state for the entire system. All other workspaces read from here. Nothing lives in ops except shared config, project files, templates, and daily context.

## What Lives Here

```
ops/
  config/              Configuration
  templates/           Reusable contract/proposal templates
  projects/            Project ops files (business state)
  daily/               Daily context (staging, instructions, weekly)
  memory/              (in business/, not here—long-term patterns)
```

## Conventions

All files follow consistent conventions so different workspaces can integrate smoothly.

### Dates

**Format:** YYYY-MM-DD (ISO 8601)
- File names: `2026-03-19.md` (never `03-19-2026` or `March 19`)
- YAML frontmatter: `date: 2026-03-19` (consistent format)
- References: "March 19" or "2026-03-19" (context-dependent)

Why: Sortable alphabetically, unambiguous across locales, version-control friendly.

### Project Names

**Format:** kebab-case (lowercase, hyphens)
- `acme-corp-website` (not `acmeCorp`, `Acme Corp`, or `AcmeCorpWebsite`)
- `example-project` (not `Example Project`)
- Files: `ops/projects/acme-corp-website.md` (not `.txt`, not in a folder)

Why: Clean in URLs, filenames, git diffs, and cross-references.

### Status Values

Use these consistently across project ops files:

```yaml
status: proposal        # Proposal stage, not yet signed
status: active          # Contract signed, work underway
status: paused          # Temporarily stopped
status: complete        # All deliverables done, money received
status: archived        # Old project, reference only
```

For milestones:

```yaml
status: not-started
status: in-progress
status: ready-for-review
status: client-review
status: complete
status: blocked
```

For deliverables within milestones:

```yaml
status: not-started
status: in-progress
status: 50%             # Or any percentage for partial work
status: ready-for-review
status: complete
status: blocked
```

Why: Standup and planning skills filter by status. Consistency enables automation.

### Payment Status

In milestone closing pipeline:

```yaml
closing:
  review_complete: false        # You tested it
  client_approved: false        # Client signed off
  invoice_sent: false           # Billed them
  payment_received: false       # Money in account
```

Check all four boxes before considering a milestone fully closed.

Why: Freelancers lose revenue between "done" and "paid." Explicit tracking prevents this.

### Waiting On Items

Format in project files:

```yaml
waiting_on:
  - what: Design assets
    who: Client Name
    since: 2026-03-10
    follow_up_interval_days: 7
    last_follow_up: 2026-03-17
    status: open                 # or follow-up-sent, escalated
```

Why: `/follow-up` skill parses this to detect overdue items and draft reminders.

### Communication Log

Every client interaction:

```yaml
communication_log:
  - date: 2026-03-19
    type: email              # or call, in-person, slack, etc.
    to: jane@acme.com        # who you communicated with
    subject: "Phase 1 status update"
    summary: "Reported 90% completion, answered questions on timeline"
```

Why: Historical record. Patterns emerge over time (response speed, decision timings).

## Data Structure Reference

### Project File (~/ops/projects/{name}.md)

```yaml
---
name: acme-corp-website
client: Acme Corp
type: web-development
status: active
contract:
  value: 15000
  signed_date: 2026-02-15
  start_date: 2026-02-20
  contact: jane@acme.com
  decision_maker: john@acme.com

milestones:
  - name: Phase 1 - Design & Setup
    value: 5000
    due: 2026-03-20
    status: in-progress
    deliverables:
      - WordPress setup: complete
      - Custom theme: 90%
      - Mobile responsive: in-progress
    closing:
      review_complete: false
      client_approved: false
      invoice_sent: false
      payment_received: false

waiting_on:
  - what: Design assets from design firm
    who: Acme Corp (they're sourcing)
    since: 2026-03-12
    follow_up_interval_days: 7
    last_follow_up: 2026-03-17
    status: open

communication_log:
  - date: 2026-03-17
    type: email
    to: jane@acme.com
    subject: "Phase 1 progress check-in"
    summary: "Reported 85% completion, shared demo link"

---

# Acme Corp – Website Rebuild

## Current Focus

Finishing Phase 1 design integration. Waiting on final asset batch from their design firm.
If assets arrive as expected, Phase 1 ready for review by March 20.

## Blockers

None. Design assets are on track from client.

## Recent Activity

- [2026-03-19] Integrated latest design comps, 90% complete on custom theme
- [2026-03-18] WordPress schema complete, admin panel functional
- [2026-03-17] Sent client progress email with demo link

## Notes

- Acme prefers weekly async updates (email)
- Their legal requires contract revision before Phase 2 (4-week process)
- Client paid on-time for retainer work previously—low risk

## Change Requests

None yet.
```

### Config Files

**hub-settings.json:** User settings (username, paths, API keys)
**repo-map.json:** Paths to all repos (read by scripts)

### Templates

See individual template files for structure. All use {PLACEHOLDERS} for customization.

## Writing to Ops

Files in ops are written by:

- **Project files:** `/done` skill, `/new-project` skill, SessionEnd hooks
- **Daily context:** `/standup`, `/wrap`, overnight scripts
- **Memory files:** `/retrospective` skill

Files are read by:
- **Project files:** `/standup`, `/weekly-plan`, `/follow-up`, `/done`, project CLAUDEs
- **Daily context:** `/standup`, `/wrap`
- **Memory files:** `/standup` (patterns), scripts (context)

## Git Discipline

Ops is in git (either as part of helm repo or separate private repo).

```bash
cd ~/ops
git add projects/
git commit -m "Update: [project] Phase 1 status"
```

Every update is logged in history. Standup can see what changed since last run.

## Access Pattern

The business workspace reads from ops:

```python
# Pseudo-code for how skills integrate
@-import ~/ops/config/hub-settings.json
@-import ~/ops/projects/*.md
@-import ~/ops/daily/staging/*
@-import ~/ops/templates/*
@-import ~/business/memory/patterns.md
```

Project workspaces also read:

```python
@-import ~/ops/projects/{this-project}.md
@-import ~/ops/projects/{this-project}/daily/{today}.md
```

This keeps context unified without duplication.

## Important Notes

- **No secrets in ops:** No passwords, API keys, or credentials. Use .env files locally.
- **No personal info in ops:** Client names are fine; personal addresses, phone numbers, etc., go in business/memory/clients.md (private).
- **Markdown is readable:** All files should be readable as plain text. Avoid binary formats.
- **YAML is parseable:** Frontmlatter must be valid YAML (indentation matters).
- **Dates are consistent:** All ISO 8601 format, always sortable.
- **Git history is truth:** Commit messages and diffs tell the full story.

---

**Philosophy:** Ops is boring infrastructure. It should be reliable, consistent, and transparent. Everything is in git. Nothing is hidden. Scripts can automate against known conventions.
