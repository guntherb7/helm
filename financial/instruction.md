# Financial Setup

Set up your business financial tracking system. This is foundational—good financial visibility reduces stress and improves decision-making.

## Phase 1: Initialize Accounts (15 min)

Create `accounts.md` with a snapshot of your current financial position:

```markdown
# Accounts

## Accounts Receivable (Money Owed To You)

| Invoice | Client | Amount | Date Sent | Due Date | Status | Notes |
|---------|--------|--------|-----------|----------|--------|-------|
| INV-2026-001 | Acme Corp | $5,000 | 2026-03-20 | 2026-04-19 | Paid | Phase 1 deposit |
| INV-2026-002 | Acme Corp | $5,000 | 2026-04-20 | 2026-05-20 | Sent | Phase 2 payment |

**Total Outstanding:** $5,000
**Total Paid This Year:** $5,000

## Accounts Payable (Money You Owe)

| Vendor | Service | Monthly | Annual | Renewal | Status |
|--------|---------|---------|--------|---------|--------|
| GitHub | Hosting | $15 | $180 | March | Active |
| Notion | Workspace | $10 | $120 | Monthly | Active |

**Total Monthly:** $25
**Total Annual:** $300

## Cash Position

- Bank balance: [Your current amount]
- Outstanding receivables: $5,000
- Outstanding payables: $25/month
- Available to allocate: [Calculation]

**Notes:**
- Bank account: [Name] at [Bank]
- Tax savings set-aside: [Amount] (25-30% of gross revenue)
```

**What to include:**

- List all invoices sent (with status)
- List all money you're owed
- List recurring expenses
- Current bank balance (or range if you prefer privacy)
- Tax reserve amount (typically 25-30% of revenue)

**Note:** This is private. No client names need to be exact—"Client A", "Project X" is fine for privacy.

## Phase 2: Set Up Invoice Tracking (10 min)

Create `financial-ops.md` with your invoice and expense calendar:

```markdown
# Financial Operations

## Invoicing Calendar

### Recurring Invoices
- Retainer clients: [Any monthly/recurring contracts]
- Milestone-based: [Projects with milestone billing]

**Invoice timing:**
- Send within 2 days of milestone completion
- Due date: Net 30 (or per contract)
- Follow-up: Day 35 if not paid

## Current Invoices (Q1 2026)

### Outstanding
- INV-2026-002: Acme Corp – $5,000 – Sent 2026-04-20 – Due 2026-05-20

### Paid This Month
- INV-2026-001: Acme Corp – $5,000 – Paid 2026-03-20

## Recurring Expenses

### Monthly
- GitHub: $15 (due 15th)
- Notion: $10 (due 1st)
- **Total: $25**

### Quarterly
- [Any quarterly expenses]

### Annual
- Insurance: $800 (due Jan 1)
- Accounting: [Amount] (due April 15)
- **Total: $[amount]**

## Tax Tracking

**Revenue to date (2026):**
- Jan: $15,000
- Feb: $0
- Mar: $5,000
- **Total: $20,000**

**Tax reserve (30%):** $6,000 (set aside, don't spend)

**Monthly average:** $6,667 (annualized: $80,000)

## Payment Issues

None currently.

## Notes

- Acme Corp pays on time (within 30 days)
- Track Net 30 invoices sent; follow up at day 35 if no payment
```

**What to include:**

- Current invoices (sent, outstanding, paid)
- Invoice amounts and dates
- Recurring monthly expenses
- Any overdue/late payments
- Tax reserve calculation

## Phase 3: Set Up Pricing Reference (10 min)

Create `pricing.md` with your current rates and history:

```markdown
# Pricing & Rates

## Current Rates (2026)

### Project-Based
- Website projects: $5,000 - $15,000 (depending on scope)
- Custom integrations: $150/hr or $5,000 fixed (per scope)
- Maintenance/retainer: $2,000/month (minimum 3 months)

### Retainer Rates
- 10 hours/week: $2,000/month
- 15 hours/week: $2,800/month
- 20 hours/week: $3,500/month

## Rate Card Decisions

- 2026 rate increase: 15% from 2025 (reflecting more experience)
- Minimum project: $5,000 (anything smaller is hourly + overhead not worth it)
- Rush premium: +25% for expedited timeline

## Historical Pricing

| Year | Project Type | Rate | Notes |
|------|--------------|------|-------|
| 2025 | Website | $3,500-$8,000 | Lower end; building portfolio |
| 2026 | Website | $5,000-$15,000 | Increased value; more selective |

## Lessons Learned

- Website projects under $5k don't cover overhead; moved to minimum $5k
- Scope creep happens; build buffer into estimates
- Fixed vs. hourly: Fixed better for us (more predictable); clients prefer it
- 3-month minimum retainers eliminate ramp-up costs
```

**What to include:**

- Your current hourly rate (if hourly)
- Your project rates (if project-based)
- Rate card for different service types
- When you last increased rates
- How rates compare to market
- Lessons from past pricing decisions

## Phase 4: Set Up Tax Preparation (10 min)

Create `tax-prep.md` with your tax filing checklist:

```markdown
# Tax Preparation

## Timeline

- **January 1 - March 31:** Gather 2025 documents
- **April 1 - April 15:** Accountant reviews; file estimate or extension
- **May - June:** Pay any estimated tax or file final return

## Documents to Gather

- [ ] All 1099s from clients (by Jan 31)
- [ ] Bank statements (all accounts, all of 2025)
- [ ] Credit card statements (business expenses)
- [ ] Invoice records (all invoices issued)
- [ ] Payment records (all payments received)
- [ ] Expense receipts (office, software, training, equipment)
- [ ] Mileage log (if claiming home office deduction)
- [ ] Equipment purchases (for depreciation)

## Deductions to Track

- Software subscriptions (Notion, GitHub, etc.)
- Professional development (courses, books, conferences)
- Home office (utilities prorated, internet)
- Equipment (computer, monitor, office furniture)
- Professional services (accountant, lawyer)
- Phone/internet (prorated if business use)
- Travel (client meetings, conferences)

## Quarterly Estimated Tax

If self-employed:
- Calculate: (Projected annual revenue × 25%) ÷ 4 = quarterly payment
- Pay on: April 15, June 15, Sept 15, Jan 15
- Method: IRS Form 1040-ES

**Example:**
```
Projected 2026 revenue: $80,000
Tax rate: 25% = $20,000/year
Quarterly: $20,000 ÷ 4 = $5,000 per quarter
```

## Accountant Contact

- Name: [Your accountant]
- Email: [Email]
- Phone: [Phone]
- Filing deadline reminder: Set for March 1 each year

## Notes

- Keep all receipts (digital or paper) for 7 years
- Separate business and personal accounts (cleaner tax time)
- Track quarterly; don't leave it all for April
```

**What to include:**

- Tax filing deadline reminders
- Documents you need to gather
- Deductions you typically claim
- Quarterly estimated tax schedule
- Accountant contact info

## Phase 5: Connect to Retrospective (5 min)

Verify that `/retrospective` loads financial context:

```bash
cd ~/business
claude skill retrospective
```

In the conversation, it should ask: "How's cash flow this month? Any payment issues?"

If not, check that `~/financial/financial-ops.md` exists and is readable.

## Monthly Checklist

Every month (suggest: last Friday):

- [ ] Update invoice status in `financial-ops.md`
- [ ] Record any new invoices sent
- [ ] Update cash position in `accounts.md`
- [ ] Check for overdue invoices (older than 35 days)
- [ ] Verify recurring expenses were charged
- [ ] Update monthly revenue total in tax-prep.md

## Quarterly Checklist

Every 3 months:

- [ ] Review `accounts.md` – Are we on pace with revenue goals?
- [ ] Review `pricing.md` – Should we adjust rates?
- [ ] Estimate quarterly tax payment amount
- [ ] Review outstanding invoices – Any patterns? (slow payers, invoice issues)
- [ ] Check if expenses match projections

## Year-End Checklist

Before January 31:

- [ ] Gather all 1099s
- [ ] Reconcile all bank and credit card statements
- [ ] Calculate total revenue for the year
- [ ] Review deductions; create list for accountant
- [ ] Schedule accountant meeting for tax return

---

**Philosophy:**

Financial clarity doesn't require fancy accounting software. A spreadsheet and monthly attention gives you the visibility to make good decisions.

When you know your cash position, you can:
- Set rates confidently (you know what you need to charge)
- Spot payment problems early (overdue invoices)
- Plan for taxes (no surprise bills)
- Scale intentionally (you see what's working financially)

This workspace makes that visible without overhead.
