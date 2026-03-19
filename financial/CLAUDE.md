# Financial – Business Finance Operations

This workspace tracks business financials, invoicing, payment tracking, and financial health. It's read by the `/retrospective` skill to review revenue patterns and cash flow.

## Structure

```
financial/
  financial-ops.md      Invoice tracking, payment status, recurring expenses
  accounts.md           Account summary (accounts receivable, payable, cash position)
  pricing.md            Current rates, pricing history, lessons learned
  tax-prep.md           Tax documents, deductions, preparation checklist
```

## How It Works

**Weekly Review Integration:**

The `/retrospective` skill reads `financial-ops.md` to:
- Check invoice status (sent, paid, overdue)
- Identify payment gaps or payment patterns
- Review cash flow (what's coming in, when)
- Spot recurring expenses that need attention

**Monthly Accounting:**

At month-end, update:
- Invoice status (payment_received date when it clears)
- Running totals for the month
- Any invoices marked overdue (follow-up trigger)

**Quarterly Financial Review:**

Review `accounts.md`:
- Are we tracking toward annual revenue goals?
- What's outstanding (accounts receivable)?
- Are recurring expenses as expected?
- Do we need to adjust pricing?

## Key Concepts

### Invoice Lifecycle

Each invoice has a status path:

```
draft → sent → paid → recorded
```

- **Draft:** Not yet sent to client
- **Sent:** Invoice delivered, waiting for payment
- **Paid:** Payment received in bank account
- **Recorded:** Entered into accounting system (if using external service)

### Payment Status Tracking

For each invoice, track:
- Invoice ID (e.g., INV-2026-001)
- Client name
- Amount
- Date sent
- Due date
- Date paid
- Payment method (ACH, check, credit card)
- Notes (late payment, early payment, payment arrangement)

### Recurring Expenses

Monthly fixed costs that need budgeting:
- Software subscriptions (Notion, GitHub, hosting, etc.)
- Insurance (E&O, general liability)
- Accounting/bookkeeping services
- Phone, internet
- Workspace costs
- Professional development

Track as:
- Expense name
- Monthly amount
- Due date
- Payment method
- Renewal date (for annual services)

### Cash Flow

Simple rule: Revenue minus recurring expenses = cash available.

Example:
```
March revenue:       $15,000 (2 invoices paid)
Recurring expenses:   $1,500
Available:           $13,500 (before taxes)
```

Taxes typically 25-30% of revenue (varies by location). Set aside accordingly.

## Two-Tier Memory

**Working Memory (this workspace):**
- Current invoices and payment status
- This month's cash position
- Outstanding payment issues
- Recurring expense calendar

**Deep Storage (business/memory/pricing-history.md):**
- Historical pricing by project type
- Payment patterns (who pays on time, who's slow)
- Seasonal revenue patterns
- Lessons learned from pricing

## Integration Points

- **Retrospective:** Reviews financial-ops.md, asks: "What's the cash position? Any invoices overdue?"
- **Standup:** Flags if awaiting critical payment to affect cash flow decisions
- **New-Project:** Uses pricing.md to set project rates
- **Follow-up:** Detects overdue invoices, drafts payment reminder

## Philosophy

Financial clarity reduces stress. You don't need fancy accounting software—just honest tracking.

When you know:
- What's owed to you (and when it's due)
- What you owe (and when it's due)
- What's actually in the bank

...you can make better business decisions.

This workspace makes that visible.

---

## Related Files

- `~/business/memory/pricing-history.md` – Historical rates, what worked, what didn't
- `~/personal/plan.md` – Personal financial goals (how much to earn, savings rate)
- `~/ops/projects/*/` – Individual project invoicing tied to milestones
