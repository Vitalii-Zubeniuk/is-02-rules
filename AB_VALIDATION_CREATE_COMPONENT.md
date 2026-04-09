# A/B Validation: create-component Command

## Objective
Validate the impact of project rules on component scaffolding quality using the same command input.

## Test Scenario
- Command: `/create-component`
- Input args: `PresenceBadge packages/excalidraw/components`
- Method: Two dry-run executions with identical input
  - A: Rules active (`.cursor/rules/*.mdc` + workspace instructions)
  - B: Rules inactive (command-only behavior)

## Run A (Rules Active)
### Expected rule influence
- Functional component conventions
- CSS modules preference
- Architecture boundary awareness
- TypeScript quality constraints
- Testing/validation awareness

### Output summary
- File path: `packages/excalidraw/components/PresenceBadge.tsx`
- Style path: `packages/excalidraw/components/PresenceBadge.module.scss`
- Included:
  - Typed props
  - Better accessibility metadata (`aria-label`, etc.)
  - Structured output sections (export/import suggestions)
  - Rule-aware validation notes and suggested test coverage

## Run B (Rules Inactive)
### Output summary
- File path: `packages/excalidraw/components/PresenceBadge.tsx`
- Style path: `packages/excalidraw/components/PresenceBadge.scss`
- Included:
  - Minimal typed props
  - Basic visual scaffold
- Missing/weaker areas:
  - No CSS module usage
  - No explicit architecture/dependency guardrails
  - No rule-driven quality checks
  - Less robust output rationale

## Side-by-side Comparison
1. Styling strategy
- A used `.module.scss`
- B used plain `.scss`

2. Quality depth
- A provided richer scaffolding and validation context
- B provided a simpler starter implementation

3. Rule alignment
- A aligned with project constraints and verification expectations
- B was only aligned to command text and did not enforce repo-level standards

## Conclusion
Rules materially improve generated output quality and consistency for `/create-component`.

- They increase compliance with architecture and style conventions.
- They produce more review-ready scaffolds (better typing, structure, and validation guidance).
- They reduce ambiguity for contributors by embedding project-specific expectations.

## Final Verdict
- Preferred mode for team workflows: **Rules Active (A)**
- Use Rules Inactive (B) only for quick prototyping when strict project conformity is not required.
