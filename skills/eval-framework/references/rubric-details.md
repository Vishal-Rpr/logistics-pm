# Eval Rubric: Detailed Scoring Criteria

## 1. Completeness (1-5)

**What to check:**

For PRDs:
- Overview present and concise?
- Goals are measurable?
- Non-goals explicitly stated?
- Feature broken into components with data points?
- Integration architecture defined (data flow, sync, error handling)?
- Open questions listed?
- Success metrics defined?

For user stories:
- User role, goal, and reason stated?
- Acceptance criteria are testable?
- Edge cases listed?
- Reference to PRD/design included?

**Scoring guide:**
- 5: Every required section present with appropriate depth
- 4: All sections present, one or two are thin
- 3: One section missing or two are superficial
- 2: Multiple sections missing
- 1: Skeleton only; most sections absent

---

## 2. Domain Accuracy (1-5)

**What to check:**
- Freight terminology used correctly (BOL vs. AWB, FCL vs. LCL, buy rate vs. sell rate)
- Workflows match real freight forwarding operations (quoting -> booking -> tracking -> invoicing)
- Integration points are realistic (accounting systems for financials, carrier APIs for tracking, rate providers for benchmarking)
- Regulatory/compliance mentions are accurate (customs, Incoterms, ISF)
- Stakeholder roles are correct (ops team processes bookings, finance handles credit, sales handles RFQs)
- Financial formulas make sense (credit exposure calculation, available credit derivation)

**Scoring guide:**
- 5: Reads like it was written by someone who works in freight forwarding
- 4: Terminology and workflows are correct; minor imprecisions
- 3: Mostly correct but includes generic language where domain-specific language is needed
- 2: Contains errors in terminology or workflow descriptions
- 1: Generic document with no meaningful domain knowledge

---

## 3. Actionability (1-5)

**What to check:**
- Could an engineer estimate the work from this spec?
- Are data models or field definitions explicit enough?
- Are acceptance criteria specific and testable (not vague like "system should be fast")?
- Are edge cases addressed (partial payments, sync failures, expired credit limits)?
- Are phasing decisions clear (what's in v1 vs. later)?
- Are technical constraints stated (latency requirements, API quotas, caching needs)?

**Scoring guide:**
- 5: An engineer could start a sprint plan from this document
- 4: Engineer can estimate but would need 1-2 clarifications
- 3: General direction is clear but significant details need filling in
- 2: Too vague for engineering estimation
- 1: Aspirational description with no implementable detail

---

## 4. Style Consistency (1-5)

**What to check (requires reference docs):**
- Heading structure matches reference PRDs?
- Level of detail per section is consistent?
- Table usage matches (data points in tables vs. prose)?
- Tone is consistent (direct vs. formal, concise vs. detailed)?
- Section naming convention matches?
- Component-based structure used where the reference uses it?

**Scoring guide:**
- 5: Indistinguishable from the user's own writing
- 4: Same structure and tone; minor formatting differences
- 3: Similar structure but noticeably different voice or level of detail
- 2: Different structure or tone from reference
- 1: No resemblance to reference docs
- N/A: No reference docs available to compare against

---

## 5. Metric Specificity (1-5)

**What to check:**
- Are success metrics defined (not just "measure success")?
- Are there specific targets (">90% within 60 days" vs. "high adoption")?
- Is there a mix of metric types?
  - Product metrics: adoption rate, time saved, task completion
  - Operational metrics: sync reliability, error rate, API quota utilization
  - Business metrics: revenue impact, cost reduction, risk reduction
- Are guardrail metrics defined (things that should NOT get worse)?
- Are metrics connected to the goals stated at the top?

**Scoring guide:**
- 5: Metrics section could be copy-pasted into an OKR doc
- 4: Clear metrics with most having targets
- 3: Metrics listed but targets are vague or missing
- 2: Generic metrics not tied to the specific feature
- 1: No metrics section or "TBD" only
