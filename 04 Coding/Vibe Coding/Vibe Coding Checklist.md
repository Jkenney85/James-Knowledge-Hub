---
type: checklist
status: active
area: coding
created: 2026-06-18
---

# Vibe Coding Checklist

A proposal-first workflow for building production software with Codex or Claude Code. It is intentionally cautious around authentication, authorization, payments, sensitive information, and irreversible data changes.

## Before starting

- [ ] Work in a Git repository on a dedicated branch.
- [ ] Confirm the correct repository, branch, and base branch.
- [ ] Make sure existing uncommitted work is understood and protected.
- [ ] Identify the goal, relevant context, constraints, and definition of done.
- [ ] Never paste production secrets, private keys, payment credentials, or customer data into a prompt.

## 1. Clarify the requirement

- [ ] Run [[Prompt Vault/Requirements Interview|Requirements Interview]].
- [ ] Confirm the summarized outcome and acceptance criteria.
- [ ] Resolve material questions about scope, architecture, data, UX, and security.
- [ ] Approve the assumptions that remain.

## 2. Build the plan

- [ ] Ask for an implementation plan scoped to the confirmed requirements.
- [ ] Verify that the plan names affected systems, migrations, tests, and deployment considerations.
- [ ] Run [[Prompt Vault/Production Risk Review|Production Risk Review]].
- [ ] Review the revised plan and approve it before implementation.

## 3. Implement safely

- [ ] Keep changes limited to the approved scope.
- [ ] Preserve unrelated user changes in the working tree.
- [ ] Add or update focused tests while implementing.
- [ ] Validate authentication and authorization at the server boundary.
- [ ] Treat prices, permissions, ownership, and payment state as server-controlled.
- [ ] Avoid logging credentials, tokens, payment details, or sensitive personal data.
- [ ] Stop and request approval if implementation requires a material scope or architecture change.

## 4. Verify behavior

- [ ] Run formatting, linting, type checks, and relevant tests.
- [ ] Test expected behavior and important failure paths.
- [ ] Test unauthorized, unauthenticated, malformed, duplicate, and stale requests where relevant.
- [ ] Verify database migrations against realistic data and confirm rollback or recovery.
- [ ] Exercise critical behavior manually when automated tests cannot prove it.
- [ ] Review the current branch diff before accepting the implementation.

## 5. Engineering review

- [ ] Run [[Prompt Vault/Engineering Review|Engineering Review]] against the current branch.
- [ ] Review findings before allowing changes.
- [ ] Approve and apply the minimal repair sequence.
- [ ] Re-run relevant checks after fixes.

## 6. Security review

- [ ] Run [[Prompt Vault/Security Audit|Security Audit]].
- [ ] Review the threat model and evidence behind each finding.
- [ ] Separate confirmed vulnerabilities from unverified concerns and hardening suggestions.
- [ ] Approve security fixes individually or in small related groups.
- [ ] Add regression tests showing that confirmed vulnerabilities no longer reproduce.

> [!important]
> An AI security review complements—but does not replace—dependency scanning, secret scanning, established security tools, penetration testing, or human review for high-risk systems.

## 7. Cleanup separately

- [ ] Run [[Prompt Vault/Safe Code Cleanup|Safe Code Cleanup]] only after functionality is stable.
- [ ] Require concrete evidence before deleting apparently unused code.
- [ ] Keep cleanup batches small and independently reviewable.
- [ ] Run tests before and after each approved cleanup batch.

## 8. Ship readiness

- [ ] Run the full appropriate regression suite.
- [ ] Confirm monitoring, logging, and alerting for critical paths.
- [ ] Confirm deployment steps, configuration, feature flags, and secret handling.
- [ ] Confirm rollback or recovery procedures.
- [ ] Review data migration and backward-compatibility risks.
- [ ] Document known residual risks and follow-up work.
- [ ] Obtain human approval before deploying high-risk changes.

## Useful prompt anatomy

Strong implementation prompts usually include:

- **Goal:** what should change
- **Context:** relevant files, errors, examples, and existing behavior
- **Constraints:** architecture, compatibility, security, and do-not rules
- **Done when:** observable behavior and checks that must pass

Return to [[04 Coding/Coding Dashboard|Coding Dashboard]].
