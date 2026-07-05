---
type: prompt
status: active
area: coding
stage: review
created: 2026-06-18
---

# Engineering Review

## Use when

Implementation and initial tests are complete. A fresh thread or reviewer context is preferable when practical.

## Prompt

```text
Act as a senior software engineer and thoroughly review the current branch against its base branch.

Begin in read-only mode. Determine the correct base branch, inspect the full branch diff, and read enough surrounding code, tests, schemas, and configuration to understand the changed behavior. Do not limit the review to superficial style comments.

Review for:

- Incorrect, inconsistent, or incomplete logic
- Edge cases and inadequate error handling
- Data-integrity, transaction, retry, and concurrency problems
- Authentication, authorization, tenancy, and ownership mistakes
- Payment, pricing, and sensitive-data risks
- Regressions, compatibility issues, and unsafe migrations
- Performance or resource problems
- Misuse of framework or library behavior
- Unnecessary complexity and maintainability problems
- Missing, brittle, or misleading tests
- Violations of repository conventions or the approved plan

Do not modify code initially.

Rank actionable findings from most to least critical. For every finding, include:

- Severity
- File and precise location
- Supporting code evidence
- Triggering conditions
- User or production impact
- Minimal recommended fix
- Test or verification needed

Clearly distinguish confirmed defects from plausible risks, questions, and optional improvements. Do not invent findings to fill a quota. If no actionable defects are found, say so and identify any meaningful verification gaps.

After reporting, propose a minimal repair sequence and wait for my approval before editing files.
```

## Notes

- Review findings first; fixes come second.
- In Codex, the built-in `/review` workflow can provide a useful independent pass over branch changes.

Return to [[../Vibe Coding Checklist|Vibe Coding Checklist]].
