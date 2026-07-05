---
type: prompt
status: active
area: coding
stage: requirements
created: 2026-06-18
---

# Requirements Interview

## Use when

The idea is still fuzzy, requirements could materially affect architecture or security, or planning too early would create rework.

## Prompt

```text
Before creating an implementation plan, inspect the repository, its instructions, relevant code, existing tests, and established conventions.

Ask focused clarifying questions until no unresolved ambiguity would materially change the scope, architecture, data model, user experience, security requirements, deployment approach, or acceptance criteria.

Do not use or claim a numerical confidence score. Do not ask questions whose answers can be safely discovered from the repository. If an uncertainty is low-risk and reversible, state the assumption you recommend instead of blocking progress.

Pay particular attention to authentication, authorization, payments, sensitive information, data ownership, migrations, external integrations, compatibility requirements, and destructive or irreversible operations.

Before planning, provide:

1. Your understanding of the requested outcome
2. Explicit acceptance criteria
3. Confirmed constraints
4. Relevant existing behavior and conventions discovered in the repository
5. Assumptions you propose making
6. Remaining uncertainties and their impact
7. Anything explicitly out of scope

Wait for my confirmation. Do not create the implementation plan or modify files until I approve this understanding.
```

## Notes

- This replaces an artificial “96% confidence” threshold with a concrete decision rule.
- Answer the questions with business rules and observable outcomes—not implementation guesses unless you have a real constraint.

Return to [[../Vibe Coding Checklist|Vibe Coding Checklist]].
