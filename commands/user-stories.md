---
name: user-stories
description: "Break a PRD or feature description into Linear-ready user stories. Each story follows a structured template with goal, scope and logic, design/PRD references, and acceptance criteria. Use after /logistics-pm:write-spec to complete the spec-to-ticket workflow."
---

# /logistics-pm:user-stories

Break a PRD or feature description into actionable user stories ready for Linear (or any tracker).

## Arguments

`$ARGUMENTS` -- A PRD (pasted text or file reference), or a feature description to decompose into stories.

## Process

1. Read the `freight-domain-knowledge` skill for domain context.
2. Read `reference-docs/sample-user-story.md` for the story template structure.
3. If reference documents exist in `reference-docs/`, read the `style-matching` skill to match the user's writing patterns.

4. Decompose the input into user stories. For each story:
   - Identify a single user-facing outcome (not an internal task)
   - Frame it from the perspective of a specific user role (ops team, finance, sales, account manager, client)
   - Keep scope small enough for one sprint

5. For each story, produce:

   **Title:** `[User Story] - <concise action description>`

   **Description body:**
   - **Goal:** What the user is trying to accomplish and why it matters
   - **Scope & Logic:** What specifically gets built. Include field definitions, business rules, edge cases. For logistics features, call out: integration touchpoints, sync behavior, latency constraints
   - **Figma designs / PRD:** Link back to the source PRD or design reference
   - **Acceptance Criteria:** Testable conditions that define "done". Each criterion should be verifiable by QA without ambiguity

   **Metadata:**
   - **State:** Backlog
   - **Labels:** ["User Story"]
   - **Priority suggestion:** Urgent / High / Medium / Low (based on dependency order and user impact)

6. Order the stories by dependency: foundational data model stories first, then business logic, then UI, then polish.

7. After generating, run the `eval-framework` skill against the full set. Focus scoring on actionability and domain accuracy dimensions.

## Output Format

```
# User Stories: [Feature Name]

Stories: [count] | Estimated sprint coverage: [X-Y sprints]

---

## Story 1: [User Story] - [Title]

**Priority:** [level]

### Goal
[What the user accomplishes]

### Scope & Logic
- [Specific implementation detail]
- [Business rule]
- [Integration point / sync behavior]

### PRD Reference
[Link or reference to source PRD section]

### Acceptance Criteria
- [ ] [Testable condition 1]
- [ ] [Testable condition 2]
- [ ] [Testable condition 3]

---

## Story 2: ...
```

## Example

```
User: /logistics-pm:user-stories [pastes credit limit PRD]
Claude: [Decomposes into 8 stories: data model for invoice tracking, accounting-system sync service, credit limit assignment UI, near-limit alerting, over-limit blocking logic, reconciliation dashboard, audit trail, admin configuration]
Claude: [Each story has goal, scope with integration details, acceptance criteria]
Claude: [Appends eval scorecard focused on actionability]
```
