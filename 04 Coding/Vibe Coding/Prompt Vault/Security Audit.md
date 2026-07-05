---
type: prompt
status: active
area: coding
stage: security
created: 2026-06-18
---

# Security Audit

## Use when

Reviewing a production change involving authentication, authorization, payments, sensitive information, exposed endpoints, uploads, integrations, or privileged actions.

## Prompt

```text
Act as a senior application-security engineer and audit the current branch against its base branch. Assess only code and systems I own or am authorized to review.

Begin in read-only mode. Inspect the branch diff and enough surrounding code, tests, configuration, schemas, and deployment context to understand the real attack surface.

First construct a concise threat model covering:

- Application entry points and exposed interfaces
- Trust boundaries and attacker-controlled inputs
- Authentication, session, and identity assumptions
- Authorization, tenancy, ownership, and privileged operations
- Payment flows, prices, webhooks, refunds, and idempotency
- Sensitive data, secrets, logging, storage, and retention
- File uploads, parsing, rendering, and outbound requests
- External services and deployment assumptions

Then review for concrete vulnerabilities and security regressions, including broken access control, injection, request forgery, unsafe deserialization or parsing, insecure uploads, secret exposure, session weaknesses, payment manipulation, race conditions, sensitive-data leakage, insecure configuration, dependency exposure, and insufficient security logging.

Do not modify code initially.

Rank findings by exploitability and impact. For each finding, provide:

- Severity
- Affected file and precise location
- Attack path and required conditions
- Supporting code evidence
- Potential confidentiality, integrity, availability, or financial impact
- Confidence level and any proof gap
- Safe validation or reproduction method
- Minimal remediation
- Regression tests required

Separate confirmed vulnerabilities from unverified concerns and defense-in-depth recommendations. Do not claim that the audit found every possible vulnerability, and do not invent findings to fill a quota.

Present a prioritized remediation plan. Wait for my approval before making any changes, and keep approved fixes minimal and separately reviewable.
```

## Notes

For high-risk systems, combine this review with dependency and secret scanning, established security tooling, and qualified human review. AI review is a useful layer, not a magic force field.

Return to [[../Vibe Coding Checklist|Vibe Coding Checklist]].
