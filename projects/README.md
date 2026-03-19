# Projects

Active client projects and archive.

## Structure

```
projects/
  example-project/               Active project workspace
    CLAUDE.md                    Project overview
    instruction.md               Setup guide
    README.md                    Client-facing summary
    status-log.md                Weekly progress updates
    .claude/
      skills/
        done/
          SKILL.md               Project closure workflow

  archive/                       Completed projects
    [completed-project]/
    [past-project]/
```

## How It Works

Each active project gets its own workspace. The workspace contains:

- **CLAUDE.md** – Context about what this project is
- **instruction.md** – Setup guide (if you're creating a new project)
- **status-log.md** – Weekly progress updates
- **Project skill** – Project-specific automation (e.g., done skill for closure)

The parent `~/ops/projects/` directory contains the ops file (the "brain" that daily standup and retrospective read).

## Active Projects

The `/standup` skill reads each project's ops file daily and shows:
- Current phase and progress
- Blockers and dependencies
- Next immediate action
- Waiting on items (client decisions, dependencies)

## Project Lifecycle

1. **New Project**
   ```bash
   claude skill new-project
   ```
   Creates folder structure, ops file, GitHub repo (if applicable)

2. **Active Work**
   - Daily standup reads project status
   - Weekly updates to status-log.md
   - Track blockers and waiting_on items
   - Manage client communication

3. **Completion**
   ```bash
   claude skill done
   ```
   Wraps up project, captures learnings, archives folder

## Examples

**example-project/** – Full example of a 3-phase website project (Acme Corp website rebuild, $15,000, 12 weeks)

This shows:
- How to structure milestones
- How to track deliverables
- How to document blockers
- How to capture client communication
- How to close and archive

Use this as a template when creating new projects.

## File You Should Know

**~/ops/projects/** – The ops directory with project metadata files (read by standup/retrospective)

These are separate from the workspace folders. The ops files are what the broader system reads for scheduling and prioritization.

## Quick Reference

**Add a new project:**
```bash
claude skill new-project
```

**Update project status:**
```bash
cd ~/projects/[project-name]
# Edit status-log.md with weekly update
git commit -am "Weekly update: [week date]"
```

**Complete and archive a project:**
```bash
cd ~/projects/[project-name]
claude skill done
```

**Check daily standup (includes all active projects):**
```bash
cd ~/business
claude skill standup
```
