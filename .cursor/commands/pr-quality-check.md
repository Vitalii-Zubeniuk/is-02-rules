---
description: "Prepare and validate a change before PR (tests, lint, formatting, risks)"
argument-hint: "Scope of changes or files to validate"
---

# /pr-quality-check

Run a pre-PR quality check for this scope:

{{args}}

Checklist:
1. Inspect changed files and summarize behavior impact.
2. Run relevant checks (prefer targeted first, then broader if needed):
   - `yarn test:typecheck`
   - `yarn test:code`
   - `yarn test:other`
   - `yarn test:app`
3. Report failures with likely root cause and minimal fix.
4. Confirm naming/style conventions and error handling expectations.
5. Output:
   - `Status`: ready/not ready
   - `Executed commands`: pass/fail table
   - `Findings`: ordered by severity
   - `Next actions`: exact commands or file edits
