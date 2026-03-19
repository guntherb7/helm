---
name: new-proposal
type: skill
description: Guided proposal generation. Asks discovery questions, references past pricing, generates document in your voice using writer agent.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
disable-model-invocation: false
---

# New Proposal Skill

Guided conversation to gather proposal details, then generate a professional document in your voice.

## Phase 1: Discovery Questions

Have a conversation. Ask these questions naturally, not as a form:

**Client & Context**
- "Who are we talking to?"
- "What does their company do?"
- "Who's the decision maker?"

**The Problem**
- "What problem are they trying to solve?"
- "Why does this matter to them (revenue, efficiency, user experience)?"
- "What have they tried already?"

**Scope & Deliverables**
- "What does success look like for them?"
- "Do they have a platform preference (WordPress, custom, etc.)?"
- "What's in scope, and what's explicitly out?"

**Timeline & Budget**
- "When do they want this done?"
- "Do they have a budget in mind?"
- "Is this a one-time project or ongoing relationship?"

**Constraints**
- "Are there any technical constraints (legacy systems, integrations)?"
- "Does they have compliance needs (security, accessibility)?"
- "Who do we need to work with on their end?"

**Version Control**
- "What's the simplest version that solves their core problem?"
- "What's the full buildout (if we had unlimited budget)?"

## Phase 2: Price Calibration

Look up comparable projects:

```bash
# Check your pricing history
cat ~/business/memory/pricing-history.md

# Check your pricing reference
cat ~/ops/templates/pricing-reference.md
```

Consider these factors:

1. **Complexity factor:** Is this familiar territory or new tech?
   - Familiar = base rate
   - New tech/integrations = +20%
   - Highly custom/uncertain = +30%

2. **Relationship factor:**
   - First-time client = -10% (build goodwill)
   - Existing client = same rate or -5% (loyalty)
   - High-risk client (slow payments, scope creep) = +15%

3. **Timeline factor:**
   - Standard timeline = base
   - Rushed (1-2 weeks) = +25%
   - Extended (3+ months) = -10% (better margins on longer work)

4. **Retainer premium:**
   - If ongoing: +15% over project work (stability premium)

5. **Milestone payment structure:**
   - Smaller upfront = better (cash flow)
   - Larger final milestone = riskier (collection)

**Pricing formula:**
- Base rate: Your effective hourly rate × estimated hours
- Apply complexity factor (×1.2 for +20%, etc.)
- Apply relationship & timeline factors
- Round to clean number ($8k, $12.5k, $15k)

## Phase 3: Generate Proposal

Call the writer agent to draft the proposal in your voice:

```
Invoke writer agent with:
- Client name
- Scope summary
- Deliverables
- Timeline
- Investment amount
- What we need from them
- Next steps

Agent reads:
- ~/ops/templates/voice-guide.md (your writing voice)
- ~/business/memory/clients.md (client context)
- Generated proposal should match your past writing style
```

## Phase 4: Structure

The proposal should follow this structure (from `~/ops/templates/proposal-structure.md`):

```markdown
# Proposal: [Client Name] — [Project Name]

## Executive Summary
[1-2 paragraphs explaining what you'll do and why it matters]

## Scope of Work

### Phase 1: [Phase Name]
[What gets delivered, timeline, payment trigger]

### Phase 2: [Phase Name]
[What gets delivered, timeline, payment trigger]

### Phase 3: [Phase Name]
[What gets delivered, timeline, payment trigger]

## Investment
- Phase 1: $X
- Phase 2: $X
- Phase 3: $X
- **Total: $XXX**

## Timeline
[Overall timeline, key milestones, when you start, when client needs to be ready]

## What I Need From You
[Client responsibilities, decisions required, timeline for decisions, assets/access needed]

## Success Criteria
[How we'll know this worked]

## Next Steps
[What happens if they say yes, timeline for decision, how to ask questions]

## Assumptions
[What you're assuming about scope, timeline, access, or their environment]
```

## Phase 5: Save & Follow Up

Save the proposal:

```bash
mkdir -p ~/business/proposals/{client-name}
# Proposal saved to: ~/business/proposals/{client-name}/proposal-{project-name}-{date}.md
```

Create a project ops file with status: proposal

```bash
# Create ~/ops/projects/{project-name}.md with:
status: proposal
client: {client name}
proposal_sent: {today's date}
proposal_value: ${amount}
decision_deadline: {when do you need an answer}
```

## Phase 6: Voice Matching

The writer agent will:
1. Read `~/ops/templates/voice-guide.md` (your documented voice)
2. Read `~/business/memory/clients.md` (client context if existing)
3. Generate proposal in your style

**Quality checks:**
- Does it sound like you?
- Is it clear about what gets delivered?
- Does it have a clear payment structure?
- Is the timeline realistic?

## Tone Guidance

Your proposal should feel like:
- **Clear** (short sentences, no jargon unless they use it)
- **Professional but warm** (friendly, not corporate)
- **Confident** (you know what you're doing)
- **Collaborative** (we're doing this together)
- **Outcome-focused** (what they get, not what you're doing)

## Common Patterns

### Fixed Price vs Hourly
- **Fixed price:** Better for well-scoped projects, easier to close
- **Hourly:** Better for undefined scope, limits your risk
- **Hybrid:** Fixed + hourly overage (recommended for uncertain projects)

### Milestone Structure
- **Standard:** 3 equal milestones (33% up front, 33% mid, 34% final)
- **Front-loaded:** 50% up front, 50% final (reduces your risk)
- **Back-loaded:** 25% up front, 75% final (higher risk, more trust needed)

### Payment Terms
- **Due upon delivery:** Best case
- **Net 30:** Industry standard
- **Net 45:** You need good cash flow management
- **Net 60+:** Add 5-10% to price (time cost of money)

## After They Respond

If they ask for changes:
- **Scope reduction:** Quote goes down
- **Scope addition:** Create change order in writing
- **Timeline acceleration:** Add rush fee (25%)
- **Timeline extension:** You can reduce slightly (lower urgency)

If they say yes:
- Run `/new-contract` to formalize the agreement
- Create project repo
- Scaffold project structure

If they ask for lower price:
- **Know your number:** Don't negotiate below your effective hourly rate
- **Value justification:** Can you show why this is worth the price? (faster, better quality, unique approach)
- **Scope reduction:** "If we focus on Phase 1 and Phase 2, we can do it for $X"
- **Extended timeline:** "If you can wait 8 weeks, we could do this for $X"

## Next Skill

When proposal is accepted → `/new-contract`

---

**Skill flow:**
1. `/new-proposal` – Generate proposal
2. Client says yes
3. `/new-contract` – Formalize agreement
4. `/new-project` – Start the work

**Reads:**
- ~/business/memory/pricing-history.md (past projects for calibration)
- ~/ops/templates/pricing-reference.md (pricing guidelines)
- ~/ops/templates/voice-guide.md (your voice)
- ~/business/memory/clients.md (client context)

**Writes:**
- ~/business/proposals/{client}/ (proposal document)
- ~/ops/projects/{project}.md (ops file with proposal status)

**Calls:** writer agent (to draft proposal)
