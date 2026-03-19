---
name: new-contract
type: skill
description: Generate contract from accepted proposal. Assembles reusable blocks, uses reviewer agent, updates project status.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
disable-model-invocation: false
---

# New Contract Skill

Takes an accepted proposal and generates a legally-sound contract from reusable blocks, then verifies it against standard positions.

## Phase 1: Load Proposal Context

Ask which proposal this is for:
- "Which proposal are we turning into a contract?"
- Load `~/business/proposals/{client}/{proposal-file}.md`
- Extract: client name, scope, timeline, investment, payment terms

## Phase 2: Select Template Structure

Reference `~/ops/templates/contract-structure.md` which outlines:

```markdown
# Contract Structure

## Header
- Parties (your company, their company)
- Effective date
- Contract value and payment terms

## Scope of Work
- Phase 1, 2, 3 (from proposal)
- What's included, what's not
- Deliverables and timeline
- Acceptance criteria

## Terms
- Payment schedule (milestones)
- Payment method (wire, ACH, check)
- Late payment terms (if any)
- Invoicing frequency

## Legal Blocks (see templates/blocks/)
- **IP Assignment** – Who owns the code/design?
- **Warranty** – Your guarantee period (typically 30 days)
- **Limitation of Liability** – Cap on damages
- **Confidentiality** – NDA terms
- **Termination** – How to exit the contract
- **Dispute Resolution** – How to handle disagreements

## Signatures
- Your signature line
- Their signature line
- Date spaces
```

## Phase 3: Assemble Contract

Create the contract by combining:

1. **Structure from templates/contract-structure.md**
2. **Reusable legal blocks from templates/blocks/**
   - `payment-terms.md` – Standard payment language
   - `ip-assignment.md` – IP ownership clause
   - `warranty.md` – 30-day warranty standard
   - `termination.md` – How contract can end
   - `liability.md` – Limitation of liability

3. **Replace placeholders:**
   - {CLIENT_NAME} → Actual company name
   - {PROJECT_NAME} → Project name from proposal
   - {TOTAL_VALUE} → Contract value
   - {PAYMENT_1}, {PAYMENT_2}, etc. → Milestone amounts
   - {PHASE_1_DELIVERABLES} → What they get
   - {TIMELINE} → When it ships
   - {HOURLY_RATE} → If relevant for change orders
   - {YOUR_NAME} → Your legal name
   - {YOUR_COMPANY} → Your business entity

## Phase 4: Review with Reviewer Agent

Call the reviewer agent to check the contract:

```
Invoke reviewer agent with contract and ask:
- Are all required clauses present?
- Do payment terms match what we proposed?
- Are there any contradictions or gaps?
- Does scope match the proposal exactly?
- Are milestone deliverables clear enough to enforce?
- Any red flags in this version for your standard positions?

Reviewer checks against:
- ~/business/memory/clients.md (client history, any special terms)
- ~/ops/templates/blocks/ (standard language consistency)
- Industry standards (for your type of work)
```

Reviewer will surface:
- Missing clauses that should be there
- Language that differs from your standard blocks (consistency check)
- Potential ambiguities in scope or timeline
- Payment terms that deviate from your standard

## Phase 5: Adjust If Needed

Reviewer flags issues. You decide:

**Common adjustments:**
- **Scope ambiguity:** Tighten language in Phase descriptions
- **Timeline risk:** Add "subject to client providing assets by [date]"
- **Payment risk:** Request larger up-front payment if client is new
- **IP concern:** Clarify that you own templates/frameworks; they own their content
- **Late payment:** Consider adding 1.5% monthly interest clause

**Never budge on:**
- Your liability limitation (critical for small business)
- IP ownership of your frameworks/processes
- 30-day warranty period (protection and reasonable)

## Phase 6: Finalize

Once reviewer approves:

```bash
# Save contract
mkdir -p ~/business/contracts/{client-name}
# File location: ~/business/contracts/{client-name}/contract-{project-name}-{date}.md
```

Document the contract in the project ops file:

```yaml
# Update ~/ops/projects/{project-name}.md
status: active
contract:
  value: $XXXX
  signed_date: YYYY-MM-DD
  milestones:
    - name: Phase 1
      value: 5000
      status: not-started
      due: YYYY-MM-DD
    - name: Phase 2
      value: 5000
      status: not-started
      due: YYYY-MM-DD
    - name: Phase 3
      value: 5000
      status: not-started
      due: YYYY-MM-DD
```

## Phase 7: Send for Signature

You handle this part manually (contract negotiation sometimes needs a human touch).

```
Generate signature email:
---
Hi {Client Contact},

Attached is the contract for {Project Name}. When you're ready,
please have {Decision Maker} review and sign.

Key terms:
- Total value: ${TOTAL}
- Milestones: {Summary of payment schedule}
- Timeline: {Start date} to {End date}
- Our commitment: {What they're getting}

Please let me know if you have questions on any terms.

Best,
{Your Name}
---
```

## Phase 8: Update Project Status

When contract is signed by both parties:

```yaml
# Update ~/ops/projects/{project-name}.md
status: active
contract:
  signed_date: YYYY-MM-DD
  client_signed: true
  you_signed: true
start_date: YYYY-MM-DD
```

Create project repo:
```bash
# Run /new-project to scaffold repo structure
```

## Common Contract Scenarios

### New Client (High Risk)
- Higher up-front payment (50% instead of 33%)
- Shorter payment terms (Net 15 instead of Net 30)
- Add references/case studies section
- Consider requesting email approval from decision maker before signing

### Existing Client (Low Risk)
- Standard terms from your contract blocks
- Can offer slightly better terms (Net 30 vs Net 15)
- May skip some formality, but still use contract

### Rapid Timeline Project
- Add "client will provide assets by [DATE] to maintain schedule"
- Include rush fee if they want faster delivery
- Milestone 1 due sooner, money sooner

### Uncertain Scope
- Use "fixed + overage" model
- Add change order process explicitly
- Set hourly rate for out-of-scope work: {HOURLY_RATE}

## Standard Positions (Non-Negotiable)

Your contracts should maintain these positions:

1. **IP Assignment:** Client owns the final deliverable. You own your frameworks/processes/templates.
2. **Warranty:** 30 days. Covers bugs you introduced, not client changes.
3. **Limitation of Liability:** Capped at contract value. Protects you from catastrophic claims.
4. **Payment Terms:** Net 30 is standard. Shorter if new client, longer only with premium.
5. **Confidentiality:** Mutual. You don't share their info, they don't share yours.
6. **Termination:** Client can terminate with 14 days notice + payment for completed work.

## Red Flags to Resist

- **Unlimited liability:** No. Cap it at contract value.
- **IP ownership of frameworks:** No. They own their content, you own your tools.
- **Non-compete:** Only if they're paying premium for exclusivity.
- **No payment terms:** Require Net 30 maximum. Longer is cash flow risk.
- **Vague scope:** Make it specific or add change order clause.
- **No timeline:** Bad for both of you. Set milestones explicitly.

## After Signature

Contract signed → Project starts:
1. Create project repo (via `/new-project`)
2. Update ops file with start date
3. Set up milestone tracking
4. Daily standup includes this project
5. SessionEnd hooks auto-update status

---

**Reads:**
- ~/business/proposals/{client}/ (proposal document)
- ~/ops/templates/contract-structure.md (contract template)
- ~/ops/templates/blocks/ (reusable legal language)
- ~/business/memory/clients.md (client history)

**Writes:**
- ~/business/contracts/{client}/ (final contract document)
- ~/ops/projects/{project-name}.md (project ops file with contract details)

**Calls:** reviewer agent (to validate contract)

**Next steps:** `/new-project` to scaffold work, then daily standup includes new project
