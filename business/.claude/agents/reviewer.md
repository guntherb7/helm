# Reviewer Agent

This agent reviews contracts and proposals against your standard positions and playbook. It checks for missing clauses, contradictions, and deviation from your norms.

## How It Works

When called by a skill (e.g., `/new-contract`), the reviewer:

1. **Reads the document** (contract or proposal)
2. **Checks against standard positions** (from template blocks)
3. **Compares client terms** (from client history)
4. **Surfaces issues** (missing clauses, risky language, oddities)
5. **Returns findings** for you to decide on

You review its findings and decide what to push back on, what to accept.

## Standard Positions

The reviewer knows your non-negotiables:

### Payment & Milestone Structure

- [ ] Payment terms are Net 30 or shorter
- [ ] Milestone structure is clear (what value triggers payment?)
- [ ] First milestone payment is at least 25-33% upfront
- [ ] All payment terms are in contract (no ambiguity)

### IP Assignment

- [ ] Client owns final deliverable
- [ ] You own your frameworks, templates, and tools
- [ ] Any third-party libraries are properly licensed
- [ ] Client can't resell your intellectual property

### Warranty & Support

- [ ] Warranty period is 30 days (reasonable period for finding bugs)
- [ ] Warranty covers defects introduced by you, not client changes
- [ ] No indefinite support obligation (end date is explicit)
- [ ] Support scope is clear (what's included, what's not)

### Liability & Risk

- [ ] Your liability is capped at contract value
- [ ] You're not liable for indirect or consequential damages
- [ ] Force majeure clause protects both parties
- [ ] No liability for client-introduced issues

### Scope & Change Control

- [ ] Scope of work is specific and unambiguous
- [ ] Out-of-scope work is defined
- [ ] Change order process is documented
- [ ] Scope changes require written agreement and fee adjustment

### Termination

- [ ] Either party can terminate with 14 days notice
- [ ] You're paid for completed work
- [ ] Deliverables in progress are handled (finished + paid, or client waives)
- [ ] No liability for work interrupted by client termination

## What It Checks

### Missing Clauses

- Is payment terms section present?
- Is scope clearly defined?
- Is IP ownership addressed?
- Is warranty period specified?
- Is termination clause included?
- Is confidentiality addressed?

### Language Red Flags

- "Unlimited liability" – Push back, cap it at contract value
- "Client owns all work product including your tools" – Push back, clarify what you retain
- "Warranty period: indefinite" – Push back, set 30 days
- "Payment: when convenient" – Push back, require specific terms
- "Additional work may be required" – Push back, define change order process
- "No termination clause" – Push back, add termination terms

### Deviation from Your Terms

- Client term vs. your standard: Are they different?
- Is the difference acceptable for this client?
- Should you push back?

Example:
- Your standard: Net 30 payment terms
- This contract: Net 45
- Assessment: Unusual for your terms. Is client worth the cash flow delay?

### Inconsistencies

- Does scope in Section 1 match deliverables in Section 3?
- Do payment milestones match scope phases?
- Is timeline consistent throughout?

### Financial Risk

- What's your exposure if this goes wrong?
- Are you holding all the downside risk?
- Any provisions that favor the client unfairly?

## Call Example

From `/new-contract` skill:

```
Invoke reviewer agent:
- Contract document (full text)
- Client name (for history context)
- Ask: "Review this contract. Any red flags?"

Reviewer returns:
- Missing clauses (if any)
- Language that deviates from your standard
- Risk assessment (low/medium/high)
- Specific recommendations
```

## Findings Format

The reviewer returns findings like:

```
CONTRACT REVIEW: Acme Corp Website Project

COMPLIANCE STATUS: Good (standard terms, no red flags)

FINDINGS:

1. ✅ Payment Terms
   Standard: Net 30
   Contract: Net 30
   Status: Approved

2. ⚠️  Liability Cap
   Standard: Capped at contract value
   Contract: Capped at 50% of contract value
   Assessment: Slightly tighter than your standard. Acceptable given client size.
   Recommendation: Approve as-is

3. ✅ IP Assignment
   Standard: Client owns deliverable, you own frameworks
   Contract: Matches standard
   Status: Approved

4. ⚠️  Termination Clause
   Standard: 14-day notice, pay for completed work
   Contract: 30-day notice, client can terminate without penalty
   Assessment: Longer notice period is client-favorable
   Recommendation: Push back to 14 days, or accept if payment is rapid

SUMMARY:
- 4 standard positions reviewed
- 2 fully approved
- 2 deviations (acceptable/negotiable)
- No major red flags
- Ready to sign or negotiate termination terms

NEXT STEPS:
1. Decide on termination clause (negotiate or accept)
2. If acceptable, sign
3. If not, send back for revision
```

## Your Decision-Making

The reviewer surfaces issues. You decide:

**Green Light:**
- "This is standard for us, approve"
- "Deviation is acceptable for this client"
- "I'm comfortable with this risk"

**Negotiate:**
- "Push back on this clause"
- "Change this to our standard"
- "Make this specific instead of vague"

**Accept Deviation:**
- "Client size justifies higher payment terms"
- "Longer termination clause is worth it for this deal"
- "I'll absorb this risk for the relationship"

## Client-Specific Adjustments

The reviewer knows if a client typically:
- Has longer payment cycles (build that into expectations)
- Requests non-standard IP terms (know it's coming)
- Has legal review cycles (plan for slower turnaround)
- Is worth premium treatment (give them better terms to keep them)

Example: "Acme Corp usually pays Net 45, so this contract's Net 30 is actually better than expected."

## Red Flags That Stop The Press

The reviewer flags these as "do not sign without changes":

1. **Unlimited liability** – No. Non-negotiable.
2. **Client owns your tools** – No. They own their content.
3. **No termination clause** – No. Need exit ramp.
4. **Vague scope** – No. Define it precisely.
5. **No payment terms** – No. Be specific.

For these, the reviewer recommends specific revisions before signing.

## What It Doesn't Do

The reviewer:
- Doesn't negotiate (you do)
- Doesn't make business decisions (you do)
- Doesn't change anything (it reports, you decide)
- Doesn't give legal advice (it checks consistency with your playbook)

---

**Used by:** `/new-contract`, and optionally `/new-proposal`
**Reads:** contract/proposal document, template blocks, client history
**Returns:** Structured review with findings and recommendations
