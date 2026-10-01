# logistics-pm

> **Disclaimer:** Two sample PRDs based on common freight forwarding patterns, fully anonymized. All company, vendor, and product names in this repo have been replaced with generic placeholders (e.g., "rate intelligence platform", "accounting system"). Reference documents are illustrative examples for demonstrating domain-tuned PRD generation and style matching.

A domain-specific product management plugin for freight forwarding and logistics teams. Built to extend generic PM copilots with vertical expertise they lack: operational knowledge of forwarding workflows (quoting, booking, customs clearance, tracking, invoicing, credit management), logistics-tuned PRD generation, a quality evaluation framework, and personalized style matching.

## The Problem

Generic PM copilots produce reasonable output for standard SaaS features. They fall apart on logistics:
- They don't understand that an RFQ response has a 2-3 second latency budget, or that quoting involves buy rates, sell rates, and surcharge stacking (BAF, CAF, THC, ETS, PSS)
- They don't know that a missing ISF filing is a $5,000+ fine, or that demurrage charges accrue daily when cargo sits at port
- They produce specs that skip integration architecture (sync direction, source of truth, failure handling), leaving engineers to guess
- They have no mechanism to evaluate whether their output is operationally accurate or just plausible-sounding

## What This Plugin Does

**4 skills** that auto-fire based on context:

| Skill | What it adds |
|-------|-------------|
| `freight-domain-knowledge` | Full operational knowledge of the freight forwarding lifecycle: macro logistics (procurement through last-mile), forwarding execution (quotation, consolidation, customs, main carriage, final delivery), documentation workflows (BOL hierarchy, commercial invoices, ISF), financial reconciliation (credit exposure, accounting system sync, milestone-based invoicing), and the digital transformation reshaping the industry. Not a glossary -- working knowledge that informs every section of a spec. |
| `logistics-prd-generation` | PRD generation tuned for logistics features. Adds sections generic plugins miss: integration architecture with data flow direction, sync frequency and failure handling, compliance requirements, financial exposure, demurrage/detention economics. Template derived from real freight forwarding PRDs. |
| `eval-framework` | 5-dimension quality rubric (completeness, domain accuracy, actionability, style consistency, metric specificity). Scores output 1-25 with a ship/polish/rework/start-over verdict. Domain accuracy is weighted heavily because it's where generic AI fails hardest on logistics specs. |
| `style-matching` | Reads your existing PRDs from `reference-docs/` and replicates your structure, tone, heading hierarchy, table usage, and level of detail. Outputs sound like you wrote them, not like a template. |

**2 commands** for explicit workflows:

| Command | What it does |
|---------|-------------|
| `/logistics-pm:write-spec` | End-to-end PRD generation: loads domain context, follows your template, matches your writing style from reference docs, then auto-evaluates the output with the quality rubric |
| `/logistics-pm:user-stories` | Takes a PRD or feature description and decomposes it into Linear-ready user stories with goal, scope and logic, PRD references, and testable acceptance criteria. Ordered by dependency. |

**Reference docs** included:
- Two sample freight forwarding PRDs (client credit limit with accounting-system integration, market rate benchmarking for RFQ quoting) as style examples
- User story template for ticket creation

## Quick Start

1. Clone this repo into your Cowork plugins directory
2. Add your own PRDs to `reference-docs/` for personalized style matching
3. Connect Linear via the included `.mcp.json` (optional)
4. Use `/logistics-pm:write-spec [feature]` to generate your first logistics PRD

## Plugin Structure

```
logistics-pm/
├── .claude-plugin/
│   └── plugin.json
├── .mcp.json
├── README.md
├── CONNECTORS.md
├── skills/
│   ├── freight-domain-knowledge/
│   │   └── SKILL.md
│   ├── logistics-prd-generation/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── prd-template.md
│   ├── eval-framework/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── rubric-details.md
│   └── style-matching/
│       └── SKILL.md
├── commands/
│   ├── write-spec.md
│   └── user-stories.md
└── reference-docs/
    ├── credit-limit-prd.md
    ├── rate-benchmarking-prd.md
    └── sample-user-story.md
```

## How It Differs from Generic PM Plugins

| Capability | Generic PM plugin | logistics-pm |
|------------|-------------------|-------------|
| Domain knowledge | None | Full freight forwarding lifecycle: quoting, booking, customs, tracking, invoicing, credit management, consolidation, Incoterms, container economics |
| PRD sections | Standard template | + Integration architecture, data sync design, compliance constraints, financial exposure, demurrage/detention economics |
| Style matching | No | Reads your reference docs and replicates your writing patterns |
| Output evaluation | No | 5-dimension rubric with scoring, verdict, and improvement suggestions |
| Ticket decomposition | No | PRD-to-user-stories with dependency ordering and acceptance criteria |

## Built With

- Real knowledge from a freight forwarding B2B company and its application
- Operational domain knowledge from hands-on product management in the forwarding industry
- Eval framework principles from Anthropic's documentation on developing tests for AI systems
- Prompt architecture patterns from open-source PM copilot research (pm-skills, knowledge-work-plugins)

## Author

Vishal Prabhakar -- Senior Product Manager in B2B freight forwarding, building AI product tools.
