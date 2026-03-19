---
name: project-done
description: Wrap up and close out the Acme Corp website rebuild project, capture learnings, and archive
model: claude-opus-4-5
allowed-tools:
  - Bash
  - Read
  - Write
---

# Project Closure – Acme Corp Website Rebuild

Final project wrap-up: verify completion, document learnings, update ops system, archive project.

Run this skill when the final milestone is delivered and client has approved/paid.

## Closure Checklist

Before proceeding, verify:

- [ ] All deliverables completed and delivered to client
- [ ] Client has signed off (approval received)
- [ ] Final invoice sent and payment received
- [ ] All project files, code, and access transferred to client
- [ ] Hosting/domains transferred and confirmed working
- [ ] 30-day support period started (if applicable)

If anything is incomplete, don't run this skill yet. Finish the work first.

---

## Step 1: Verify Project Status in Ops System

Read the project ops file and confirm all milestones show complete:

The ops file at `~/ops/projects/acme-website-rebuild.md` should show:

```yaml
status: completed  # Or set it now
actual_end_date: [TODAY'S DATE]

milestones:
  - name: Phase 1 - Design & Setup
    status: complete
    closing:
      review_complete: true
      client_approved: true
      invoice_sent: true
      payment_received: true

  [All phases complete...]
```

If not, update the status now before proceeding.

---

## Step 2: Document Final Status

Update the project's `status-log.md` with final entry:

```markdown
## Project Complete

**Final Deliverables:**
- WordPress website rebuilt and deployed
- Custom theme with responsive design
- Salesforce CRM integration
- Contact forms with automation
- Analytics and tracking set up

**Project Duration:** Feb 20 – May 10, 2026 (12 weeks)
**Final Cost:** $15,000 (3 phases @ $5k each)
**Payment Status:** Complete ($15k received)

**Client Satisfaction:** Excellent (engaged throughout, paid on time, no scope disputes)

**What Went Well:**
- Clear milestone structure kept expectations aligned
- Client very engaged; early testing prevented major issues
- Salesforce integration handled well despite initial API access delays
- No major scope creep or budget overruns

**What Would We Change:**
- Request design assets upfront (lesson learned)
- Get API access and documentation earlier in kickoff
- Plan for legal review cycles on enterprise projects (Acme's 4-week legal review)
- Salesforce integrations need 20% time buffer (was tighter than expected)

**Client Feedback:**
- Jane Smith (primary contact): "Smooth project, clear communication, hit timeline"
- John Chen (CTO/decision maker): "Technical work was solid; API integration worked well"

**Lessons for Future:**
- This pricing model ($15k for 12-week web+integration) works well
- Clients at this price point (enterprise) expect legal review cycles; plan 4 weeks between phases
- Salesforce integration experience is valuable; can charge premium for this
- Clear milestone structure + Friday status emails = happy clients
```

---

## Step 3: Capture Learnings for Memory

Add to `~/business/memory/pricing-history.md`:

```markdown
## Acme Corp – Website Rebuild & Salesforce Integration (2026)

**Project Type:** Web rebuild + custom integrations
**Total Value:** $15,000
**Duration:** 12 weeks (Feb 20 – May 10, 2026)
**Payment Terms:** 3 phases @ $5k each (Net 30)

**What Worked:**
- Milestone-based structure with clear deliverables
- Friday status emails (client preferred async over meetings)
- Early testing/UAT (client was very engaged)
- Salesforce integration expertise was a differentiator
- Clear scope definition prevented scope creep

**What We'd Adjust:**
- Request all design assets upfront (ours requested mid-phase)
- Get API access in kickoff, not mid-Phase 2
- Plan for 4-week enterprise legal review cycles
- Salesforce work needs +20% time buffer (integration was more complex than estimated)

**Client Behavior:**
- Jane (PM): Responsive, organized, prefers email updates
- John (CTO): Technical, thorough, slower to respond but good decisions
- Decision cycle: 3-5 days typical
- Payment: On time, prefers ACH

**Pricing Lesson:**
- $15k is good pricing for this scope + integration work
- For 2027, can increase to $18-20k as we gain more Salesforce experience
- CRM integration work is high-margin; should focus here

**Would We Take Again?**
- Yes. Great client, good payment terms, clear scope, valuable experience.
- This is the type of project to pursue more of.

**Success Criteria Met:**
- ✅ Phase 1: Clean design, mobile responsive, client happy with look/feel
- ✅ Phase 2: Salesforce data flowing, forms working, analytics tracking
- ✅ Phase 3: Zero critical bugs, clean launch, client trained on updates
```

Also update `~/business/memory/clients.md` with Acme Corp notes if not already there.

---

## Step 4: Archive the Project

Move the project folder to archive:

```bash
# Create archive if it doesn't exist
mkdir -p ~/projects/archive

# Move project folder
mv ~/projects/acme-website-rebuild ~/projects/archive/acme-website-rebuild

# If using git, commit the archival
cd ~/projects/archive/acme-website-rebuild
git commit -am "Project complete: Acme Corp website rebuild delivered 2026-05-10"
```

---

## Step 5: Send Closure Email

Draft and send final email to client (using your voice from `~/ops/templates/voice-guide.md`):

```
To: jane@acme.com
Cc: john@acme.com
Subject: Acme Corp Website Rebuild – Project Complete

Hi Jane and John,

The website rebuild is officially complete and delivered. Here's the status:

**Deliverables:**
- WordPress website fully deployed and live
- Custom responsive theme
- Salesforce integration (contact forms, lead tracking)
- Analytics and conversion tracking set up
- Documentation and training completed

**Handoff:**
- All project files, source code, and credentials transferred to your team
- Hosting configured and ready for your team to manage
- 30-day support period has started (contact me with any issues)

**Access & Admin:**
- [Include any credentials, links, or access info]
- Support email: [Your email]
- Support hours: [Your availability]

It was great working with you and your team. You were engaged, collaborative, and easy to work with—the kind of project we enjoy. If you need anything in the future, reach out anytime.

Thank you,
[Your name]
```

---

## Step 6: Final Checklist

Mark complete in the system:

Update `~/ops/projects/acme-website-rebuild.md`:

```yaml
status: completed
actual_end_date: 2026-05-10
project_url: https://acme.example.com
```

---

## What Happens Next

**For the client:**
- 30-day support period (for any bugs or issues)
- After 30 days, shift to retainer or project-based if they want ongoing work

**For you:**
- Project archived; lessons captured
- Pricing and client patterns recorded in memory
- Ready to pursue similar projects or next client
- Case study material if appropriate (with client permission)

**For future similar projects:**
- You now have pricing data
- You know what to expect from enterprise clients
- You can charge more confidently (or bid faster)
- You have patterns on what works

---

## Common Follow-Up Actions

**If client wants ongoing support:**
- Propose retainer (e.g., "$2,000/month for 10 hours maintenance + support")
- Document in new project file with retainer status

**If client wants enhancements:**
- Treat as new project or change order
- Follow proposal → contract → new-project workflow

**If client asks for referrals:**
- Get testimonial if they're happy
- Ask permission to use as case study
- Offer referral bonus if applicable

**If you want a case study:**
- Ask client for a brief testimonial
- Ask permission to share the project publicly
- Document metrics (if appropriate): timeline, budget, technology, results

---

**Project closed. You can now focus on the next one.**
