# Business Workspace Setup

Follow these steps in order to get your business operations system running.

## Phase 1: Foundation (30 min)

### 1. Install Notion Plugin
- Go to Notion workspace settings → Connected apps
- Search for "Claude Code" or your automation tool
- Grant read/write permissions to your Business database
- Note your workspace ID (you'll need this in hub/config/hub-settings.json)

### 2. Verify Skills Load
Run in business/:
```bash
ls -la .claude/skills/*/SKILL.md
```
You should see all 10 skill files listed. If any are missing, check the file list against CLAUDE.md.

### 3. Set Up Memory Directory
```bash
mkdir -p business/memory
touch business/memory/patterns.md business/memory/clients.md
```
Populate from templates:
- `patterns.md` – Start with empty sections (populated by /retrospective)
- `clients.md` – Add your 3-5 most important clients with contact/communication info

### 4. Feed Voice Extraction Data
For the /new-proposal skill to match your voice, give it training data:
```bash
# In an editor, populate ops/templates/voice-guide.md with:
# - Your typical writing style (concise? verbose? formal? casual?)
# - Tone examples from past proposals you've sent
# - What you never say
# - 2-3 example sentences in your voice
```

Also populate `business/memory/clients.md` with real or realistic client entries so /new-proposal can reference past relationships.

### 5. Set Up Agents
Agents are invoked by skills; they don't need activation, but verify they exist:
```bash
ls -la business/.claude/agents/
```
Should see `writer.md` and `reviewer.md`. These are templates the skills invoke.

## Phase 2: Testing (15 min per cycle)

### 6. Test Full Standup Cycle
```bash
cd business
claude skill standup
```
This should:
- Greet you with "how are you doing today?"
- Load your weekly plan (if available) and daily instructions
- Ask energy/mood/focus check-in questions
- Show landscape of projects (payment proximity weighted)
- Suggest 2-4 items with micro-steps
- Generate a daily file in ops/daily/staging/
- Sync to Notion (if configured)
- Stay open for your morning work

After this runs, check:
- `ops/daily/staging/YYYY-MM-DD.md` exists with your plan
- Your Notion business inbox is synced (if connected)

### 7. Test Weekly Cycle
Monday morning:
```bash
cd business
claude skill weekly-plan
```
This should:
- Load Friday's retrospective (ops/daily/weekly/retrospective.md)
- Show all project statuses and financial calendar
- Set 3-5 weekly objectives
- Write to ops/daily/weekly/plan-{week}.md

### 8. Test Wrap Skill
End of day:
```bash
cd business
claude skill wrap
```
This should:
- Load today's plan
- Have a brief conversation about what actually happened
- Write "What Actually Happened" and "Carried Forward" sections
- Pre-stage tomorrow's context

After it runs, check:
- `ops/daily/staging/YYYY-MM-DD-wrap.md` contains your daily summary
- Next day's instructions are staged in `ops/daily/instructions/`

## Phase 3: Operational Details

### 9. Checklist Creation
First time you need a financial checklist (quarterly tax, quarterly retro, etc.):
- Add entry to financial/financial-ops.md
- Create file in financial/checklists/{checklist-name}.md
- System will evolve this based on what you actually do each cycle

### 10. Daily Project Instruction Files
After /standup, per-project daily files are written to:
- `~/ops/projects/{project-name}/daily/YYYY-MM-DD.md`

These are read by project /done skill to track milestones. Create them manually if /standup doesn't generate them (for projects not yet in standup rotation).

## Quick Reference

- **Daily:** /standup (morning), /wrap (evening)
- **Weekly:** /weekly-plan (Monday), /retrospective (Friday)
- **As-needed:** /new-proposal, /new-contract, /follow-up, /new-project, /done, /update
- **Memory:** Updated by /retrospective; read by /standup and /weekly-plan
- **Notion sync:** /standup writes, so your Notion business DB stays current

## Troubleshooting

**Skills not loading?**
- Check that all SKILL.md files exist in `.claude/skills/*/`
- Verify directory structure matches CLAUDE.md skill list

**Standup not loading context?**
- Create `ops/daily/staging/` and `ops/daily/instructions/` directories
- Verify `personal/plan.md` exists (can be empty)
- Check that project ops files follow ops/projects/{name}.md convention

**Notion sync failing?**
- Verify Notion plugin is installed and permissions are granted
- Check hub-settings.json has correct workspace ID
- Run /standup in verbose mode to see sync logs

