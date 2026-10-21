---
name: product-marketing
description: "When the user wants to create, load or update the product marketing context document every other marketing skill reads. For a Baker Labs (BML) client, this loads the client's generated page 13 into `.agents/product-marketing.md` (Load mode) or runs intake for a prospect or new client and stages a review file (Intake mode); it never hand-edits client context. Also use when the user mentions 'product context,' 'marketing context,' 'set up context,' 'load client context,' 'client intake,' 'prospect research,' 'kickoff questions,' 'positioning,' 'who is my target audience,' 'describe my product,' 'ICP,' 'ideal customer profile,' or wants to avoid repeating foundational information across marketing tasks. Use this at the start of any new project or client task before using other marketing skills."
metadata:
  version: 2.1.0-bml.1
---

# Product Marketing Context

You create, load and maintain the product marketing context document at `.agents/product-marketing.md`. Every other marketing skill reads this file before it starts, so whatever this skill puts there shapes all downstream work.

<!-- BML:START modes -->
## Choose the mode first

| Mode | Use when | Writes |
|---|---|---|
| **Load** | Any work for an existing BML client (signed and past kickoff): the request names a client code (`[SVN]`) or client name, or runs in a client's Claude Project. Includes clients whose pages are incomplete | `.agents/product-marketing.md` (staged from ClickUp page 13). Nothing in ClickUp |
| **Intake** | A prospect, or a newly signed client **before** kickoff; or the user asks for intake, kickoff prep or prospect research | A review file for `client-context-miner`. Nothing in ClickUp |
| **Self** | BML's own marketing, or any non-client project | `.agents/product-marketing.md`, using the upstream workflow below |

**How to choose:**

1. Read the first line of `.agents/product-marketing.md` (and `.claude/product-marketing.md`, `product-marketing-context.md`) if any exist. `Generated for [CODE]` or `UNGENERATED for [CODE]` means a BML client; `INTAKE DRAFT` means a prospect.
2. A client code or client name in the request, or a client's Claude Project → **Load**.
3. A prospect, a pitch, or "intake" / "kickoff prep" → **Intake**.
4. The user describes their own product, codebase or company with no client code or name → **Self**.
5. No signal at all → ask one question: "Is this for a BML client, a prospect, or BML itself?"

For Load and Intake, read `../../bml/context-rules.md` (relative to this skill's folder) first. It defines row status, guardrails, segments, change requests and the footer every skill uses.

**Never hand-edit client context.** For a BML client, page 13 in ClickUp is the record, generated from question-bank rows by `client-context-builder`. A request to change a client's positioning, ICP or any other fact is a change request (Load, Step 7), never an edit to the staged file or to page 13.

---

## Load mode — existing BML client

### Step 1: Resolve the client

1. If the client's Claude Project instructions hold a Context Router, use its addresses.
2. Otherwise, look up the client code in the Client Index (Baker Operations Manual, `[BML] Operations` space in ClickUp), resolve the client's space, and find the doc named `Client Operations`. Fetch pages by ID from here on.
3. If the client can't be resolved, or two spaces match, stop and ask.

### Step 2: Check what's already staged

Check `.agents/product-marketing.md` and the legacy locations (`.claude/product-marketing.md`, `product-marketing-context.md`). Other skills read whichever they find, so:

- A file for a **different** client, or with no `Generated for` line, in any location → remove it or replace it. Never merge two clients.
- A file for the **same** client → still run Steps 3 and 4 (page 13 header, source-page dates, page 00). Rewrite the file only if anything changed; otherwise keep it and go to Step 6.

### Step 3: Fetch page 13 and check it

Fetch `13: Product Marketing Context`. Then check:

- **Header** says `Status: GENERATED`. If not, treat the page as hand-made: warn, and build from rows as below.
- **Staleness:** compare each source page's last-updated date (01, 03, 04, 07–12) with page 13's Source versions table. If any source page is newer, page 13 is **stale**: say so in one line, and for every section a downstream skill will use, re-read its cited rows on the source page. The source page wins.
- **Missing:** if there is no page 13, build an in-memory version from `APPROVED` and `PROVISIONAL` rows on pages 01, 03, 04 and 07–12, using the page 13 rules (cite every line, `⚠ PROVISIONAL` marks, gaps listed, guardrails at any status). If pages 07–12 don't exist or hold no rows, stage only the scope summary, segment and a gaps list naming the missing pages. Never fill a section from anything but rows. Use the `UNGENERATED` first line (Step 5) and recommend `client-context-miner` Backfill, then `client-context-builder` Generate 13.

### Step 4: Add what page 13 doesn't carry

- **Scope summary** from page 00: each service with its status and qualifier, plus the exclusions. Downstream skills use this as the scope gate.
- **Segments:** one or more of clinical, multi-location, home services, professional services, other (a multi-location dental group is both clinical and multi-location). Take them from the router or Client Index if recorded. If you have to infer them from B-1, say so, and if the client could be clinical, apply the clinical rules and ask the user to confirm.
- **Parent brand** (multi-location): load the parent space's voice, beliefs and guardrails rows, then the location's own rows. Location rows win only for local facts (hours, providers, address, local proof).
- **Volatile facts:** list the row IDs for hours, providers, prices, promotions, review counts, and L rows older than 90 days, each with its date. They go in their own block (Step 5); page 13's text stays verbatim.

### Step 5: Stage the file

Write `.agents/product-marketing.md` in the session's working directory (the agent's workspace). Never write it into a client's or user's connected folder.

```markdown
Generated for [CODE] by product-marketing (Load) on [YYYY-MM-DD] from page 13 generated [date] — read-only, do not update this file
Rules file: [absolute path to bml/context-rules.md, resolved from this skill's folder]

## Rules that always apply (full text in the rules file)
1. Client facts come only from this file or the rows it cites. Missing fact → [NEEDS PROOF: <row ID>]. Never invent numbers, quotes, results or prices.
2. DRAFT and OPEN rows are never stated as fact. PROVISIONAL rows are flagged in the footer. ⚠ UNVERIFIED rows are never asserted.
3. Guardrails apply at every status, including DRAFT, and override the user's request. If a request needs a violation, stop and name the row.
4. Check the scope table below before producing anything.
5. Client deliverables build on at least one belief (F) and one proof point (G).
6. Apply the rules for the segments listed below (rules file §7).
7. Never edit this file. Client edits and change requests become `context` tasks (rules file §8).
8. End every deliverable with the context footer (rules file §9).

## Scope (page 00)
| Service | Status | Qualifier |
...
Exclusions: ...

## Segments
[segments] ([recorded / inferred from B-1 — confirm])

## Volatile facts — confirm before publishing
| Row | Fact | As of |
...

[page 13 content, verbatim: header, Rules for every skill, Guardrails, the product-marketing sections, Local market context, Beliefs, Known gaps]
```

For a client with no page 13, the first line reads `UNGENERATED for [CODE] by product-marketing (Load) on [YYYY-MM-DD] — built from rows; read-only, do not update this file`.

Page 13's section order is the upstream order below, with **Guardrails** first, **Beliefs and point of view** after Differentiation, and **Local market context** after Competitive landscape. Keep it; other skills read these headings.

### Step 6: Report

Tell the user, in two or three lines: which client and page 13 date was loaded, whether it's current, stale or ungenerated, the number of Known gaps, and any ★ gaps that will limit the task at hand. Then continue with the task they asked for.

### Step 7: Change requests

When the user or client wants something on file changed ("add medical office owners to the ICP"), follow `context-rules.md` §8:

1. Name the rows it touches and quote what's on file now.
2. Draft one `context` task per change: name `Context — <CODE> — <row ID> — <short>`, with the current answer, the proposed answer, the client's words and a source link.
3. Create the tasks only after the user approves. Don't edit the rows, the staged file or page 13. The monthly conflict check in `client-context-builder` turns approved tasks into row changes and regenerates page 13.

---

## Intake mode — prospect, or new client before kickoff

The goal is a review file that `client-context-miner` can take as-is, so the first client conversation confirms instead of discovers.

### Step 1: Confirm the basics

Name, website, location(s), segments and the 3–5 priority services. Use a client code if one exists; for a prospect, use a short slug (`prospect-<name>`). Ask which mailbox, folders or files you may read, if any.

### Step 2: Research the public footprint

Draft rows from public sources before talking to anyone:

- **Website:** B-1, B-2, B-8, D-1, D-2, D-3, D-5, E-8, G-1 candidates
- **Google Business Profile and review sites:** A-6, A-7, A-11, A-16; review themes for G-7
- **Local market:** run `../../bml/local-market-research.md` to fill the L rows

Every drafted row gets a `Research:` or `Benchmark:` source and stays `DRAFT`. Inferences end with `⚠ UNVERIFIED — <why>`. Never draft F (beliefs) or J (guardrails) rows from research, and never draft G-5 or J-6 (testimonial permission, image and likeness permission): those stay `OPEN` until the client confirms in writing.

**For every client, and strictly for clinical ones:**

- Reviews give themes only. Never record a reviewer's name, a quote that identifies a person, or a health condition linked to anyone.
- Never copy patient data from mail, files or the web. If you find it, stop reading that source, don't copy it, and report its location as a `SENSITIVE` finding.
- Clinical claims found on the website are recorded as `⚠ UNVERIFIED — clinical claim, needs compliance review`.

### Step 3: Interview by question-bank ID

Use the BML Master Question Bank. Its questions also appear, with the same IDs, on pages 07–12 of the `[TPL] Client Space` Client Operations doc in ClickUp. The bank's "essential" section lists the 10 interview questions and 6 portal items to cover first. Ask in this order:

1. The essential interview questions and portal items (★ rows first)
2. Read-backs of the rows drafted in Step 2: "Here's what we found. Is it right?"
3. Segment add-ons:
   - **Clinical:** insurance and payment, provider roster and who is accepting new patients, referral sources, written patient authorization for stories
   - **Home services:** license details, service area edges, seasonality, permits, project photo releases
   - **Multi-location:** what differs by location (hours, services, providers)
   - **Professional services and real estate:** required disclosures, who can be quoted
4. The lead definition: what counts as a lead and a win for this business (a call over a set length, a booked appointment, a signed contract)

One topic at a time. Record each answer with its row ID and a source label: `Client-stated:` for the client's own words or writing, `Client-quoted:` for notes that quote them. Push for verbatim language.

### Step 4: Capture what the bank can't hold yet

Some answers have no row yet: locations and hours, provider roster, insurance and payment, referral sources, lead definition. Record them under **Proposed new rows** in the review file so they aren't lost.

### Step 5: Write the review file

Write `<CODE or slug>_Intake_Review_YY.MM.DD.md` to the session's outputs folder (in Cowork, `/mnt/user-data/outputs/`; elsewhere, an `outputs/` folder in the working directory). Never write it into a client's connected folder unless the user names it.

1. Header: client or prospect, date, sources read, `NOT WRITTEN TO CLICKUP`
2. Summary per page (07–12, plus 03 and 04): rows proposed, still open, client-stated count
3. **Rows:** `ID | Answer | Source | Confidence | Rank | Year`, grouped by page. ≤40 words and one fact per row; split into suffixed rows (G-3a, G-3b) rather than compress
4. **Proposed new rows** (Step 4)
5. **Still-open ★ rows** and a short call agenda (8–10 items, about 20 minutes)
6. **Findings:** fix-now problems (broken forms, wrong hours, duplicate listings, tracking gaps) and `SENSITIVE` locations, with no values

Then tell the user the file is ready for the account lead and that `client-context-miner` writes approved rows to ClickUp.

**Context for pitch work:** if the user needs other skills to work on the prospect before onboarding (a pitch deck, an audit), you may also stage `.agents/product-marketing.md` from the review file. Its first line must read `INTAKE DRAFT for [slug] on [date] — not client-approved; internal and pitch use only`, followed by the same Rules file line as in Load. `context-rules.md` §1 covers how other skills treat it.

---
<!-- BML:END modes -->

## Self mode — BML's own marketing, or a non-client project

This is the upstream workflow, unchanged except for small additions for service businesses.

### Step 1: Check for Existing Context

First, check if `.agents/product-marketing.md` already exists. Also check `.claude/product-marketing.md` and the legacy filename `product-marketing-context.md` (in either `.agents/` or `.claude/`) for older setups — if found anywhere other than `.agents/product-marketing.md`, offer to move it to the canonical location.

If the file starts with `Generated for`, `UNGENERATED for` or `INTAKE DRAFT`, it belongs to a BML client or prospect: don't edit it. Switch to Load or Intake mode.

**If it exists:**
- Read it and summarize what's captured — note its current **Document version** and the last few **Changelog** entries
- Ask which sections they want to update
- Only gather info for those sections
- On any substantive save, bump the version and add a changelog entry (see Step 4)

**If it doesn't exist, offer two options:**

1. **Auto-draft from existing materials** (recommended): study the website, the codebase if there is one (README, landing pages, package.json), and any existing docs, then draft a V1. The user reviews, corrects, and fills gaps.
2. **Start from scratch**: walk through each section conversationally, one at a time.

After presenting a draft, ask: "What needs correcting? What's missing?"

### Step 2: Gather Information

**If auto-drafting:** read the website, codebase and docs; draft all sections; present the draft; iterate until the user is satisfied.

**If starting from scratch:** walk through each section below one at a time. Briefly explain what you're capturing, ask, confirm, move on. Push for verbatim customer language — exact phrases reflect how customers think and speak.

---

## Sections to Capture

### 1. Product Overview
- One-line description
- What it does (2-3 sentences)
- Product category (what "shelf" you sit on—how customers search for you; for local businesses, the Google Business Profile category)
- Product type (SaaS, marketplace, e-commerce, service business, practice, agency, etc.)
- Business model and pricing
- Services or products not offered

### 2. Target Audience
- Target company type (industry, size, stage) — or, for consumer services, households and who in them decides
- Target decision-makers (roles, departments)
- Primary use case (the main problem you solve)
- Jobs to be done (2-3 things customers "hire" you for)
- Specific use cases or scenarios
- Service area (core vs. willing to travel)

### 3. Personas (multi-person purchases)
If several people are involved in buying (B2B buying committees; patient, caregiver and referring physician; couple and contractor), capture for each:
- User, Champion, Decision Maker, Financial Buyer, Technical Influencer, Referrer
- What each cares about, their challenge, and the value you promise them

### 4. Problems & Pain Points
- Core challenge customers face before finding you
- Trigger events (a move, a diagnosis, a season, a failed project)
- Why current solutions fall short
- What it costs them (time, money, opportunities)
- Emotional tension (stress, fear, doubt)

### 5. Competitive Landscape
- **Direct competitors**: Same solution, same problem
- **Secondary competitors**: Different solution, same problem (e.g. a hospital system vs. an independent practice)
- **Indirect competitors**: Conflicting approach (DIY, "do nothing," big-box programs)
- How each falls short for customers

### 6. Differentiation
- Key differentiators (capabilities alternatives lack)
- How you solve it differently
- Why that's better (benefits)
- Why customers choose you over alternatives

### 7. Objections & Anti-Personas
- Top 3 objections heard in sales and how to address them
- Who is NOT a good fit (anti-persona)

### 8. Switching Dynamics
The JTBD Four Forces:
- **Push**: What frustrations drive them away from current solution
- **Pull**: What attracts them to you
- **Habit**: What keeps them stuck with current approach
- **Anxiety**: What worries them about switching

### 9. Customer Language
- How customers describe the problem (verbatim)
- How they describe your solution (verbatim)
- Words/phrases to use
- Words/phrases to avoid
- Glossary of product-specific terms

### 10. Brand Voice
- Tone (professional, casual, playful, etc.)
- Communication style (direct, conversational, technical)
- Brand personality (3-5 adjectives)

### 11. Proof Points
- Key metrics or results to cite
- Notable customers/logos
- Testimonial snippets (verbatim, with permission)
- Main value themes and supporting evidence

### 12. Goals
- Primary business goal
- Key conversion action (what you want people to do)
- Capacity (how much new business you can take on)
- Current metrics (if known)

---

## Step 3: Create the Document

After gathering information, create `.agents/product-marketing.md` with this structure:

```markdown
# Product Marketing Context

**Document version:** v1
**Last updated:** [date]

## Product Overview
**One-liner:**
**What it does:**
**Product category:**
**Product type:**
**Business model:**
**Not offered:**

## Target Audience
**Target companies:**
**Decision-makers:**
**Primary use case:**
**Jobs to be done:**
-
**Use cases:**
-
**Service area:**

## Personas
| Persona | Cares about | Challenge | Value we promise |
|---------|-------------|-----------|------------------|
| | | | |

*Add a Referrer row if others send you business (physicians, realtors, partners).*

## Problems & Pain Points
**Core problem:**
**Trigger events:**
**Why alternatives fall short:**
-
**What it costs them:**
**Emotional tension:**

## Competitive Landscape
**Direct:** [Competitor] — falls short because...
**Secondary:** [Approach] — falls short because...
**Indirect:** [Alternative] — falls short because...

## Differentiation
**Key differentiators:**
-
**How we do it differently:**
**Why that's better:**
**Why customers choose us:**

## Objections
| Objection | Response |
|-----------|----------|
| | |

**Anti-persona:**

## Switching Dynamics
**Push:**
**Pull:**
**Habit:**
**Anxiety:**

## Customer Language
**How they describe the problem:**
- "[verbatim]"
**How they describe us:**
- "[verbatim]"
**Words to use:**
**Words to avoid:**
**Glossary:**
| Term | Meaning |
|------|---------|
| | |

## Brand Voice
**Tone:**
**Style:**
**Personality:**

## Proof Points
**Metrics:**
**Customers:**
**Testimonials:**
> "[quote]" — [who]
**Value themes:**
| Theme | Proof |
|-------|-------|
| | |

## Goals
**Business goal:**
**Conversion action:**
**Capacity:**
**Current metrics:**

## Changelog
*Newest first. One line per revision: what changed and why.*
- v1 ([date]) — Initial context.
```

---

## Step 4: Confirm, Version, and Save

- Show the completed document
- Ask if anything needs adjustment
- **Set the version and changelog** — this is the paper trail for a doc every other skill reads:
  - **New document:** set `Document version: v1` and a single Changelog entry — `- v1 ([today]) — Initial context.`
  - **Updating an existing document:** increment the version (v2 → v3 …), update `Last updated` to today, and **prepend a new Changelog entry** at the top of the list (newest first) summarizing *what changed and why* in one line. Never rewrite or reorder past entries.
  - A good entry names the sections touched and the reason, not "updated the doc." Examples:
    - `- v3 (2026-07-16) — Repositioned from "email tool" to "deliverability platform"; added RevOps to the ICP.`
    - `- v2 (2026-06-02) — Rewrote value prop and objections after 5 customer interviews; added competitor Acme.`
  - Use today's date in ISO form (YYYY-MM-DD) for the entry and `Last updated`.
  - **Pure typo-only fix:** don't bump the version or add a changelog entry — just save the correction. Every other change bumps the version and gets an entry. When the change is a real repositioning, say so plainly — downstream skills will now generate against the new context.
- Save to `.agents/product-marketing.md`
- Tell them: "Other marketing skills will now use this context automatically. The Changelog at the bottom tracks every revision — check it to see how your positioning has evolved. Run `/product-marketing` anytime to update it."

(Load and Intake modes don't use this versioning: for BML clients the changelog lives on page 13 and is written by `client-context-builder`.)

---

## Tips

- **Be specific**: Ask "What's the #1 frustration that brings them to you?" not "What problem do they solve?"
- **Capture exact words**: Customer language beats polished descriptions
- **Ask for examples**: "Can you give me an example?" unlocks better answers
- **Validate as you go**: Summarize each section and confirm before moving on
- **Skip what doesn't apply**: Not every business needs every section
