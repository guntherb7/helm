---
name: update
type: skill
description: Quick one-liner status append. No conversation. Usage - /update project-name Finished feature A, tests passing
allowed-tools:
  - Read
  - Write
  - Bash
  - Grep
disable-model-invocation: false
---

# Update Skill

Rapid one-liner status append. No conversation—just log what you did.

## Usage

```
/update {project-name} {Brief description of what changed}
```

Examples:

```
/update acme-corp Built payment integration, all tests passing
/update client-website Deployed Phase 1 to staging, awaiting client review
/update consulting-project Completed discovery phase analysis, ready for presentation
```

## What It Does

1. **Parse arguments**
   - First word = project name (or UUID to match against `~/ops/projects/`)
   - Rest = update text

2. **Append to Recent Activity**
   ```yaml
   recent_activity:
     - date: 2026-03-19
       update: Built payment integration, all tests passing
   ```

3. **Commit**
   ```bash
   cd ~/ops
   git add projects/{project-name}.md
   git commit -m "[Project] {Your update text}"
   ```

4. **Done**

## When to Use

Use `/update` when:
- You just finished something and want to log it quickly
- You don't need to update milestone status or waiting items
- You don't need a full conversation
- You're in the flow and don't want to context-switch

Use `/done` when:
- You want to update milestone deliverable status
- You need to add waiting items
- Scope changed and you need to document it
- You're wrapping up a session and want a full check-in

## Examples

**Quick completion:**
```
/update example-project Added user profile page, responsive design complete
```
→ Logs "Added user profile page, responsive design complete" to recent_activity

**Bug fix:**
```
/update example-project Fixed critical payment button bug, tests added
```
→ Logs the fix for morning standup context

**Waiting status:**
```
/update example-project Sent client review request, awaiting feedback
```
→ Logs what you did (you can add waiting_on items with `/done` later)

**Deliverable ready:**
```
/update example-project Phase 2 deliverables complete, ready for client review
```
→ Logs it so standup knows to ask about delivery next morning

## Behind the Scenes

The skill:

1. **Finds the project** in `~/ops/projects/` (searches for name or UUID match)
2. **Appends to recent_activity** with today's date
3. **Commits to git** with message "[Project] {your text}"
4. **Returns** instantly

```bash
# This is what runs:
cat << EOF >> ~/ops/projects/{project-name}.md
  - date: $(date +%Y-%m-%d)
    update: {Your text}
EOF

git add projects/{project-name}.md
git commit -m "[{project-name}] {Your text}"
```

## Why One-Liners?

Fast logging keeps you in flow. You don't want to context-switch to fill out a form.

The `/standup` skill reads your recent_activity log every morning, so these one-liners become context for daily planning.

And if you need to do a full status check later, `/done` reads the same file and can expand on any of these one-liners.

## Project Name Matching

The skill tries to match your input against projects in `~/ops/projects/`:

- **Exact match:** `/update acme-corp ...` → finds `acme-corp.md`
- **Partial match:** `/update acme ...` → finds `acme-corp.md` if unambiguous
- **UUID match:** `/update 3fd6b891 ...` → finds by project ID

If ambiguous, it asks which one.

## No Output

This skill is silent on success. It just updates and commits.

If it fails (project not found, can't write), it tells you clearly.

## Integration with Standup

The `/standup` skill reads `recent_activity` every morning:

```
RECENT ACTIVITY
- Finished payment integration (Mar 19)
- Sent client review request (Mar 18)
- Phase 2 deliverables complete (Mar 17)
```

This gives standup context about what you've actually done, so it can weight priorities appropriately.

## Syntax

```
/update {project} {text}
       ↑            ↑
       |            └─ Everything else is your update message
       └─ First word, must be a project you have
```

Examples of valid syntax:

```
/update acme-corp Finished feature
/update client-website Code review done, ready to merge
/update project-1 Added authentication, blocked on API docs
/update "example project" Message can be anything
```

Project names with spaces work if quoted.

---

**Inputs:** project name + update text
**Outputs:** Updated project ops file + git commit
**Speed:** ~1 second
**Use case:** Quick logging, keep momentum
