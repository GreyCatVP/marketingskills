# BML Context Rules

Shared rules for every marketing skill doing work for a Baker Labs (BML) client. Read this file before the skill's own instructions whenever the work is for a BML client: the request names one (by code like `[SVN]` or by name), it runs in a client's Claude Project, or `.agents/product-marketing.md` (or a legacy copy) starts with `Generated for`, `UNGENERATED for` or `INTAKE DRAFT for`.

Where these rules conflict with the skill's own defaults, examples or templates, these rules win. The skill still decides *how* to do the marketing work; these rules decide *what is true about the client* and *what may be said*.

Not doing BML client work (BML's own marketing, a general question, a non-client project)? Ignore this file.

---

## 1. Where client context comes from

Client context lives in the client's ClickUp space, in the `Client Operations` doc:

| Page | Holds |
|---|---|
| `00` Services and Retainer | What BML is retained for. The scope gate |
| `01`–`06` | Voice, style, customers, journey, design, task statuses |
| `07`–`12` | Question-bank rows: business and goals, offerings, positioning and local market, beliefs, proof, guardrails |
| `13` Product Marketing Context | Generated from the rows on 01–12. Never edited by hand |

`product-marketing` (Load mode) stages page 13 at `.agents/product-marketing.md`, with a scope summary and the client's segment at the top. Every skill reads that file first, as it already does.

**Read order:** router → staged page 13 → current Monthly Context Brief → source pages only if needed (detail behind a cited row, topic filtering, or a stale page 13).

**Staged file types:**

| First line | What it is | How to treat it |
|---|---|---|
| `Generated for [CODE]` | Page 13 for an existing client | All rules below apply |
| `UNGENERATED for [CODE]` | Built from rows because page 13 doesn't exist | All rules apply; many sections will be gaps. Don't fill them |
| `INTAKE DRAFT for [slug]` | A prospect or pre-kickoff client, from public research and intake | No scope gate (there's no page 00 yet). Every fact counts as `DRAFT`: use it only in internal or pitch material, marked "from public research", and never in anything published. `⚠ UNVERIFIED` items are never asserted. Guardrails, segment rules and the footer still apply |

**If the staged file is missing, or its header names a different client:** run `product-marketing` in Load mode before continuing. Never carry one client's context into another client's work.

**Never change page 13 or `.agents/product-marketing.md` for a BML client.** No skill writes to either, for any reason. If a skill offers to update the context file, log a learning, add a changelog line or bump its version, decline. Changes go to rows (section 8).

**Learnings ledger.** Upstream skills keep a `Learnings` table in the context file and offer to add rows to it. For a BML client, page 13 is the only source of learnings: read its Learnings section if it has one, and treat a missing one as normal, not as a gap to fill. A finding that would earn a learning (a test result, a decision with a reason, a repeated correction) becomes a `context` task under section 8.

## 2. Row status

Every fact on pages 01–12 is a row: `ID | Question | Answer | Source | Status`.

| Status | What a skill may do |
|---|---|
| `OPEN` | Nothing. It is a gap |
| `DRAFT` | Never state it as fact in client-facing work. **Exception: guardrail rows (page 12 and F-13) restrict output even at `DRAFT`** |
| `PROVISIONAL` | Use it, and flag it in the footer |
| `APPROVED` | Use it freely, citing the row ID |
| `RETIRED` | Never use it |

A row marked `⚠ UNVERIFIED` is do-not-assert at any status.

Page 13 marks provisional facts `⚠ PROVISIONAL:` and lists every unanswered or draft row under **Known gaps**.

## 3. Scope gate

Before producing anything, find the service on page 00 (summarized at the top of the staged file):

- `Active` → proceed
- `Active — <qualifier>` → produce only what the qualifier permits, and say what it excludes
- absent or anything else → stop: "<service> isn't in [CODE]'s retainer." Offer a scope note for the account lead instead

## 4. Claims

**Facts about the client come only from page 13 or the rows it cites.** Not from the skill's examples, industry norms or plausible guesses.

Where content needs a fact the client record doesn't have — a number, a result, a customer, a testimonial, a credential, a price — write `[NEEDS PROOF: <row ID>]` using the closest question-bank row, or `[NEEDS PROOF: new — <what>]` if none fits. This overrides any "be specific" or "add proof" step in the skill. Never invent a figure, even as an example.

Also:

- Quotes and testimonials come verbatim from page 11 with their row ID. Never compose, merge or tidy one.
- Prices only where the row says the client approved publishing them (D-4).
- Review counts, ratings, and anything in the staged file's "Volatile facts" block carry "⚠ confirm before publishing".
- Local market rows (L-*) are BML research: use them for targeting, topics and planning. A market statistic in public copy cites its source and date, and never implies the client's own results.

## 5. Guardrails

Check every draft against:

- every page 12 row and F-13, **at any status including `DRAFT`**
- the router's do-not-assert list
- page 13's never-use words (H-5)
- the segment rules in section 7

A violation in the draft → fix it and note it in the footer. **A request that requires a violation** → stop before drafting and name the row: "Blocked by J-4: never name competitors in public content." When two rules conflict, the more cautious one wins.

## 6. Distinctiveness

A client deliverable builds on **at least one belief** (F row, `APPROVED` or `PROVISIONAL`) **and at least one proof point** (G row, or a citable A row). That is what keeps it from reading like every competitor's.

- Short units (one headline, one RSA, one post) are checked across the set or campaign.
- If the record has no usable belief or proof for the topic, don't fake one. Mark the footer `Distinctiveness: FAIL — not client-ready` and list the rows to fill.
- Agency artifacts (reports, plans, decks) skip this check.

Also check capacity and lead handling: don't push a service the client can't take on (B-6), promise response times beyond I-3, or duplicate something the brief shows as recently published.

## 7. Segment rules

The staged file lists the client's segments; a client can have more than one (a multi-location dental group is clinical and multi-location). Apply every listed segment's rules on top of everything above. If the segments were inferred and the client could be clinical, apply the clinical rules until someone confirms otherwise.

**Clinical practices** (medical, dental, eye care, veterinary, fertility, radiology, surgery)

- No patient data in any tool or draft. No patient names, dates of service, conditions tied to a person.
- Clinical claims (outcomes, efficacy, results) only from approved published copy, and flagged for review by a named provider.
- Testimonials and patient stories need written patient authorization on file (G-5, J-6).
- No scarcity, urgency, fear or outcome-promise framing unless a page 12 row permits it. Use reassurance instead: approved social proof, process clarity, credentials, access (hours, insurance, booking).
- Tracking pixels and lead forms on patient-facing pages are a compliance question for a person, not a skill. Flag them; don't add them.

**Multi-location brands** (franchise or several offices)

- Brand voice, beliefs and guardrails come from the parent brand; locations hold only their own local rows.
- Franchisor brand rules (J-3) apply to every location.
- Never mix one location's proof, hours or providers into another's content.

**Home services and construction**

- Show license details where J-3 requires them.
- Project photos only with a release on file (J-6). Never present AI images as real projects.
- Respect service area (B-8, L-1) and seasonal capacity (B-6, I-10).

**Professional services and real estate**

- Credentials and disclosures per J-3.
- No outcome guarantees (returns, case results, sale prices) unless a row permits them.

**Other** — no extra rules beyond sections 1–6.

## 8. Client edits and feedback

When a client edits, rejects or comments on a deliverable, or anyone asks to change a fact on file (`product-marketing` Load, Step 7, follows this too):

1. Compare BML's version with the client's and list each meaningful change.
2. Name the row each change touches (or "no row") and a category: tone, accuracy, seriousness, belief, claim.
3. Draft one task per change, tagged `context`, named `Context — <CODE> — <row ID> — <short>`, with before/after, the client's words and the source link. Create it only after the user approves.
4. Never edit the row or page 13 directly, and never add a Learnings entry to the staged file. The monthly conflict check in `client-context-builder` classifies the change and drafts row edits for approval.

## 9. Context footer

Every client deliverable and agency artifact ends with this block. It is internal: strip it before the piece goes to the client.

```
—— Context footer · [CODE] ——
Scope: [service] — [Active / qualifier]
Context: page 13 generated [date] ([current / STALE — source rows checked]) · brief [month]
Rows used: [IDs]
Provisional: [IDs + one line each, or "none"]
Needs proof: [ID — what's missing, or "none"]
Guardrails: [pass / fixed: … / blocked: …]
Distinctiveness: [belief ID + proof ID / FAIL — rows to fill / n/a for agency artifacts]
Gaps to ask about: [1–3 IDs worth raising in the next monthly session]
```

## 10. Writing back

Skills never write to ClickUp on their own. When a research skill finds something that answers an `OPEN` row, stage it for review in the format `client-context-miner` uses:

`ID | Answer (≤40 words, one fact) | Source label + link | Confidence | Date`

Source labels: `Client-stated:` · `Client-quoted:` · `Agency-authored:` · `Research:` · `Benchmark:`. Everything lands as `DRAFT`. Page 10 beliefs and page 12 guardrails are never filled from research. A finding that disagrees with a non-`OPEN` row is a conflict for the next monthly check, not an edit.
