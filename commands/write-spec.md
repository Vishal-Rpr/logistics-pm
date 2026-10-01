---
name: write-spec
description: "Generate a logistics-domain Product Requirements Document. Covers freight forwarding features including rate management, credit limits, tracking, invoicing, and API integrations."
---

# /logistics-pm:write-spec

Generate a PRD for a logistics or freight forwarding feature.

## Arguments

`$ARGUMENTS` — The feature or problem to spec out. Examples:
- "automated rate comparison across three NVOCCs"
- "client credit limit enforcement with accounting-system sync"
- "real-time container tracking dashboard"

## Process

1. Read the `freight-domain-knowledge` skill for domain context.
2. Read the `logistics-prd-generation` skill and its `references/prd-template.md` for the PRD structure.
3. Check `reference-docs/` for style reference documents. If they exist, read the `style-matching` skill and apply it.

4. Before generating, ask the user to clarify (if not already clear from the arguments):
   - Which workflow does this affect? (quoting, booking, tracking, invoicing, credit management)
   - What external systems does it integrate with?
   - Who are the primary users?
   - Are there latency or speed requirements?

5. Generate the PRD following the template structure. Apply domain knowledge from `freight-domain-knowledge` to ensure accuracy.

6. After generating, run the `eval-framework` skill against the output. Include the scorecard at the end.

7. Save the PRD as `PRD-[feature-name].md`.

## Example

```
User: /logistics-pm:write-spec carrier rate caching for the quoting workflow
Claude: [Asks clarifying questions about carriers, cache TTL, quoting team size]
Claude: [Generates PRD with: Overview, Goals, Non-goals, Components (Cache Layer, Rate Ingestion, Quote Display), Integration Architecture, Open Questions, Success Metrics]
Claude: [Appends eval scorecard]
```
