---
name: style-matching
description: "Match PRD structure, tone, and formatting to the two style PRDs included in this skill. Use with logistics-prd-generation or /logistics-pm:write-spec. The style corpus is in this file. Do not look for separate reference files."
---

# Style Matching from Reference Documents

## Purpose

Generate documents that sound like the user wrote them, not like a generic AI template. The two style PRDs are included below in this skill. They are the same documents the RAG co-pilot uses.

## Instructions

1. Use the two PRDs in the "Style corpus" section of this skill. They are already here. Do not search the chat folder, the Desktop, or `reference-docs/` for them. Do not say they are missing. Do not fall back to a generic template.
2. Do not use `sample-user-story.md` as PRD style.
3. From those two documents, extract these style signals:

### Structure Signals
- What sections do they use? In what order?
- Do they use a component-based structure (separate sections per feature component) or a monolithic flow?
- How deep is the heading hierarchy? (H2 only? H2 + H3? H2 + H3 + H4?)
- Do they use horizontal rules (`---`) to separate major sections?

### Detail Level Signals
- How long is the Overview section? (1 sentence? 2-3 sentences? A paragraph?)
- Do they define data points in tables or in prose?
- Do they include calculated fields and formulas?
- How specific are their acceptance criteria?
- Do they include key considerations sections per component?

### Tone Signals
- Formal or conversational?
- Active voice or passive?
- Do they use "we" or "the system"?
- Do they use bold for emphasis or keep it minimal?
- Do they use jargon freely or define terms?

### Formatting Signals
- Tables for data definitions?
- Bullet points or numbered lists?
- Code blocks for formulas or system behavior?
- Consistent naming conventions for fields?

4. When generating a new document, replicate these signals. The output, placed next to the two PRDs below, should look like it came from the same author.

## Notes

- Style matching is about structure and voice, not content. The new PRD should cover its own topic fully while sounding like the user.
- If the two documents vary in style, follow the more detailed one.
- Do not copy content from the style corpus. Only replicate the structural and tonal patterns.

## Style corpus

The following two documents are the complete style corpus.

### Document 1
# PRD: Client credit limit and accounting system integration

> _Sample reference document for writing-style matching only. Fictional B2B freight context; contains no company-specific data._

## Overview
Improve the integration between a B2B freight forwarding platform and its accounting system to enable holistic client credit management. This system will unify invoicing, payment tracking, credit/debit note handling, and credit limit evaluation into a single source of truth for customer financial health.

## Goals
- Provide real-time visibility into each customer's credit position
- Automate the sync of invoicing and payment data between the platform and the accounting system
- Enable credit limit assignment, enforcement, and revision workflows
- Reduce manual reconciliation effort between the platform and the accounting system

## Non-goals
- Replacing the accounting system as the system of record for accounting
- Automating credit limit decisions (this is a decision-support tool, not auto-approval)

---

## Component 1: Invoice portion

Tracks the full lifecycle of each invoice and all adjustments made against it.

### Data points
| Field | Description | Source |
|-------|-------------|--------|
| Invoice ID | Unique identifier synced from accounting system | Accounting system |
| Invoice value | Original billed amount | Accounting system |
| Payments applied | Payments received and matched against this invoice | Accounting system |
| Credit notes (invoice-level) | Credit memos applied/adjusted against this specific invoice | Accounting system |
| Debit notes (invoice-level) | Debit memos applied/adjusted against this specific invoice | Accounting system |
| Invoice revisions | Any amendments to the original invoice value or line items | Accounting system / App |

### Calculated fields
- **Net invoice outstanding** = Invoice value - Payments applied - Credit notes adjusted + Debit notes adjusted (accounting for revisions)

### Key considerations
- Invoices may be partially paid; the system must track partial payment application
- Revisions to invoices should maintain an audit trail (original value, revised value, reason, timestamp)
- Credit and debit notes at the invoice level must be distinguished from customer-level notes (see Component 2)

---

## Component 2: Customer level balances

Tracks credits, debits, and payments at the customer level that have NOT yet been adjusted against a specific invoice.

### Data points
| Field | Description | Source |
|-------|-------------|--------|
| Unadjusted advances/payments | Payments or deposits made by the customer not yet applied to an invoice | Accounting system |
| Customer-level credit notes | Credit memos issued to the customer not tied to a specific invoice | Accounting system |
| Customer-level debit notes | Debit memos issued to the customer not tied to a specific invoice | Accounting system |

### Key considerations
- In the accounting system, credit memos can exist at the customer level without being associated to any invoice. These sit as open credits on the customer's account and can be applied to future invoices or refunded.
- Unadjusted advances should be flagged for reconciliation -- they represent customer funds held on account
- The system should surface aging of unadjusted items (e.g., "Sample Client has a $5,000 unapplied credit note from 45 days ago")

---

## Component 3: Customer credit evaluation

Manages the documentation, assignment, and governance of credit limits for each client.

### Workflow
1. **Documentation** -- Collect required financial documents from the client (e.g., bank references, trade references, financial statements)
2. **Credit limit assignment** -- Based on evaluation, assign a credit limit amount to the customer
3. **Validity period** -- Each credit limit has an effective date and expiry date
4. **Revision** -- Credit limits can be revised (up or down) based on payment behavior, changed business conditions, or periodic review

### Data points
| Field | Description |
|-------|-------------|
| Credit limit amount | Maximum outstanding balance allowed for this customer |
| Effective date | When this credit limit takes effect |
| Expiry date | When this credit limit expires and must be reviewed |
| Supporting documents | Uploaded references and financial documents |
| Approval status | Pending / Approved / Rejected / Under review |
| Revision history | Log of all changes to the credit limit with reasons |

### Key considerations
- Credit limits should trigger alerts before expiry (e.g., 30 days before)
- Revision requests should follow an approval workflow (who can request, who can approve)
- Historical credit limit data should be retained for audit purposes

---

## Component 4: Customer credit limit (the combined view)

This is the final, holistic output that combines all three components above into a single customer credit position.

### Formula

```
Customer credit exposure =
    Sum of all outstanding invoices (net of payments and adjustments at invoice level)
  + Debit notes issued at customer level
  - Credit notes issued at customer level
  - Unadjusted advances/payments
```

### Credit limit status

```
Available credit = Assigned credit limit - Customer credit exposure
```

| Status | Condition |
|--------|-----------|
| Within limit | Available credit > 0 |
| Near limit | Available credit < configurable threshold (e.g., 10% of assigned limit) |
| Over limit | Available credit < 0 |
| Expired | Credit limit past expiry date |

### Business rules
- When a customer is **over limit**, new shipment bookings should be flagged/blocked (configurable per customer)
- When a customer is **near limit**, an alert should be sent to the account manager and finance team
- When a credit limit is **expired**, treat as zero limit until renewed
- Dashboard should provide a real-time view of all customers' credit positions

---

## Sync architecture (accounting system integration)

### Direction of data flow
- **Accounting system -> App**: Invoice data, payment records, credit/debit memos, customer balances
- **App -> Accounting system**: (Future) Credit limit status, hold flags

### Sync frequency
- To be determined: real-time webhook vs. scheduled polling (recommend starting with scheduled polling every 15-30 minutes, with option to move to webhooks)

### Error handling
- Failed syncs should be logged and retried
- Data discrepancies between app and accounting system should surface in a reconciliation dashboard

---

## Open questions
- What approval workflow is needed for credit limit assignment and revision? (Single approver vs. multi-level?)
- Should shipment bookings be blocked when over limit, or just warned? Is this configurable per customer?
- What is the desired sync frequency with the accounting system?
- Are there existing credit policies or documentation templates to incorporate?
- How should customers with multiple sub-entities in the accounting system be handled?

---

## Success metrics
- Reduction in time spent on manual credit checks before booking
- Reduction in overdue receivables from customers exceeding their credit limit
- Percentage of customers with active, non-expired credit limits
- Sync reliability (% successful syncs with accounting system)


### Document 2
# PRD: Market rate benchmarking for RFQ quoting

> _Sample reference document for writing-style matching only. Fictional B2B freight context; cost figures are illustrative estimates, not actual vendor pricing._

## Overview
Integrate an ocean and air freight rate intelligence platform into the RFQ workflow so that every quote shared with a client is validated against an independent market benchmark. The goal is to ensure pricing is competitive (not leaving money on the table) and defensible (not quoting below market), with minimal added time to the quoting process.

## Goals
- Benchmark every RFQ against market rates before the quote reaches the client
- Keep the benchmarking step fast enough that it doesn't slow down quote turnaround
- Give the quoting team a clear market position indicator (above, at, or below market?)
- Build a data layer that over time reveals which lanes are consistently strong or weak

## Non-goals
- Replacing human judgment in pricing -- this is decision-support, not auto-pricing
- Surfacing benchmarking data directly to clients (restricted by typical API license terms)
- Air freight benchmarking in Phase 1 (ocean FCL is the priority; air can follow)

---

## The benchmarking use case

### What happens today
The quoting team receives an RFQ, pulls buy rates from carriers and NVOCCs, applies margin, and sends the quote. There is no systematic check of whether the final price is competitive relative to the broader market. Pricing instincts are based on experience, not data.

### What this changes
Before a quote is sent, the system queries the rate intelligence API for the market rate on that origin-destination-container type combination and displays a benchmark comparison. The quoting user sees whether the proposed rate is below, at, or above the market low/average/high -- and can adjust accordingly.

### Speed requirement
RFQ turnaround is time-sensitive. The benchmarking lookup must add no more than **2-3 seconds** to the workflow. Rate intelligence APIs typically respond in under 1 second for port-to-port queries, which fits this constraint.

---

## Approach evaluation: Standard API vs. MCP wrapper

Both approaches use the same underlying rate intelligence REST API and the same platform subscription. The difference is in how the integration is consumed within the platform.

### Option A: Standard API integration

Direct backend integration -- the application server calls the rate intelligence API during the quoting flow.

**How it works:**
- Backend service calls the estimated-rates endpoint with origin port, destination port, container type, and date
- Response returns market low, average, and high rates
- The UI displays the benchmark alongside the proposed quote

**Pros:**
- Deterministic and predictable -- same query always returns same result
- Easy to audit and log every benchmark check
- No additional abstraction layer; fewer moving parts
- Fits naturally into existing application architecture
- Rate limiting and quota management are straightforward to monitor

**Cons:**
- Requires dedicated backend development (API client, error handling, caching, UI integration)
- Each new use case (e.g., adding air freight, adding carrier spread data) requires code changes
- No conversational interface -- quoting team must use the app UI only

**Best for:** Production quoting workflow embedded in the application

### Option B: MCP server wrapping the rate intelligence API

Build a lightweight MCP (Model Context Protocol) server that wraps the API, enabling AI agents and tools to query market rates conversationally.

**How it works:**
- Python or TypeScript MCP server exposes tools like `get_market_rate(origin, destination, container_type)`
- AI agent calls the tool, gets structured rate data back
- Can be used in any MCP-compatible client

**Pros:**
- Enables natural-language rate queries ("What's the market rate for a 40HC from Shanghai to Rotterdam?")
- Extremely fast to build -- 4 to 8 hours for a working MCP server using the official SDK
- Flexible -- adding new tools (carrier spread, capacity, trends) is just adding a new function
- Useful beyond quoting -- sales prep, market research, management reporting
- Can coexist with Option A -- the MCP server can call the same API

**Cons:**
- Adds an abstraction layer between the user and the data
- AI interpretation adds a small amount of unpredictability vs. direct API
- Not suitable as the sole integration for the production quoting UI -- still need Option A for the app
- Requires MCP client setup for each user who wants conversational access
- Newer technology -- less battle-tested in production freight workflows

**Best for:** Ad-hoc analysis, sales preparation, AI-assisted pricing decisions, and internal research

### Recommendation

**Do both, in sequence.** Start with the MCP wrapper (Phase 1) because it's fast to build, immediately useful for the quoting team's ad-hoc needs, and serves as a proof of concept. Then build the standard API integration (Phase 2) to embed benchmarking directly into the production quoting UI.

| Criteria | Standard API | MCP Wrapper |
|----------|-------------|-------------|
| Build time | 2-4 weeks | 4-8 hours |
| Ongoing maintenance | Medium | Low |
| User experience | Structured UI | Conversational |
| Production-readiness | High | Medium |
| Flexibility for new use cases | Low (code changes) | High (add tools) |
| Dependency on AI layer | None | Yes |

---

## Cost evaluation

### Rate intelligence subscription (applies to both approaches)

Rate intelligence platforms do not typically publish pricing. All plans are custom-quoted based on volume, lanes, and features. The figures below are illustrative order-of-magnitude estimates for planning only, not actual vendor quotes.

| Cost component | Illustrative range | Notes |
|----------------|----------------|-------|
| Platform subscription (ocean) | low-to-mid five figures/yr | Varies widely by forwarder size; smaller orgs trend lower |
| API add-on | Included, or a low-five-figure add-on/yr | Depends on call quota |
| Monthly call quota | Defined per plan | Exceeding quota may trigger warnings or temporary suspension |
| Typical ROI claim | Recovered within a tender cycle | Vendors market low-single-digit % freight spend savings |

### MCP wrapper build cost (one-time, Phase 1)

| Cost component | Estimate | Notes |
|----------------|----------|-------|
| Development time | 4-8 hours | Using Python FastMCP or TypeScript SDK |
| Developer cost | $400-$1,200 | At $50-$150/hr depending on in-house vs. contractor |
| Ongoing maintenance | ~2 hrs/month | SDK updates, minor fixes |
| Infrastructure | $0-$20/month | Runs locally via stdio or cheap cloud hosting |
| **Total Year 1** | **$700-$2,000** | Excluding platform subscription |

### Standard API integration build cost (Phase 2)

| Cost component | Estimate | Notes |
|----------------|----------|-------|
| Development time | 40-80 hours | Backend client, caching layer, UI components, testing |
| Developer cost | $4,000-$12,000 | Depending on team rate |
| Ongoing maintenance | ~4-8 hrs/month | Monitoring, quota management, feature additions |
| Infrastructure | $50-$200/month | Caching layer (Redis or similar), monitoring |
| **Total Year 1** | **$5,000-$15,000** | Excluding platform subscription |

### Total cost comparison (Year 1)

| Scenario | Platform sub | Build cost | Relative total |
|----------|-------------|------------|-------|
| MCP only (Phase 1) | five-figure subscription | low (hundreds-low thousands) | subscription + minimal build |
| Standard API only | five-figure subscription | mid (low-five figures) | subscription + moderate build |
| Both (recommended) | five-figure subscription | mid (low-five figures) | subscription + moderate build |

**The key insight:** The platform subscription dominates total cost (roughly 80-90%) regardless of approach. The MCP vs. standard API decision is a build strategy question, not a cost question.

---

## Rate intelligence API: Pros and cons for per-RFQ benchmarking

### Pros

| Advantage | Detail |
|-----------|--------|
| Data depth | Very large dataset of real contract and spot transactions across a broad set of global port pairs |
| Rate granularity | Market low, average, high, plus carrier-level spread |
| Short vs. long-term rates | Distinguishes short-term/spot from long-term/contract rates -- critical for matching the RFQ type |
| Container type coverage | Standard dry, reefer, and high-cube equipment across 20' and 40' sizes |
| Surcharge inclusion | Rates include common surcharges (e.g., BAF, CAF, ETS, canal) -- apples-to-apples comparison |
| API response speed | Sub-second response times for port-to-port queries; fits the fast turnaround requirement |
| Estimated rates | When direct data is thin on a lane, the platform returns modeled estimates |
| Market position scoring | See where the proposed rate sits in the market distribution, not just a single number |

### Cons

| Risk | Detail | Mitigation |
|------|--------|------------|
| No public pricing | Cannot evaluate cost without a sales conversation | Request a demo and quote; push for a pilot/trial period |
| Monthly call quota | High-volume RFQ benchmarking could burn through quota fast | Implement a caching layer -- rates don't change hourly; a 6-12 hour cache per lane is reasonable |
| LCL coverage gap | Strongest on FCL; LCL data is thinner | Continue using NVOCC rates as the LCL benchmark |
| No third-party data sharing | API license prohibits sharing data directly with clients | Use internally only; show clients the quote, not the benchmark source |
| Rate limiting (HTTP 429) | Burst queries during peak quoting hours could hit limits | Queue and throttle API calls; caching reduces this significantly |
| Geo-hierarchy fallback | On thin lanes, may return rates for a broader region instead of exact port pair | Surface actual origin/destination metadata so users know the granularity |
| Aggregated rates only | Market averages, not a specific carrier's bookable rate | By design for benchmarking; not a buy rate substitute |

---

## Caching strategy (critical for per-RFQ use)

Since every RFQ triggers a benchmark lookup, and the quoting team may handle dozens to hundreds of RFQs per day, a caching layer is essential to stay within quota and maintain speed.

| Parameter | Recommendation |
|-----------|---------------|
| Cache key | `{origin_port}:{destination_port}:{container_type}:{rate_type}` |
| Cache TTL | 6-12 hours for spot rates; 24 hours for contract rates |
| Cache store | Redis (if available) or in-memory with persistence |
| Cache hit rate target | 60-80% (many RFQs share common lanes) |
| Quota impact | A 70% cache hit rate on 100 daily RFQs reduces API calls from 100 to 30/day (~900/month vs. 3,000/month) |

---

## Phased rollout

### Phase 1: MCP wrapper (Week 1-2)
- Build MCP server wrapping the estimated-rates and carrier-spread endpoints
- Deploy locally for the quoting team lead as a pilot
- Validate data quality and response speed against real RFQs
- Confirm subscription tier and quota are adequate

### Phase 2: Production API integration (Week 3-6)
- Build backend service with caching layer
- Integrate benchmark display into the quoting UI
- Add market position indicator (below / at / above market)
- Logging and audit trail for every benchmark check

### Phase 3: Analytics and expansion (Week 7+)
- Lane-level analytics: which routes are consistently above/below market?
- Margin optimization suggestions based on market position
- Air freight benchmarking
- Historical trend view per lane

---

## Open questions
- What is the actual daily/monthly RFQ volume? This directly determines the API quota tier needed
- Should the benchmark be mandatory (blocking) before a quote is sent, or advisory (optional)?
- Which team members get access to the MCP wrapper vs. the production UI?
- Should caching happen at the application level, or would a shared cache across the quoting team be more appropriate?
- What is the split between FCL and LCL RFQs? (Determines how much value the platform adds vs. needing a separate LCL benchmark)

---

## Success metrics
- Percentage of RFQs benchmarked before quote is sent to client (target: >90% within 60 days)
- Average time added to quoting workflow by the benchmark step (target: <3 seconds)
- Reduction in quotes that come back as "too high" from clients (directional)
- Number of lanes identified where pricing was significantly above/below market
- API call volume vs. quota utilization (target: stay below 80%)
- Quote win rate trend after benchmarking is live
