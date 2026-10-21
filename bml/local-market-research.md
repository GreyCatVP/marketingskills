# BML Local Market Research

How BML researches a client's local market and fills the `L` rows ("Local market (research)" on page 09 of the client's `Client Operations` doc). These rows feed the **Local market context** section of page 13.

The client is not expected to know any of this. BML finds it.

Called from:

- `product-marketing` Intake mode — for prospects and new clients, before the first interview
- `client-context-miner` Backfill — for existing clients, alongside its Web and Benchmark passes
- the quarterly market refresh loop (`marketing-loops`) — refreshes L-3, L-4, L-5 and L-7

---

## Before you start

You need:

- **Business name, website, and each location's address.** From page 00 or 07 for existing clients; from the website and Google Business Profile for prospects.
- **Priority services** (D-1). If unknown, use the 3–5 services the website leads with, and say so.
- **Stated service area** (B-8) if the client has given one.
- **Segment:** clinical, multi-location, home services, professional services, or other (see `context-rules.md` §7).

Tools, in order of preference: Semrush (keywords, local competitors, paid), Google Places search (map results, ratings, review counts), web search and fetch (sites, directories, news, Census), Windsor.ai only for the client's *own* accounts. Pull each data source once per run and reuse it.

## The rows

| ID | Question | How to research it | Output (≤40 words per row) |
|---|---|---|---|
| L-1 | Market definition: towns, ZIPs or counties in the service area; drive time from each location | Start from B-8. If none, use towns within ~20–30 minutes' drive of each location, and the towns the website and GBP name. Note core vs. willing-to-travel | Named towns/counties, core vs. edge |
| L-2 | Who lives there | US Census ACS (data.census.gov) for the L-1 counties or places: population, households, median age, median household income, owner-occupied share, 5-year growth | 4–6 figures, each with year |
| L-3 ★ | Local search demand | Semrush keyword research for "<service> + <town>" and "<service> near me" for each priority service, US database, local where available. Note monthly volume, trend, and seasonal peaks | Top 5–10 searches with volume; peak months |
| L-4 ★ | Who wins the map pack and organic results | Google Places search for each priority service in the core towns; Semrush organic competitors. Record the top 3 map results and top 3 organic results per service | Name, rating, review count, primary category |
| L-5 | What customers praise and complain about at those competitors | Read the most recent and lowest-rated reviews for the L-4 competitors. Group into themes | 3–5 praise themes, 3–5 complaint themes. **No reviewer names or quotes that identify a person** |
| L-6 | Competitor offers and messaging | Competitor homepages, service pages, current ads. Note price signals, promotions, guarantees, and the claims they all repeat | Claims everyone makes (feeds E-8); any standout offer |
| L-7 | Who bids on local search terms | Semrush paid research on the L-3 terms | Advertisers seen, typical CPC range |
| L-8 | Referral ecosystem | Public directories and association listings: physician groups and hospital systems (clinical), realtors and builders (home services, real estate), insurers, general contractors | Named organizations by type, not individuals |
| L-9 | Local media and community | Local publications, TV, podcasts, chambers, business associations, recurring events, sponsorship options | Named outlets and organizations |
| L-10 | Local demand drivers | Local news and county or state data from the last 12 months: housing market, new developments, major employers, regulation, weather or seasonal events | 2–4 drivers, each dated |
| L-11 | Category platforms that matter here | Search the priority services on category platforms (e.g. Healthgrades, Zocdoc, Vitals for clinical; Houzz, Angi, Nextdoor for home services; LoopNet, CoStar for commercial real estate). Note where L-4 competitors appear and how they rate | Platforms that rank or are used, with competitor presence |
| L-12 | Which local businesses AI assistants recommend | Ask 5–10 priority prompts ("best <service> in <town>") in ChatGPT, Gemini and Google AI Overviews where available; run each 2–3 times | Businesses named most often, per prompt |

Multi-location clients: run L-1, L-2, L-4, L-5 and L-12 per location and suffix the rows (L-4a, L-4b). Keep one row where results don't differ.

## Rules

- **Source:** every row's Source cell starts with `Research:` or `Benchmark:`, then the tool or URL, then the date pulled: `Benchmark: Semrush keyword research, 26.10.20`.
- **Status:** everything lands as `DRAFT`. A strategist promotes to `PROVISIONAL` and the account lead to `APPROVED`; no client sign-off is needed.
- **Only fill `OPEN` rows.** If a refresh finds a different answer for a filled row, stage it as an update with both versions and dates. Don't overwrite.
- **Freshness:** a row older than 90 days is stale. It is listed under "Volatile facts — confirm before publishing" when page 13 is staged.
- **No personal data:** no reviewer names, no individual referrers, no patient or customer details.
- **Competitor names** are fine in these rows. Public copy still follows J-4.
- **Don't guess.** If a source returns nothing, write `—` and add a note to the review file. A missing number is better than an estimated one.

## Output

Write the findings to the review file of whichever skill called this procedure (`<CODE>_Intake_Review_YY.MM.DD.md` or the miner's `<CODE>_Backfill_Review_YY.MM.DD.md`), as a table:

```
| ID | Answer | Source | Confidence | Date |
|---|---|---|---|---|
| L-3 | "<service> <town>" <n>/mo; "<service> near me" <n>/mo; peaks <months> | Benchmark: Semrush keyword research | High | <YY.MM.DD> |
```

Then list, separately:

- **Signals for other rows:** anything that suggests an answer to a client row (E-1, E-8, A-5, A-6, A-7, A-11), marked as a candidate for the client to confirm
- **Fix-now findings:** problems spotted on the client's own listings (wrong hours, duplicate profiles, inconsistent name/address/phone)

Nothing is written to ClickUp from this procedure. The account lead reviews the file, and `client-context-miner` writes approved rows.
