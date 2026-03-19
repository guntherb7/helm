# Hub Setup – Background Automation

Follow these steps to set up overnight automation. Start with manual jobs running well first (at least one week), then add automation.

## Phase 1: Configuration (15 min)

### 1. Update hub-settings.json

Edit `~/ops/config/hub-settings.json`:

```json
{
  "mac_user": "YOUR_USERNAME",
  "home_dir": "/Users/YOUR_USERNAME",
  "nas_path": "/Volumes/NAS/backup",
  "schedules": {
    "standup_time": "09:00",
    "wrap_time": "18:00",
    "context_builder_time": "06:00",
    "weather_fetch_time": "06:15",
    "inbox_capture_time": "06:30",
    "uncommitted_check_time": "07:00",
    "fallback_wrap_time": "23:45",
    "nas_backup_time": "02:00"
  },
  "notion": {
    "workspace_id": "YOUR_NOTION_WORKSPACE_ID",
    "database_id": "YOUR_NOTION_DB_ID"
  },
  "weather": {
    "latitude": 37.7749,
    "longitude": -122.4194,
    "timezone": "America/Los_Angeles"
  },
  "financial": {
    "tax_quarters": ["04-15", "06-15", "09-15", "01-15"],
    "payroll_day": "1st"
  }
}
```

### 2. Update repo-map.json

Edit `~/ops/config/repo-map.json` with your actual paths:

```json
{
  "repos": [
    {"name": "business", "path": "/Users/YOUR_USERNAME/helm/business"},
    {"name": "ops", "path": "/Users/YOUR_USERNAME/helm/ops"},
    {"name": "personal", "path": "/Users/YOUR_USERNAME/helm/personal"},
    {"name": "financial", "path": "/Users/YOUR_USERNAME/helm/financial"},
    {"name": "hub", "path": "/Users/YOUR_USERNAME/helm/hub"}
  ],
  "projects": {
    "example-project": "/Users/YOUR_USERNAME/helm/projects/example-project"
  }
}
```

### 3. Create Log Directory

```bash
mkdir -p ~/helm/hub/logs
chmod 755 ~/helm/hub/logs
```

## Phase 2: Build Scripts (30 min)

All scripts are templates in `~/helm/hub/scripts/`. You need to build/enable them.

### Script 1: overnight-context-builder.sh

This script:
- Walks all repos in repo-map.json
- Runs `git log` for commits since last standup
- Checks financial calendar for upcoming deadlines
- Lists waited items needing follow-up
- Creates `~/ops/daily/staging/overnight-context.md`

Template exists at `~/helm/hub/scripts/overnight-context-builder.sh`. Verify it exists:

```bash
ls -la ~/helm/hub/scripts/overnight-context-builder.sh
```

Make it executable:

```bash
chmod +x ~/helm/hub/scripts/overnight-context-builder.sh
```

### Script 2: weather-fetch.sh

Uses free Open-Meteo API to get tomorrow's forecast + pollen.

```bash
chmod +x ~/helm/hub/scripts/weather-fetch.sh
```

Requires `jq` (JSON parser):

```bash
which jq  # Check if installed
# If not: brew install jq
```

### Script 3: apple-notes-inbox.sh

Uses `osascript` to read Apple Notes "Inbox" note and append to `~/personal/inbox.md`.

```bash
chmod +x ~/helm/hub/scripts/apple-notes-inbox.sh
```

Requires AppleScript permissions. You may need to:
1. Open System Settings → Privacy & Security → Automation
2. Find Terminal (or your runner)
3. Grant permission to control System Events

### Script 4: uncommitted-check.sh

Walks all repos and checks for uncommitted changes.

```bash
chmod +x ~/helm/hub/scripts/uncommitted-check.sh
```

### Script 5: fallback-wrap.sh

At 11:45pm, checks if `/wrap` ran today. If not, creates a mechanical summary from git diffs.

```bash
chmod +x ~/helm/hub/scripts/fallback-wrap.sh
```

### Script 6: notion-sync.sh

Called by `/standup` skill. Pushes hit list to Notion database.

```bash
chmod +x ~/helm/hub/scripts/notion-sync.sh
```

Requires:
- Notion API token (from `hub-settings.json`)
- `curl` (built-in on Mac)
- `jq` (installed above)

### Script 7: nas-backup.sh

Backs up critical directories to NAS via rsync.

```bash
chmod +x ~/helm/hub/scripts/nas-backup.sh
```

Requires:
- NAS mounted at path in hub-settings.json
- `rsync` (built-in on Mac)

### Script 8: session-end-update.sh

Called by project SessionEnd hooks. Updates project ops file based on git commits.

```bash
chmod +x ~/helm/hub/scripts/session-end-update.sh
```

## Phase 3: Manual Testing (20 min)

Test each script manually before automating:

```bash
# Test context builder
~/helm/hub/scripts/overnight-context-builder.sh

# Check output
cat ~/ops/daily/staging/overnight-context.md

# Check log
tail ~/helm/hub/logs/overnight-context-builder-*.log
```

Repeat for each script. All should complete without errors.

## Phase 4: Launchd Setup (Mac Only) (20 min)

### Create Plist Files

Create `~/Library/LaunchAgents/` directory if it doesn't exist:

```bash
mkdir -p ~/Library/LaunchAgents
```

For each script, create a plist file. Example for `overnight-context-builder.sh`:

**File:** `~/Library/LaunchAgents/com.helm.overnight-context.plist`

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

Create plist files for each:
- `com.helm.overnight-context.plist` (6:00am)
- `com.helm.weather-fetch.plist` (6:15am)
- `com.helm.apple-notes-inbox.plist` (6:30am)
- `com.helm.uncommitted-check.plist` (7:00am)
- `com.helm.fallback-wrap.plist` (11:45pm)
- `com.helm.nas-backup.plist` (2:00am)

### Load Services

```bash
launchctl load ~/Library/LaunchAgents/com.helm.overnight-context.plist
launchctl load ~/Library/LaunchAgents/com.helm.weather-fetch.plist
launchctl load ~/Library/LaunchAgents/com.helm.apple-notes-inbox.plist
launchctl load ~/Library/LaunchAgents/com.helm.uncommitted-check.plist
launchctl load ~/Library/LaunchAgents/com.helm.fallback-wrap.plist
launchctl load ~/Library/LaunchAgents/com.helm.nas-backup.plist
```

Verify:

```bash
launchctl list | grep helm
```

Should show 6 services.

## Phase 5: Verify Overnight Run (next morning)

When you wake up tomorrow:

```bash
# Check if context was built
ls -la ~/ops/daily/staging/overnight-context.md

# Check weather was fetched
ls -la ~/ops/daily/staging/weather.md

# Check logs for any errors
tail ~/helm/hub/logs/overnight-context-builder-*.log
```

All files should exist with recent timestamps (this morning).

If any failed, check the error logs and fix the issue before continuing.

## Phase 6: Unload (if needed)

If you need to disable a service:

```bash
launchctl unload ~/Library/LaunchAgents/com.helm.overnight-context.plist
```

To stop all HELM services:

```bash
launchctl unload ~/Library/LaunchAgents/com.helm.*.plist
```

## Linux/Cron Alternative

If you're on Linux or prefer cron over launchd:

Edit crontab:

```bash
crontab -e
```

Add entries:

```cron
0 6 * * * /home/user/helm/hub/scripts/overnight-context-builder.sh >> /home/user/helm/hub/logs/overnight-context.log 2>&1
15 6 * * * /home/user/helm/hub/scripts/weather-fetch.sh >> /home/user/helm/hub/logs/weather-fetch.log 2>&1
30 6 * * * /home/user/helm/hub/scripts/apple-notes-inbox.sh >> /home/user/helm/hub/logs/apple-notes.log 2>&1
0 7 * * * /home/user/helm/hub/scripts/uncommitted-check.sh >> /home/user/helm/hub/logs/uncommitted.log 2>&1
45 23 * * * /home/user/helm/hub/scripts/fallback-wrap.sh >> /home/user/helm/hub/logs/fallback-wrap.log 2>&1
0 2 * * * /home/user/helm/hub/scripts/nas-backup.sh >> /home/user/helm/hub/logs/nas-backup.log 2>&1
```

Save and exit. Cron will run jobs at specified times.

## Troubleshooting

**Notion sync not working?**
- Verify `hub-settings.json` has correct workspace_id and database_id
- Check that Notion integration is authorized
- Look at `~/helm/hub/logs/notion-sync-*.log` for errors

**Weather not fetching?**
- Verify `curl` and `jq` are installed
- Check internet connectivity
- Verify weather settings in `hub-settings.json`

**Scripts not running at scheduled times?**
- Check `launchctl list` to see if services are loaded
- Check logs in `~/helm/hub/logs/`
- Verify times in plist files match your intentions

**AppleScript permissions error?**
- Grant Terminal permission in System Settings → Privacy & Security → Automation
- May need to restart Terminal or the system

## Maintenance

Check scripts monthly:

```bash
# See what ran recently
ls -lt ~/helm/hub/logs/ | head -20

# Check for errors
grep ERROR ~/helm/hub/logs/*.log
```

If a script consistently fails, fix it and restart the service:

```bash
launchctl unload ~/Library/LaunchAgents/com.helm.{service-name}.plist
# Fix the issue
launchctl load ~/Library/LaunchAgents/com.helm.{service-name}.plist
```

---

**Timing:** Set up after you're comfortable with manual standup/wrap cycle (at least 1 week)
**Critical files:** `hub-settings.json`, `repo-map.json`
**Logs location:** `~/helm/hub/logs/`
**Context output:** `~/ops/daily/staging/`
