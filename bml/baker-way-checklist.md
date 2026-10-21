# The Baker Way: Skill Upgrade Checklist

The criteria every marketing skill in this fork is audited against before BML uses it for client work. `bml-skill-upgrade` scores each skill against these items; reviewers use the same IDs when approving or changing a recommendation.

Each finding gets one tag:

- **Technical** — a mechanical fix (a path, a tool, an output location, a missing pointer). Approved in bulk unless the reviewer objects.
- **Directional** — a change to what the skill does, who it serves, or how it decides. Needs an explicit decision.
- **Keep** — upstream behavior that already fits. Listed so the reviewer can see it was checked.

---

## Context

| ID | Check | Passes when |
|---|---|---|
| BW-1 | Context source | The skill reads `.agents/product-marketing.md` (staged page 13) first, uses it before asking questions, and never writes or "updates" that file for a client |
| BW-2 | Rules pointer | The skill's context-check line also points to `bml/context-rules.md` for BML client work |
| BW-3 | Scope gate | Before producing client work, the skill checks the service against page 00 (via the staged scope table) and respects qualifiers like "planning only" |
| BW-4 | Read order | Router → page 13 → Monthly Context Brief → source pages only if needed. No step tells the agent to browse Drive or search ClickUp broadly for context |
| BW-5 | One client per run | Nothing carries one client's facts, examples or files into another client's work |

## Claims and guardrails

| ID | Check | Passes when |
|---|---|---|
| BW-6 | Claims from rows | Client facts come only from page 13 or cited rows. Any "be specific", "add proof" or "use numbers" step is redirected to `[NEEDS PROOF: <row ID>]` when the record lacks the fact |
| BW-7 | No invented examples | Examples in the skill and its references use placeholders (`<n>`, `<service>`) rather than realistic figures, names or quotes an agent might copy into client work |
| BW-8 | Guardrails | Drafts are checked against page 12 and F-13 at any status, including `DRAFT`. A request that needs a violation stops and names the row |
| BW-9 | Distinctiveness | Client deliverables build on at least one belief (F) and one proof point (G) |
| BW-10 | Footer | Deliverables end with the context footer from `context-rules.md` §9 |

## Fit for our clients

| ID | Check | Passes when |
|---|---|---|
| BW-11 | No SaaS defaults | Signups, trials, MRR/ARR, funding rounds, PLG, MQL/SDR pipelines and app stores are not assumed. Where the skill uses them, it says when they apply and offers the local-service equivalent |
| BW-12 | Conversions | The conversion is a call, booking, form or signed job unless the client's rows say otherwise. CTAs follow 01's required CTA language and I-3 lead handling |
| BW-13 | Capacity | Plans and campaigns respect B-6 capacity and seasonality (I-10); nothing pushes a service the client can't take on |
| BW-14 | Local presence | Where relevant, the skill accounts for Google Business Profile, reviews, service area, listings and the L rows (local market research) |
| BW-15 | Segment rules | The skill works under `context-rules.md` §7: clinical, multi-location, home services, professional services. Persuasion tactics that conflict with clinical rules are gated |
| BW-16 | Style ownership | The skill distinguishes agency artifacts (Baker house style) from client deliverables (client brand) and loads the right voice |

## Systems and outputs

| ID | Check | Passes when |
|---|---|---|
| BW-17 | Our tools | Defaults point to BML's stack: ClickUp, Google Drive, Windsor.ai (GA4, Ads, Search Console, Business Profile, Meta), Semrush, Fireflies, Wix. Other tools (Ahrefs, HubSpot, Stripe, Mixpanel, Notion) are marked "if the client uses it", not assumed |
| BW-18 | Output location | Finished work goes to the client's Drive `OUTPUTS/<folder>` (per the Skills Registry) with a ClickUp record where the work is tracked. No persistent work in machine-local paths (`~/…`), and no default publishing to Notion or GitHub |
| BW-19 | Write-back | Research findings that answer a client question are staged as `DRAFT` rows in the miner's review format (`context-rules.md` §10). The skill never writes to ClickUp on its own |
| BW-20 | Approvals | Anything that publishes, sends or spends stops for a human checkpoint; client-facing work notes the client's approver (J-1) |
| BW-21 | Recurring work | Anything that repeats on a cadence is set up as a scheduled task, never in-session cron or a "loop until" that dies with the session |
| BW-22 | Privacy | No patient or customer personal data, no reviewer names, no credentials anywhere in the skill's inputs, outputs or examples |

## Library hygiene

| ID | Check | Passes when |
|---|---|---|
| BW-23 | Registry fit | What the skill consumes and produces matches its Skills Registry row; overlaps are covered in the Registry's Disambiguation table |
| BW-24 | Merge-friendly | BML text sits between `<!-- BML:START <name> -->` and `<!-- BML:END <name> -->` markers; upstream text outside them is unchanged unless a Directional change was approved |
| BW-25 | Versioning | `metadata.version` and `VERSIONS.md` use `<upstream version>-bml.<n>`; the repo release version is bumped the same way |
| BW-26 | Evals | The skill has 2–4 BML evals covering its BML behavior, alongside the upstream evals, and `validate-skills.sh` passes |
| BW-27 | Public repo | Nothing client-specific (names, codes, facts) appears in the skill. Client identity belongs only in output |

---

## Triage verdicts

Before the detailed audit, every skill in a section gets one verdict:

| Verdict | Means |
|---|---|
| **Rewrite** | The skill's core assumptions don't fit BML; it needs BML modes or a restructured workflow |
| **Overlay** | The method fits; it needs BML blocks for context, tools, outputs and guardrails |
| **Leave** | Already fits once BW-2 is in place. No edits beyond the rules pointer |
| **Not for BML** | No current client need (e.g. app-store or in-product paywall work). Mark it in the Registry; don't upgrade |
