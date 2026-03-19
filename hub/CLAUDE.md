# Hub – Background Automation

This workspace manages automated overnight jobs that prepare context for your morning standup. Scripts run on an always-on machine (Mac Studio, Raspberry Pi, or similar) via launchd/cron.

## Architecture

All scripts:
- Read paths from `~/ops/config/repo-map.json` (never hardcode paths)
- Write context to `~/ops/daily/` (shared, read by standup)
- Log to `~/hub/logs/` with timestamp
- Use JSON or markdown for output (human + machine readable)
- Return exit code 0 on success, non-zero on failure

## Scripts

Run manually or via launchd. All are in `~/hub/scripts/`:

| Script | Purpose | Runs | Outputs |
|:-------|:--------|:-----|:--------|
| `overnight-context-builder.sh` | Git diffs across all repos since last standup | 6am | `~/ops/daily/staging/overnight-context.md` |
| `weather-fetch.sh` | Tomorrow's forecast + pollen via Open-Meteo API | 6:15am | `~/ops/daily/staging/weather.md` |
| `apple-notes-inbox.sh` | Read Apple Notes "Inbox" note, append to inbox.md | 6:30am | `~/personal/inbox.md` |
| `uncommitted-check.sh` | Flag repos with uncommitted changes | 7am | `~/hub/logs/uncommitted-{date}.log` |
| `fallback-wrap.sh` | If wrap didn't run by 11:45pm, do mechanical scan | 11:45pm | `~/ops/daily/staging/wrap-fallback-{date}.md` |
| `notion-sync.sh` | Called by /standup, push hit list to Notion DB | on-demand | Notion database |
| `nas-backup.sh` | rsync critical directories to NAS | 2am | `~/hub/logs/backup-{date}.log` |
| `session-end-update.sh` | Called by project SessionEnd hooks | on-demand | `~/ops/projects/` updates |

## File Paths

Never hardcode paths in scripts. Always read from `repo-map.json`:

```bash
# Read repo paths from config
repo_map="$HOME/helm/ops/config/repo-map.json"
business_repo=$(cat "$repo_map" | jq -r '.repos[] | select(.name == "business") | .path')
projects_dir=$(cat "$repo_map" | jq -r '.repos[] | select(.name == "ops") | .path')/projects
```

This keeps scripts portable if you move things.

## Logging

All scripts write logs to `~/hub/logs/`:

```bash
log_file="$HOME/helm/hub/logs/$(basename "$0" .sh)-$(date +%Y%m%d-%H%M%S).log"
echo "[$(date)] Starting overnight context build" >> "$log_file"
# ... do work ...
echo "[$(date)] Success" >> "$log_file"
```

Each run creates a new timestamped log. Logs are human-readable and machine-parseable (timestamp prefix).

## Launchd Setup (Mac)

Create plist files in `~/Library/LaunchAgents/`:

**Example: Overnight context builder at 6am daily**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.helm.overnight-context</string>
    <key>ProgramArguments</key>
    <array>
        <string>/Users/YOUR_USERNAME/helm/hub/scripts/overnight-context-builder.sh</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>6</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>StandardOutPath</key>
    <string>/Users/YOUR_USERNAME/helm/hub/logs/overnight-context-stdout.log</string>
    <key>StandardErrorPath</key>
    <string>/Users/YOUR_USERNAME/helm/hub/logs/overnight-context-stderr.log</string>
</dict>
</plist>
```

Load it:

```bash
launchctl load ~/Library/LaunchAgents/com.helm.overnight-context.plist
```

## Output Format

Scripts write to shared `~/ops/daily/` so the standup can read them:

**Overnight context (`~/ops/daily/staging/overnight-context.md`):**
```markdown
# Overnight Context – 2026-03-19

## Git Activity
- business: 3 commits, last one 2 hours ago ("Fix payment button styling")
- projects/acme-corp: 5 commits ("Salesforce integration complete")
- projects/example-project: no changes since last standup

## Milestone Alerts
- Acme Corp Phase 2: Due in 2 days, 90% complete
- Example Project: Phase 1 ready for client review

## Waited Items Status
- Design assets from Acme (7 days, follow-up due today)
- Client feedback on Example Project (3 days, on track)

## Financial Calendar
- Quarterly taxes due: April 15 (26 days away)
- Invoices to send: Phase 1 completion (Acme Corp, pending review)
```

**Weather (`~/ops/daily/staging/weather.md`):**
```markdown
# Weather – 2026-03-19

## Tomorrow's Forecast
- High: 72°F, Low: 58°F
- Condition: Partly cloudy
- Wind: 8 mph
- Pollen: High (oak, birch)
- Recommendation: Outdoor activities possible, allergy management needed
```

## Error Handling

If a script fails:
1. Exit with non-zero code
2. Write error to log with timestamps
3. Don't create output file (standup won't read partial/broken context)
4. Next morning, standup will mention error in its context loading

Example:

```bash
if ! git diff --name-only HEAD~1..HEAD > "$temp_file"; then
    echo "[$(date)] ERROR: Git diff failed" >> "$log_file"
    exit 1
fi
```

## Testing

Run scripts manually before loading into launchd:

```bash
# Test script
~/helm/hub/scripts/overnight-context-builder.sh

# Check output
cat ~/ops/daily/staging/overnight-context.md

# Check log
tail ~/helm/hub/logs/overnight-context-builder*.log
```

All should work without errors.

## Cron Alternative (Linux)

If you're on Linux or prefer cron, add to crontab:

```cron
# Run overnight context builder at 6am
0 6 * * * /home/user/helm/hub/scripts/overnight-context-builder.sh

# Weather fetch at 6:15am
15 6 * * * /home/user/helm/hub/scripts/weather-fetch.sh

# Fallback wrap at 11:45pm
45 23 * * * /home/user/helm/hub/scripts/fallback-wrap.sh
```

## Important Notes

- **Never hardcode usernames or paths.** Use `$HOME`, read from config.
- **Always use timestamps in logs.** Helps with debugging.
- **Return proper exit codes.** 0 for success, non-zero for failure.
- **Write human-readable output.** Markdown is good. JSON is fine for data.
- **Assume scripts can fail gracefully.** Standup will notice missing context and tell you.
- **Test manually before automating.** Cron and launchd can hide issues.

---

**Timezone:** All scripts run in your system timezone (read from `~/.config/helm/timezone` if you want to override)
**Logs:** `~/helm/hub/logs/` (organized by script, timestamped)
**Context output:** `~/ops/daily/staging/` (read by /standup)
**Config:** `~/ops/config/hub-settings.json` (user/paths/schedules)

**Philosophy:** Automation should be boring. It runs, writes context, and gets out of the way. The standup reads what it wrote and adjusts accordingly.
