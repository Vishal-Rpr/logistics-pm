---
name: logistics-prd-generation
description: "This skill generates Product Requirements Documents tuned for freight forwarding and logistics features. It should be used when the user asks to 'write a PRD', 'create a spec', 'draft requirements', or 'write a feature spec' for anything related to shipping, logistics, freight, rate management, credit limits, tracking, invoicing, or supply chain operations. It adds domain-specific sections that generic PRD templates miss: API integration points, data sync architecture, speed constraints, compliance considerations, and financial risk handling."
---

# Logistics PRD Generation

## Purpose

Generate PRDs for logistics/supply chain features that go beyond generic product specs. A freight forwarding PRD needs sections that standard PM templates omit: sync architecture, API integration points, latency requirements, compliance constraints, and financial exposure handling.

## Instructions

1. Read `references/prd-template.md` for the required document structure.
2. If reference documents exist in `reference-docs/`, read them to match the user's writing style and level of detail. See the `style-matching` skill for guidance.
3. Ask the user for the feature topic if not already provided. Before generating, clarify:
   - What workflow does this affect? (quoting, booking, tracking, invoicing, credit management)
   - What systems does it integrate with? (accounting system, carrier APIs, rate providers, TMS)
   - Who are the primary users? (ops team, finance, sales, clients)
   - Are there speed or latency constraints?
4. Generate the PRD following the template structure. For each section:
   - **Overview**: 2-3 sentences. State what this does and why it matters now.
   - **Goals**: 3-5 bullet points. Measurable where possible.
   - **Non-goals**: Explicitly scope out what this does NOT do. Critical for logistics features where scope can creep into adjacent systems.
   - **Domain-specific sections**: Break the feature into components. Each component gets its own data points table, calculated fields, and key considerations. Use tables for data points (Field | Description | Source).
   - **Integration architecture**: Data flow direction, sync frequency, error handling. Specify which system is the source of truth.
   - **Open questions**: Unresolved decisions. Frame as questions, not assumptions.
   - **Success metrics**: Include both product metrics (adoption, time saved) and operational metrics (sync reliability, error rates, quota utilization).
5. Use accessible language. Avoid jargon unless it is standard freight terminology (which should be used precisely).
6. Save the output as `PRD-[feature-name].md`.

## Quality Checks

Before finalizing, verify:
- [ ] Every integration point specifies data flow direction and failure handling
- [ ] Speed/latency constraints are stated where the feature touches the quoting or booking path
- [ ] Financial features include credit exposure or risk language
- [ ] Compliance considerations are addressed if the feature touches customs, duties, or documentation
- [ ] Open questions are actual unknowns, not disguised assumptions
- [ ] Success metrics include at least one operational metric alongside product metrics

## Notes

- Prefer component-based structure (see Credit Limit PRD in `reference-docs/`) over monolithic feature descriptions
- Use tables for data point definitions; they are clearer than prose for engineering handoff
- Always distinguish decision-support features from automation (logistics PMs must make this explicit because of regulated workflows)
