---
name: gvx-client-onboarding
description: "When the user is taking on a new GVX client or setting up marketing work for an existing client. Use when the user says 'new client,' 'onboard a client,' 'set up marketing for [client],' 'client workspace,' 'client context,' 'add this client to the hub,' or 'start work for [business].' Creates the client's product-marketing context, a client file with scope and boundaries, a registry entry for the GVX Hub, and a first-two-weeks plan that picks the right marketing skills in order."
metadata:
  version: 1.0.0
---

# GVX Client Onboarding

Gets a client set up so every other marketing skill works for *that client* and never mixes them with GVX or other clients.

## Principle

Context is per business. Each client project folder has its own `.agents/` directory. Skills read the context in the folder you are working in. Work for client A and client B happens in separate folders.

## Workflow

### 1. Intake (ask only what is missing)

- Client name, short code (2-5 letters, e.g. PBC), and live site URL
- What they sell, to whom, in which region
- Why they hired GVX and what success looks like in 90 days
- What GVX is engaged for: site, SEO, ads, email, content, other
- Where the project lives: repo URL and local folder
- Who approves work on the client side (role only)

If a site or repo exists, read it first and draft answers from it before asking.

### 2. Create the client context

Run the `product-marketing` skill inside the client's project folder. This creates `.agents/product-marketing.md` (positioning, audience, ICP, voice). Mark every guess as unconfirmed and have the user correct it.

### 3. Create the client file

Create `.agents/gvx-client.md`:

```
# Client: <Name> (<CODE>)
Engagement: <services>, started <date>
Billing: flat rate for labor (no metered usage billed to the client)
Live site: <url>   Repo: <url>   Local folder: <path>
Success in 90 days: <one or two measurable outcomes>
Approver (role): <e.g. President>
Channels: <email thread, shared folder, none>

## Boundaries
- No credentials, API keys, or passwords in this repo. Record where access is held, never the secret.
- Paid actions (DataForSEO, ads, etc.) show their cost before running. Log spend per client.
- Nothing is published, sent, or emailed on the client's behalf without the approver's okay.

## Decisions log
- <date>: <decision and why>
```

### 4. Register in the GVX Hub

Add an entry to `clients` in the hub's `public/data/registry.json` (id, name, short, blurb, links, folders, check) and run `npm run scan`. Also add the live URL to `status.sites` so uptime is checked.

### 5. First two weeks

Run in this order, saving each result in the client folder:

| Order | Skill | Output |
|-------|-------|--------|
| 1 | product-marketing | Confirmed context |
| 2 | analytics | Tracking audit: what is measured, what is missing |
| 3 | seo-audit | Technical and on-page findings, ranked |
| 4 | competitors | Who the buyer compares them to |
| 5 | copywriting / cro | Fixes for the pages that matter most |
| 6 | gvx-launch-video | A short launch video when something ships |

Present findings as a short list ranked by impact and effort. Ask the approver to pick, then do the work.

## Related Skills

- **product-marketing**: builds the context file
- **seo-audit**, **analytics**, **competitors**: the first audits
- **gvx-brand-voice**: only for GVX's own copy, never client copy
