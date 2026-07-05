---
type: prompt
status: active
area: coding
stage: planning
created: 2026-06-18
---

# Production Risk Review

## Use when

An implementation plan exists but coding has not started, especially for production applications or risky migrations.

## Prompt

```text
Review the proposed implementation plan as a senior production engineer.

Begin in read-only mode. Inspect the current branch, relevant surrounding code, repository instructions, tests, deployment configuration, and operational conventions. Evaluate only risks grounded in the proposed change and actual system context.

Focus especially on:

- Authentication and authorization
- Payments, pricing, refunds, idempotency, and webhook handling
- Sensitive-data collection, storage, exposure, and retention
- Database migrations, data integrity, and backward compatibility
- Concurrency, retries, duplicate requests, and partial failure
- External integrations and degraded dependencies
- Configuration, secrets, deployment, and rollback
- Observability, auditability, and incident recovery

Rank material risks from most to least serious. For each risk, provide:

- Severity
- Likelihood and impact
- Affected plan step and system
- Concrete failure mode
- Evidence or reasoning
- Preventive mitigation
- Verification or test required
- Rollback or recovery strategy
- Residual risk after mitigation

Clearly distinguish confirmed risks from assumptions requiring validation. Do not inflate the plan with speculative complexity.

Then present a revised implementation plan that incorporates proportionate mitigations, verification checkpoints, and rollback considerations.

Do not modify code or configuration. Show the findings and revised plan, then wait for my approval before implementation.
```

## Notes

For small, reversible changes, the correct result may be a short risk list. Rigor is useful; ceremony for its own sake is not.

Return to [[../Vibe Coding Checklist|Vibe Coding Checklist]].
