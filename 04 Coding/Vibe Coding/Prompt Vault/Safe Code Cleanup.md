---
type: prompt
status: active
area: coding
stage: cleanup
created: 2026-06-18
---

# Safe Code Cleanup

## Use when

The feature is working and verified. Run cleanup separately from feature implementation so behavior changes remain reviewable.

## Prompt

```text
Review the current branch against its base branch for dead code, duplicate logic, unused components, and unnecessary complexity.

Begin in read-only mode. Preserve existing behavior, public interfaces, framework entry points, configuration-driven behavior, generated-code boundaries, migrations, serialization formats, plugin hooks, reflection, and dynamically referenced code.

Treat code as removable only when there is concrete evidence that it is unused. Search call sites, imports, routes, configuration, tests, build tooling, runtime registration, and repository documentation before proposing deletion.

For each proposed cleanup, provide:

- Category: dead code, duplicate logic, unused component, or unnecessary complexity
- File and precise location
- Concrete evidence that the code is unnecessary
- Expected benefit
- Behavior, compatibility, or deployment risk
- Minimal proposed change
- Tests or verification needed to prove safe removal

Clearly distinguish high-confidence cleanup from uncertain candidates. Do not delete code merely because static search found no direct reference.

Group changes into small, independently reviewable batches. Keep speculative refactoring separate from straightforward removal or deduplication. Establish the relevant test baseline before cleanup and run those checks after each approved batch.

Do not modify files yet. Present the cleanup proposal and wait for my approval.
```

## Notes

“Delete every unused thing” is an attractive nuisance. Dynamic frameworks are full of code that looks unused right up until production asks where it went.

Return to [[../Vibe Coding Checklist|Vibe Coding Checklist]].
