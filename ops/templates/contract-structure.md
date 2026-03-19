# Contract Template Structure

Use this structure for all contracts. The `/new-contract` skill will assemble from reusable blocks in `ops/templates/blocks/`.

---

# Agreement for Services

**This Agreement is entered into as of {SIGNATURE_DATE}**

**BETWEEN:**

{YOUR_LEGAL_NAME}, doing business as {YOUR_BUSINESS_NAME}
("Service Provider")

**AND:**

{CLIENT_LEGAL_NAME}
("Client")

---

## 1. Scope of Work

### 1.1 Services

Service Provider agrees to provide the following services:

**Phase 1: {Phase Name}**
- {Deliverable 1}
- {Deliverable 2}
- {Deliverable 3}

**Phase 2: {Phase Name}**
- {Deliverable 1}
- {Deliverable 2}

**Phase 3: {Phase Name}**
- {Deliverable 1}
- {Deliverable 2}

### 1.2 Timeline

- Project Start: {START_DATE}
- Phase 1 Due: {DATE}
- Phase 2 Due: {DATE}
- Phase 3 Due: {DATE}
- Project End: {END_DATE}

### 1.3 What's Not Included

The following are explicitly out of scope:
- {Item 1}
- {Item 2}
- {Item 3}

Any work outside this scope will be handled via written change order (Section 7).

---

## 2. Investment & Payment Terms

### 2.1 Total Investment

| Phase | Description | Amount |
|:------|:------------|:--------|
| Phase 1 | {Description} | ${PHASE_1_VALUE} |
| Phase 2 | {Description} | ${PHASE_2_VALUE} |
| Phase 3 | {Description} | ${PHASE_3_VALUE} |
| **Total** | | **${TOTAL_VALUE}** |

### 2.2 Payment Schedule

**[INSERT block from templates/blocks/payment-terms.md]**

### 2.3 Late Payments

If payment is more than 15 days late, Service Provider may suspend work until payment is received.

---

## 3. Intellectual Property

**[INSERT block from templates/blocks/ip-assignment.md]**

---

## 4. Warranty

**[INSERT block from templates/blocks/warranty.md]**

---

## 5. Limitation of Liability

**[INSERT block from templates/blocks/liability.md]**

---

## 6. Confidentiality

### 6.1 Confidential Information

Each party agrees to keep the other's confidential information private and not disclose it to third parties without written permission.

### 6.2 Exceptions

Confidentiality does not apply to:
- Information already public
- Information required to be disclosed by law
- General business methodologies or frameworks used by Service Provider

---

## 7. Changes to Scope

### 7.1 Change Order Process

If Client requests work outside the original scope:

1. Service Provider will document the requested change
2. Service Provider will provide a written estimate of impact on timeline and cost
3. Client must approve the change in writing
4. The contract will be amended via change order
5. Work proceeds once signed amendment is received

### 7.2 Out-of-Scope Work

Without a signed change order, Service Provider is not obligated to complete out-of-scope work.

---

## 8. Termination

**[INSERT block from templates/blocks/termination.md]**

---

## 9. Assumptions

This agreement assumes:
- {Assumption 1, e.g., Client has WordPress hosting}
- {Assumption 2, e.g., Assets will be provided by specific dates}
- {Assumption 3, e.g., One point of contact for decisions}

If any assumptions change, the timeline or investment may be adjusted via change order.

---

## 10. General Terms

### 10.1 Independent Contractor

Service Provider is an independent contractor, not an employee or partner of Client.

### 10.2 Governing Law

This agreement is governed by the laws of {YOUR_STATE/JURISDICTION}.

### 10.3 Entire Agreement

This agreement constitutes the entire agreement between the parties and supersedes all prior agreements or understandings.

### 10.4 Amendments

Amendments to this agreement must be in writing and signed by both parties.

---

## 11. Signatures

**Service Provider:**

{YOUR_NAME}

Signature: _____________________________ Date: _____________

**Client:**

{CLIENT_NAME}

Authorized Signature: _____________________________ Date: _____________

Print Name: _____________________________

Title: _____________________________

---

## Appendix A: Detailed Scope

[Optional: Include detailed technical specs if applicable]

## Appendix B: Reference Materials

[Optional: Past work examples, case studies, etc.]

---

## Notes

- **Use reusable blocks:** Sections 2.2, 3, 4, 5, and 8 reference block files. Insert the full text from those files to complete this template.
- **Replace all {PLACEHOLDERS}:** Every placeholder must be filled in.
- **Keep it simple:** Avoid legal jargon when plain English works.
- **Review with reviewer agent:** The `/new-contract` skill will check this for consistency and completeness.
- **Both parties sign:** Don't send until you've reviewed and both parties agree.

---

**Standard Blocks Location:**
- `templates/blocks/payment-terms.md`
- `templates/blocks/ip-assignment.md`
- `templates/blocks/warranty.md`
- `templates/blocks/liability.md`
- `templates/blocks/termination.md`
