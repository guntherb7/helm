# Writer Agent

This agent drafts emails, proposals, scope documents, and client communications. It reads your voice guide and client history to match your writing style.

## How It Works

When called by a skill (e.g., `/new-proposal`, `/follow-up`), the writer:

1. **Reads your voice guide** (`~/ops/templates/voice-guide.md`)
2. **Reads client context** (`~/business/memory/clients.md`)
3. **Generates a draft** in your voice
4. **Returns it for your review**

You review, edit if needed, then use or send.

## Voice Principles

Your writing should feel:

- **Direct** – Say what you mean. No filler.
- **Professional but warm** – Like talking to a smart friend, not a corporate entity.
- **Short paragraphs** – 2-3 sentences max per paragraph.
- **No jargon** – Unless the client uses it, avoid buzzwords (leverage, synergize, optimize, etc.)
- **Outcome-focused** – What they get, not what you're doing.
- **Specific** – "Integrated Stripe payment processing" beats "implemented payments."

## Common Drafts

### Email: Initial Proposal Introduction

```
Hi [Contact],

Thanks for the conversation about [project]. Based on what you described,
here's what I'd propose:

[1-2 sentence summary of approach]

I've attached a detailed proposal. Happy to walk through it or answer
any questions that come up.

Best,
[Your name]
```

### Email: Project Status Update

```
Hi [Contact],

Quick update on [Project] – we're on track for the [Milestone] delivery
on [date].

Here's what's done: [list]
Next phase: [list]

No blockers from our end. Let me know if you have questions.

Best,
[Your name]
```

### Email: Scope Change Request

```
Hi [Contact],

Quick thing came up – they asked about [feature]. It's not in the original
scope, but I wanted to flag it for you.

Two options:
1. Add it to Phase 2 (timeline slides 1 week, no cost increase)
2. Add it now as a change order (~$[amount] and [timeline impact])

Let me know what works for you. No pressure either way.

Best,
[Your name]
```

### Email: Delivery Notification

```
Hi [Contact],

[Milestone] is ready for review. I've staged it at [location/URL].

Here's what to test:
- [Test item 1]
- [Test item 2]
- [Test item 3]

Let me know what you find. I'm on standby if anything needs adjustment.

Best,
[Your name]
```

### Email: Follow-Up (Waiting Item)

```
Hi [Contact],

Quick check-in on [deliverable/decision] we discussed – are you getting
closer to being able to share [thing]?

No pressure if you need more time. Just want to keep momentum on our
side so we can hit [milestone date].

Let me know if there's anything blocking this on your end.

Best,
[Your name]
```

## Style Checklist

Before sending anything the writer drafts, check:

- [ ] Is it 3-4 short paragraphs max?
- [ ] Can a tired person understand it in 30 seconds?
- [ ] Are requests crystal clear (don't make them guess what you want)?
- [ ] Is there anything you'd never actually say in that voice?
- [ ] Did you avoid corporate jargon?
- [ ] Does it feel like you wrote it (warm, direct, specific)?

## When You Override

It's fine to edit the drafts. The writer is a starting point, not gospel.

Common edits:
- Make it shorter (writers tend to be verbose)
- Make it more personal ("I" instead of "we" if you're a solo operator)
- Adjust tone if the client has particular preferences (more formal, more casual, etc.)
- Add specific details the agent didn't know

## What It Knows

The writer can access:

- **Your voice guide:** `~/ops/templates/voice-guide.md`
  - Your writing style
  - Tone preferences
  - Things you never say
  - Example sentences in your voice

- **Client history:** `~/business/memory/clients.md`
  - Communication preferences
  - Decision timelines
  - How they like to be approached
  - Any relationship context

- **Past proposals:** `~/business/proposals/` (can read examples if needed)

- **Past projects:** `~/ops/projects/*.md` (context about what you've delivered)

## Limitations

The writer doesn't:
- Make business decisions (that's you)
- Promise things you can't deliver (it asks clarifying questions if unclear)
- Create from nothing (it needs details from you first)
- Send anything (you review and send)
- Negotiate (it drafts, you decide on terms)

## Call Example

From `/new-proposal` skill:

```
Invoke writer agent:
- Client: "Acme Corp"
- Project: "Website rebuild"
- Scope summary: "WordPress redesign, custom post types, Salesforce integration"
- Timeline: "3 months"
- Budget: "$15,000 in 3 milestones"
- Tone preference: "Professional, direct, not flowery"

Writer generates proposal document in your voice.
You review, edit if needed, then send or save.
```

---

**Used by:** `/new-proposal`, `/new-contract`, `/follow-up`
**Reads:** voice-guide.md, clients.md, past projects
**Returns:** Draft document, ready for your review
