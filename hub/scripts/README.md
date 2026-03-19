# Hub Scripts

Background automation scripts. All run via launchd (Mac) or cron (Linux) on an always-on machine.

## Script Descriptions

### overnight-context-builder.sh

**Purpose:** Build context file for morning standup by scanning all repos for recent activity.

**What it does:**
- Walks all repos listed in `~/ops/config/repo-map.json`
- Runs `git log` for commits since last standup
- Extracts activity summaries from commit messages
- Checks `~/ops/projects/*.md` for waiting_on items
- Checks `~/financial/financial-ops.md` for upcoming deadlines
- Generates `~/ops/daily/staging/overnight-context.md` with findings

**Output:** `~/ops/daily/staging/overnight-context.md`
**Frequency:** Daily at 6:00 AM
**Dependencies:** Git, shell utilities

**Example output:**
```markdown
# Overnight Context – 2026-03-19

## Git Activity (since last standup)
- business: 2 commits
- acme-corp project: 5 commits (Salesforce integration complete)

## Milestone Alerts
- Project A Phase 2: Due in 2 days, 95% complete

## Waiting Items Status
- Design assets from Client X (7 days, follow-up due today)

## Financial Calendar
- Quarterly taxes due: April 15 (26 days)
```

### weather-fetch.sh

**Purpose:** Get tomorrow's weather and pollen forecast via free API.

**What it does:**
- Calls Open-Meteo API (free, no key needed) for your location
- Gets tomorrow's forecast: temperature, precipitation, wind
- Gets pollen data if available
- Recommends indoor/outdoor work based on conditions
- Generates `~/ops/daily/staging/weather.md`

**Output:** `~/ops/daily/staging/weather.md`
**Frequency:** Daily at 6:15 AM
**Dependencies:** curl, jq
**Configuration:** Latitude/longitude in `hub-settings.json`

**Example output:**
```markdown
# Weather – 2026-03-19 (tomorrow)

High: 72°F / Low: 58°F
Condition: Partly Cloudy
Wind: 8 mph
Pollen: High (oak, birch)

Recommendation: Outdoor activities possible, plan allergy management
```

### apple-notes-inbox.sh

**Purpose:** Capture phone notes from Apple Notes "Inbox" note throughout the day.

**What it does:**
- Uses AppleScript to read Apple Notes
- Looks for a note called "Inbox"
- Appends contents to `~/personal/inbox.md`
- Clears Apple Notes inbox after capture (optional)
- Timestamps each capture

**Output:** Appends to `~/personal/inbox.md`
**Frequency:** Daily at 6:30 AM (but you add to Apple Notes throughout the day)
**Dependencies:** AppleScript (Mac only), System Events permission
**Setup:** Grant Terminal permission in System Settings

**Example:**
```
# Apple Notes "Inbox" contains:
Call Mom about birthday
Review SaaS pricing comparison
Question on Salesforce API docs

# Gets appended to ~/personal/inbox.md:
[2026-03-19 06:30] Call Mom about birthday
[2026-03-19 06:30] Review SaaS pricing comparison
[2026-03-19 06:30] Question on Salesforce API docs
```

### uncommitted-check.sh

**Purpose:** Flag repositories with uncommitted changes so you don't forget to commit.

**What it does:**
- Walks all repos in `repo-map.json`
- Runs `git status` for each
- Lists any repos with uncommitted changes
- Writes report to log file (not context file, just FYI)
- Helps prevent "oops I forgot to commit" situations

**Output:** `~/helm/hub/logs/uncommitted-check-YYYYMMDD-HHMMSS.log`
**Frequency:** Daily at 7:00 AM
**Dependencies:** Git

**Example output:**
```
[2026-03-19 07:00:15] Uncommitted check started
[2026-03-19 07:00:15] Scanning: /Users/user/helm/business
[2026-03-19 07:00:15]   OK: All committed
[2026-03-19 07:00:15] Scanning: /Users/user/helm/projects/acme-corp
[2026-03-19 07:00:18]   WARNING: 3 uncommitted files
[2026-03-19 07:00:18]     - src/api/payments.ts (modified)
[2026-03-19 07:00:18]     - tests/payments.test.ts (modified)
[2026-03-19 07:00:18] Completed
```

### fallback-wrap.sh

**Purpose:** Safety net. If you forget to run `/wrap` at end of day, this creates a mechanical summary.

**What it does:**
- Runs at 11:45 PM
- Checks if `/wrap` ran today (looks for today's wrap file)
- If not found, creates fallback wrap by scanning git commits since morning
- Extracts rough summary of what happened
- Writes to `~/ops/daily/staging/wrap-fallback-{date}.md`
- Standup next morning sees this and asks: "This is what I think happened yesterday, correct?"

**Output:** `~/ops/daily/staging/wrap-fallback-{date}.md` (only if wrap didn't run)
**Frequency:** Daily at 11:45 PM
**Dependencies:** Git, shell utilities

**When it helps:**
- You worked late and forgot the wrap ritual
- You got distracted and skipped wrap
- System failure prevented wrap from running

**Important:** This is a fallback, not a replacement. Run the actual `/wrap` skill for better context capture.

### notion-sync.sh

**Purpose:** Push your daily hit list to Notion for visibility across devices.

**What it does:**
- Called by `/standup` skill after planning (not automated, triggered by skill)
- Reads `~/ops/daily/staging/{date}-hit-list.md` (created by standup)
- Parses items and priorities
- Pushes to Notion "Business Inbox" database via API
- Creates/updates Notion tasks for each item

**Output:** Notion database updated
**Triggered by:** `/standup` skill completion
**Dependencies:** curl, jq, Notion API token
**Configuration:** workspace_id and database_id in `hub-settings.json`

**Integration:**
- Standup creates hit list
- Hit list file triggers this script
- Your Notion dashboard stays in sync
- Mobile phone can show Notion tasks

### nas-backup.sh

**Purpose:** Nightly backup of critical directories to NAS for disaster recovery.

**What it does:**
- Runs at 2:00 AM daily
- Backs up all repos: `~/helm/business/`, `~/helm/ops/`, etc.
- Uses rsync (efficient, only copies changes)
- Destination: path specified in `hub-settings.json`
- Logs to `~/helm/hub/logs/nas-backup-{date}.log`
- Email notification on failure (optional, requires mailx)

**Output:** `~/helm/hub/logs/nas-backup-YYYYMMDD-HHMMSS.log`
**Frequency:** Daily at 2:00 AM
**Dependencies:** rsync (built-in on Mac/Linux)
**Configuration:** nas_path in `hub-settings.json`

**Typical output:**
```
[2026-03-19 02:00:05] NAS backup started
[2026-03-19 02:00:05] Destination: /Volumes/NAS/helm-backup/
[2026-03-19 02:00:12] Syncing: business/ (2.1 MB)
[2026-03-19 02:00:18] Syncing: ops/ (856 KB)
[2026-03-19 02:00:24] Syncing: projects/acme-corp/ (12.3 MB)
[2026-03-19 02:02:45] Completed: 3.2 GB total
```

### session-end-update.sh

**Purpose:** Auto-update project ops file when you close a project workspace session.

**What it does:**
- Called by SessionEnd hook in project `.claude/settings.json`
- Scans git commits from this session
- Extracts commit messages and file changes
- Updates `~/ops/projects/{project-name}.md` recent_activity
- Checks if any milestones are complete (based on deliverables marked done in commits)
- Verifies YAML syntax of updated ops file

**Output:** Updated `~/ops/projects/{project-name}.md`
**Triggered by:** Project SessionEnd hook
**Dependencies:** Git, shell utilities
**Fallback:** If this hook fails, `/done` skill provides manual alternative

**Integration:**
- You close project session in VS Code
- SessionEnd hook triggers this script
- Script updates ops file automatically
- Standup next morning sees updated status
- No manual tracking needed

---

## Running Scripts Manually

Test any script manually before automating:

```bash
# Make executable
chmod +x ~/helm/hub/scripts/overnight-context-builder.sh

# Run it
~/helm/hub/scripts/overnight-context-builder.sh

# Check output
cat ~/ops/daily/staging/overnight-context.md

# Check logs
tail ~/helm/hub/logs/overnight-context-builder-*.log
```

## Scheduling

### Launchd (Mac)

See `instruction.md` for setup. Each script needs a plist file.

### Cron (Linux)

Add to crontab:

```bash
crontab -e
```

Example:

```cron
0 6 * * * /home/user/helm/hub/scripts/overnight-context-builder.sh
15 6 * * * /home/user/helm/hub/scripts/weather-fetch.sh
```

## Logging

All scripts create timestamped logs in `~/helm/hub/logs/`:

```bash
ls -la ~/helm/hub/logs/
```

Format: `{script-name}-YYYYMMDD-HHMMSS.log`

Check for errors:

```bash
grep ERROR ~/helm/hub/logs/*.log
```

## Dependencies Summary

| Script | Dependencies | Where |
|:-------|:-------------|:------|
| overnight-context-builder | git, shell | Built-in |
| weather-fetch | curl, jq | curl built-in, jq: `brew install jq` |
| apple-notes-inbox | osascript, shell | Built-in (Mac only) |
| uncommitted-check | git, shell | Built-in |
| fallback-wrap | git, shell | Built-in |
| notion-sync | curl, jq | curl built-in, jq: `brew install jq` |
| nas-backup | rsync, shell | Built-in |
| session-end-update | git, shell | Built-in |

Install `jq` if not already present:

```bash
brew install jq
```

---

**Philosophy:** These scripts should run quietly and reliably. Failures should be logged, not noisy. They prepare context for your morning standup, but they're not essential—if one fails, you notice at standup and move on.
