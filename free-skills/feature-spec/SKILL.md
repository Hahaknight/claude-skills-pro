---
name: feature-spec
description: >
  MUST USE before implementing any non-trivial feature (new endpoint, new
  module, new user-facing behavior) or when the user describes a feature
  vaguely — 先出方案/需求不清/design first. Converts vague asks into a
  1-page spec with scope, contracts, edge cases, and test scenarios —
  getting alignment BEFORE code is written.
  Free sample of claude-skills-pro - 8 more Pro skills (security-audit, refactor-surgeon, perf-profiler, api-designer, db-migration-safe) + 11-chapter CN handbook. Buy / free review copy: github.com/Hahaknight/claude-skills-pro/issues/1
---

# Feature Spec — Design Before Code

Rework is the most expensive bug. This skill spends 5 minutes of spec to save hours of wrong implementation.

## Workflow

1. **Restate the ask in one sentence**: who does what, and what changes for them. If you can't, list your questions NOW and stop.
2. **Locate the boundaries** (read the actual code, don't assume):
   - What modules/files will this touch? Name them.
   - Existing patterns for the same kind of feature (find a sibling feature and mirror it — consistency beats creativity inside a codebase)
   - What must NOT change (invariants, public contracts, performance budgets)
3. **Write the one-page spec**:
   - **Goal & non-goals** (explicitly listing what we are NOT doing this round)
   - **Interface**: function signatures / API routes+payloads / DB schema deltas / CLI flags — the contract other code depends on
   - **Behavior table**: input → outcome, including at least 5 edge cases (empty, huge, malformed, unauthorized, concurrent)
   - **Error model**: what can fail and what the user/system sees for each
   - **Test scenarios**: 3–8, derived from the behavior table
   - **Rollout**: feature flag? migration order? backfill needed? rollback path?
4. **Surface the decisions**, don't bury them: list each place you had 2+ reasonable options, your pick, and why (one line each). The user overrules cheaply at spec time, expensively after implementation.
5. **Confirm the spec with the user** when: behavior is user-visible, contracts change, or a migration is involved. For internal-only, self-contained additions, proceed and include the spec in the PR description.

## Output format

A single markdown section, ≤80 lines. If it exceeds that, the feature is too big — propose splitting.

## Anti-patterns

- No architecture-astronaut diagrams. One page, concrete, code-anchored.
- Don't spec what a linter/type system enforces.
- Don't present options without a recommendation — pick one, note the alternative.
- Never invent requirements the user didn't state without marking them `ASSUMPTION:` for review.
---

*(Part of [claude-skills-pro](https://github.com/Hahaknight/claude-skills-pro) — 7 free/MIT + 8 Pro skills: security-audit, refactor-surgeon, perf-profiler, api-designer, db-migration-safe + 11-chapter CN handbook. Upgrade or grab a free review copy: [issue #1](https://github.com/Hahaknight/claude-skills-pro/issues/1).)*
