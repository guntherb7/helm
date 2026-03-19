# HELM Setup Guide

This guide walks you through setting up HELM as your personal operations system.

## Prerequisites

- VS Code with Claude Code extension installed
- Git on your machine
- Basic comfort with markdown and YAML
- A private location to clone each repo (or create private repos on GitHub)

## Phase 1: Structure (15 min)

HELM is organized as six interconnected repos in a parent folder:

```
helm/
  business/       Daily command center (daily planning, proposals, contracts)
  ops/            Shared state (projects, config, templates, memory)
  personal/       Life operations (goals, values, health, personal tasks)
  financial/      Business finances (receipts, invoices, checklists)
  hub/            Background automation (cron jobs, context builders)
  projects/       Client repos (one per active engagement)
```

### Option A: Single Private Repo (Recommended)

Clone the public repo, then make your own copy private:

```bash
git clone https://github.com/guntherb7/helm.git ~/helm
cd ~/helm
git remote set-url origin https://github.com/YOUR_USERNAME/helm-private.git
git push origin main
```

### Option B: Separate Private Repos

Clone the public repo, then split into separate private repos:

```bash
# Clone public
git clone https://github.com/guntherb7/helm.git ~/helm

# For each directory, create a private repo and set origin
cd ~/helm/business
git remote set-url origin https://github.com/YOUR_USERNAME/helm-business-private.git
git push origin main
```

Update paths in `hub/config/repo-map.json` to point to your repo locations.

## Phase 2: Configuration (20 min)

### 1. Fill in hub/config/hub-settings.json

```json
{
  "mac_user": "YOUR_USERNAME",
  "home_dir": "/Users/YOUR_USERNAME",
  "nas_path": "/Volumes/NAS/backup",
  "schedules": {
    "standup_time": "09:00",
    "wrap_time": "18:00",
    "weekly_planning_day": "monday",
    "weekly_planning_time": "09:30"
  },
  "notion": {
    "workspace_id": "YOUR_NOTION_WORKSPACE_ID",
    "database_id": "YOUR_NOTION_DB_ID"
  },
  "financial": {
    "tax_quarters": ["04-15", "06-15", "09-15", "01-15"],
    "payroll_day": "1st"
  },
  "email": {
    "from": "you@example.com",
    "signature": "Your Name"
  }
}
```

See the Notion integration section below for how to get workspace/database IDs.

### 2. Create repo-map.json

Update `ops/config/repo-map.json` with your actual repo paths:

```json
{
  "repos": [
    {
      "name": "business",
      "path": "/Users/YOUR_USERNAME/helm/business"
    },
    {
      "name": "ops",
      "path": "/Users/YOUR_USERNAME/helm/ops"
    },
    {
      "name": "personal",
      "path": "/Users/YOUR_USERNAME/helm/personal"
    },
    {
      "name": "financial",
      "path": "/Users/YOUR_USERNAME/helm/financial"
    },
    {
      "name": "hub",
      "path": "/Users/YOUR_USERNAME/helm/hub"
    }
  ],
  "projects": {
    "example-project": "/Users/YOUR_USERNAME/helm/projects/example-project"
  }
}
```

### 3. Configure Notion Integration (Optional)

Notion integration syncs your daily hit list to a Notion database automatically after standup.

1. Go to Notion → Settings → Integrations
2. Create a new integration called "Claude Code HELM"
3. Grant read/write permissions
4. Create a database in Notion called "Business Inbox" with columns:
   - Title (text)
   - Priority (select: critical, high, medium, low)
   - Status (select: not started, in progress, done)
   - Project (text)
   - Due Date (date)
5. Copy the database ID from the database URL
6. Update `hub-settings.json` with workspace_id and database_id

## Phase 3: Personal Data (30 min)

### 4. Write Your Personal Plan

Create `personal/plan.md` with your actual values and goals:

```markdown
# My Operations Plan

## This Year's Focus
[What matters most to you this year?]

## Values
[3-5 core values that guide decisions]

## Health Goals
[What does healthy look like?]

## Personal Goals
[What do you want to accomplish?]

## What Success Looks Like
[Define it for yourself]
```

The standup reads this every morning. Be real here.

### 5. Fill in Personal Operations

Create `personal/personal-ops.md` with your actual ongoing tasks. Examples:

```yaml
## Active
- task: Weekly 1:1s with team
  recurring: weekly
  day: Tuesday 10am
  goal: Maintain communication and feedback loop

- task: Blog post on current topic
  effort: medium
  window: Before end of month
  goal: Build thought leadership

## Health
- task: Physical therapy exercises
  recurring: daily
  duration: 15min
  goal: Prevent shoulder issues

- task: Weekly long run
  recurring: weekly
  day: Saturday
  duration: 90min
  goal: Maintain running fitness
```

### 6. Add Your Clients

Update `business/memory/clients.md` with your actual client list:

```yaml
## Client: Acme Corp
- contact: jane@acme.com (Jane Smith, Product Lead)
- secondary: john@acme.com (John Chen, Decision Maker)
- communication_preference: Async updates, monthly calls
- response_time: 2-3 days typical
- decision_maker: John
- timezone: EST
- notes: |
  Likes short status emails. Prefers no calls unless urgent.
  Quarterly reviews required. Budget approval required by CFO.
  Payment 30 days net from invoice.
```

### 7. Extract Your Writing Voice

The `/new-proposal` skill generates documents in your voice. Feed it training data:

Update `ops/templates/voice-guide.md`:

```markdown
# Your Writing Voice

## Style
[Your typical style: concise, verbose, formal, casual, technical, plain English?]

## Tone
[Professional but warm? Direct and blunt? Collaborative?]

## Patterns
[Things you always include: executive summary? Case studies? Budget breakdown?]

## Examples
[Paste 2-3 sentences from actual proposals you've sent]

## Never Say
[Things that are not you: "leverage", "synergize", etc.]
```

## Phase 4: Testing (20 min)

### 8. Test the Daily Workflow

**Morning standup:**

```bash
cd ~/helm/business
claude skill standup
```

This should:
- Greet you and ask how you're doing
- Ask energy/mood/focus check-in questions
- Load your weekly plan and project status
- Show landscape of what's urgent
- Suggest 2-4 items with micro-steps
- Create `ops/daily/staging/YYYY-MM-DD.md` with your plan
- Stay open for your morning work

**End of day wrap:**

```bash
cd ~/helm/business
claude skill wrap
```

This should:
- Ask what actually happened today
- Capture what you finished and what carries forward
- Create `ops/daily/staging/YYYY-MM-DD-wrap.md`
- Stage tomorrow's context

### 9. Test Weekly Cycle

**Monday planning:**

```bash
cd ~/helm/business
claude skill weekly-plan
```

This should:
- Load all project statuses
- Ask what your 3-5 weekly objectives are
- Create `ops/daily/weekly/YYYY-WNN-plan.md`

**Friday retrospective:**

```bash
cd ~/helm/business
claude skill retrospective
```

This should:
- Review the week through four lenses (revenue, energy, avoidance, growth)
- Update `business/memory/patterns.md` with new observations
- Create `ops/daily/weekly/YYYY-WNN-retro.md`

### 10. Test Project Workflow

Create an example project:

```bash
cd ~/helm/business
claude skill new-project
```

Follow the prompts to create a test project. This should:
- Create `ops/projects/example-project.md`
- Create `projects/example-project/` repo
- Set up hooks and GitHub integration

Then test the `/done` skill on that project.

## Phase 5: Automation (30 min, defer one week)

Wait at least a week after starting manual use before setting up cron jobs. The system works better when you understand the manual workflows first.

### 11. Set Up Overnight Context Builder

This cron job runs at 6am and prepares context for standup:

```bash
cd ~/helm/hub/scripts
chmod +x overnight-context-builder.sh
```

Add to launchd (Mac):

```bash
cp com.helm.overnight-context.plist ~/Library/LaunchAgents/
launchctl load ~/Library/LaunchAgents/com.helm.overnight-context.plist
```

### 12. Set Up Fallback Wrap

This cron job runs at 11:45pm as a safety net if you forgot to wrap:

```bash
chmod +x fallback-wrap.sh
cp com.helm.fallback-wrap.plist ~/Library/LaunchAgents/
launchctl load ~/Library/LaunchAgents/com.helm.fallback-wrap.plist
```

### 13. Set Up Notion Sync

Add to your Notion workspace if you want hit list syncing:

In `/standup`, verify:
- `hub-settings.json` has workspace_id and database_id
- Notion integration is connected

The sync happens automatically after standup completes.

## Phase 6: Daily Rhythm (ongoing)

**Daily:**
- Morning: `/standup` (10-15 min)
- Throughout day: Open your project repos, Claude Code syncs context from CLAUDE.md and daily instructions
- Evening: `/wrap` (5 min)

**Weekly:**
- Monday: `/weekly-plan` (15 min)
- Friday: `/retrospective` (20 min)

**As needed:**
- `/new-proposal` – When you have a new potential client
- `/new-contract` – When proposal is accepted
- `/follow-up` – When you need to check on waiting items
- `/new-project` – When you start a new engagement
- `/done` – To manually update project status
- `/update` – Quick one-liner status append

## Troubleshooting

### Skills not showing up?

Check that all SKILL.md files exist:

```bash
ls -la ~/helm/business/.claude/skills/*/SKILL.md
```

Should show 10 files (standup, wrap, weekly-plan, retrospective, new-proposal, new-contract, follow-up, new-project, done, update).

### Standup not loading context?

Create the required directories:

```bash
mkdir -p ~/helm/ops/daily/staging
mkdir -p ~/helm/ops/daily/instructions
mkdir -p ~/helm/ops/daily/weekly
mkdir -p ~/helm/ops/projects
mkdir -p ~/helm/personal/health
```

### Notion sync not working?

- Verify Notion integration is installed in your workspace
- Check `hub-settings.json` has correct workspace_id and database_id
- Look for error messages at the end of standup output
- Test with a simple message first

### Git hooks failing?

SessionEnd hooks are optional. If they're causing issues:
- Remove `~/helm/.claude/hooks/SessionEnd.md`
- Use `/done` skill manually instead
- Or re-add hooks after you're comfortable with the system

## Next Steps

1. **Day 1:** Clone, config, write your plan, add clients
2. **Day 2-5:** Use daily standup/wrap manually, get comfortable
3. **Week 2:** Add weekly-plan and retrospective
4. **Week 3:** Use /new-proposal and /new-contract skills
5. **Week 4:** Set up automation jobs

The system is most powerful when you build it gradually. Start with standup and wrap. Everything else follows naturally.

---

**Questions?** See [README.md](README.md) for architecture overview or refer to individual directory CLAUDE.md files for scoped setup.
