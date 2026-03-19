---
name: example-project
client: Acme Corp
type: web-development
status: active
start_date: 2026-02-20
contract:
  value: 15000
  signed_date: 2026-02-15
  contact: jane@acme.com
  decision_maker: john@acme.com
  payment_terms: Net 30
  payment_method: ACH

milestones:
  - name: Phase 1 - Design & Setup
    value: 5000
    due: 2026-03-20
    status: in-progress
    deliverables:
      - WordPress setup: complete
      - Custom theme design: 95%
      - Mobile responsive testing: in-progress
      - Admin panel customization: not-started
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
      - Analytics setup: not-started
      - Performance optimization: not-started
    closing:
      review_complete: false
      client_approved: false
      invoice_sent: false
      payment_received: false

  - name: Phase 3 - Testing & Launch
    value: 5000
    due: 2026-05-10
    status: not-started
    deliverables:
      - QA testing and bug fixes: not-started
      - Client UAT coordination: not-started
      - Content migration and launch: not-started
      - 30-day support and training: not-started
    closing:
      review_complete: false
      client_approved: false
      invoice_sent: false
      payment_received: false

waiting_on:
  - what: Design assets from design firm
    who: Acme Corp (they're sourcing from contractor)
    since: 2026-03-12
    follow_up_interval_days: 7
    last_follow_up: 2026-03-17
    status: open
    note: Final asset batch expected by March 19

  - what: Salesforce API documentation and sandbox access
    who: John Chen (CTO)
    since: 2026-03-01
    follow_up_interval_days: 10
    last_follow_up: 2026-03-15
    status: follow-up-sent
    note: Requested twice. John said "next week" on Mar 15.

communication_log:
  - date: 2026-03-17
    type: email
    to: jane@acme.com
    subject: "Phase 1 progress update – on track for March 20"
    summary: "95% complete on design integration. Waiting on final asset batch. Shared staging link for review."

  - date: 2026-03-15
    type: call
    to: "jane@acme.com, john@acme.com"
    subject: "Phase 1 review call"
    summary: "Discussed timeline, answered questions on Salesforce integration, clarified Phase 2 scope. John to provide API docs."

  - date: 2026-03-12
    type: email
    to: jane@acme.com
    subject: "Design assets request"
    summary: "Asked for final design comps from design firm. Jane said they're finalizing."

  - date: 2026-02-20
    type: call
    to: jane@acme.com
    subject: "Project kickoff"
    summary: "Reviewed scope, confirmed timeline, tech stack, and communication preferences."

---

# Acme Corp – Website Rebuild

A WordPress website rebuild and Salesforce integration for Acme Corp's product launch. Three-phase project over 12 weeks.

## Contract Summary

- **Total Value:** $15,000 (3 phases × $5,000 each)
- **Milestones:** Phase 1 (design/setup), Phase 2 (integration), Phase 3 (testing/launch)
- **Timeline:** Feb 20 – May 10, 2026
- **Primary Contact:** Jane Smith (jane@acme.com)
- **Decision Maker:** John Chen, CTO (john@acme.com)
- **Payment:** $5k upon signature + $5k at Phase 1 completion + $5k at Phase 2 completion

## Current Focus

**Phase 1 – Design & Setup (95% complete)**

Finishing custom WordPress theme integration and mobile responsive testing. Phase 1 deliverables are on track for March 20 deadline.

Waiting on final design asset batch from Acme's design firm (expected March 19). Once received, theme integration completes immediately.

Admin panel customization begins once design assets arrive.

## Blockers

**Design assets (CRITICAL)** – Acme's design firm is finalizing the last batch. Not our blocker, but critical path item. Follow-up scheduled for March 19 if not received.

**Salesforce API access (MEDIUM)** – John promised API sandbox + documentation. Last follow-up was March 15 ("next week"). May delay Phase 2 start if not received by March 22. Flag for follow-up by March 20.

## Recent Activity

- **[2026-03-17]** Design integration 95% complete. Admin panel ready to start once assets arrive. Shared staging link with Jane for initial review.
- **[2026-03-15]** Kickoff call with Jane and John. Tech stack confirmed. Salesforce integration scope clarified. John committed to API access by "next week."
- **[2026-03-12]** Requested final design asset batch from Jane. She coordinating with design firm.
- **[2026-03-10]** Custom theme design complete. Responsive testing in progress.
- **[2026-02-20]** Project kicked off. WordPress environment spun up, theme development started.

## Notes

**Client Personality:**
- Jane (primary): Organized, responsive, prefers async email updates. Very collaborative.
- John (decision maker): Technical, asks detailed questions. Slower to respond but thorough.
- Decision cycles: 3-5 days typical for Acme. Their legal team needs contract review before Phase 2 (4-week process—plan for this in April).

**Strengths & Preferences:**
- Acem prefers weekly Friday status emails (structured format)
- They respect deadlines and ask about realistic timelines
- They had bad experience with agency that over-promised. Appreciate transparency about blockers.
- Very engaged. Testing early and often is in their culture.

**Risks:**
- Design firm is external variable. If they slip, our Phase 1 deadline is at risk. Monitor closely.
- Salesforce integration is first time we're integrating their CRM. John has strong opinions. Plan for iteration/clarification in Phase 2.
- Legal review (4 weeks) happens between Phase 1 and Phase 2. Build into timeline planning.

**Success Criteria:**
- Phase 1: Clean design implementation, mobile responsive, admin panel functional, client happy with look/feel
- Phase 2: Salesforce data flowing correctly, forms working, analytics tracking events
- Phase 3: Zero critical bugs, clean launch, client team trained on updates

## Change Requests

None yet.

## Scope Clarifications

**Out of Scope (explicitly discussed with John):**
- Custom Salesforce workflows (they have a Salesforce consultant for that)
- Advanced analytics (just basic GA4 setup and event tracking)
- Email marketing integration (can be Phase 2+ if they want)

**In Scope but Deferred to Phase 2+:**
- Advanced admin features (custom reporting, bulk uploads)
- Mobile app integration (if they want app later)

## Payment Pipeline

**Phase 1 (Due March 20):**
- [ ] Deliverables complete (design integration)
- [ ] Client testing and approval
- [ ] Invoice sent to Jane
- [ ] Payment received (30 days from invoice, so ~April 20)

**Phase 2 (Due April 20):**
- [ ] Dependent on Phase 1 approval + Salesforce API access
- [ ] Integration complete, client UAT
- [ ] Invoice sent
- [ ] Payment received (~May 20)

**Phase 3 (Due May 10):**
- [ ] Testing and launch
- [ ] Invoice sent
- [ ] Payment received (~June 10)

## Lessons (for future similar projects)

- Ask for design assets earlier (lesson: request in kickoff, not mid-phase)
- Get API access upfront (lesson: add to kickoff dependencies)
- Plan for legal review cycles upfront (lesson: Acme's 4-week legal cycle is normal for enterprise clients)
- Salesforce integrations take longer than estimated (lesson: add 20% buffer for CRM work)

---

**Status Summary for Standup:** Phase 1 95% done, on track for March 20 deadline pending design assets. Waiting on Salesforce API access (John). No major blockers on our side. Client very engaged and happy so far.
