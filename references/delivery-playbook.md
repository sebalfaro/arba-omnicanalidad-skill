# Delivery playbook

Use the smallest set of phases that covers the task. A phase becomes mandatory when its trigger applies.

| Phase | Skill | Trigger | Exit gate |
|---|---|---|---|
| Architecture | `codebase-design` | New module seam, responsibility split, state model, or cross-component behavior | One clear authority per decision and interfaces deep enough to hide implementation detail |
| Domain decisions | `domain-modeling` | New terminology, changed invariant, durable architecture decision, ADR or `CONTEXT.md` update | Terms and decisions are explicit, consistent, and recorded once |
| Test strategy | `tdd` | Feature implementation, behavioral bug fix, integration boundary, or regression-prone change | A failing test demonstrates the missing behavior before production code changes |
| Implementation | `tdd` | A slice has testable acceptance behavior | Red-green-refactor completed; focused tests and required static checks pass |
| Diagnosis | `diagnosing-bugs` | Failure cause is unclear, flaky, performance-related, or possibly preexisting | Root cause is demonstrated with evidence before broadening the patch |
| Prototype | `prototype` | A design choice cannot be resolved cheaply from existing code or reasoning | Throwaway result answers the named design question; prototype code is not silently promoted to production |
| Consolidation | `code-simplifier` | Green implementation contains duplicated guards, branching, or incidental complexity | Exact behavior preserved with a smaller, clearer decision surface |
| Manual E2E | `arba-omni` | User-facing feature has acceptance flows requiring manual validation | Guide exists; every applicable case is approved, paused, blocked with cause, or explicitly out of scope |
| Incremental review | `requesting-code-review` | A meaningful implementation block is complete or the user requests iterative review | Critical findings fixed; Important findings fixed or technically disproven; iteration cap respected |
| Final review | `code-review` | Branch/WIP is being prepared for merge and a fixed comparison point exists | Standards and Spec axes both have explicit verdicts |

## Recommended sequence

1. Architecture before moving or inventing seams.
2. Domain modeling only for decisions worth persisting.
3. TDD per independently verifiable slice.
4. Diagnosis whenever evidence does not yet identify the cause.
5. Simplification after green, when measurable duplication or complexity exists.
6. Manual E2E after implementation and regression checks, followed by user approval.
7. Incremental review after each meaningful block and final Standards+Spec review before staging or merge.

## Plan checklist

A delivery plan accounts for every applicable item:

- outcome and users affected;
- invariants and explicit exclusions;
- current baseline and dirty-worktree boundary;
- implementation slices and dependencies;
- acceptance matrix, including unaffected roles or consumers;
- unit, integration, realtime/concurrency, and E2E coverage as relevant;
- compatibility, rollout, observability, and rollback when risk warrants them;
- known unrelated failures tracked separately;
- Definition of Done with checkable gates;
- skill map for the phases selected above.

## Review loop

Give a reviewer a fixed base, the implementation state or head, the requirements, and read-only instructions. Keep review context independent from implementation reasoning.

For every round:

1. Triage findings by actual severity.
2. Fix Critical and Important findings within scope.
3. Re-run the narrowest verification that proves the fix, followed by proportional regression checks.
4. For High/Critical findings affecting architecture, persistence, authorization, contracts, events, concurrency, or shared state, reassess the approach and record the selected alternative before applying the fix.
5. Request another round only when findings caused material changes and the iteration cap allows it.

When feedback comes from another agent, validate each finding before changing code. Classify false positives with evidence, analyze valid findings and alternatives, obtain user approval for the proposed fix, and obtain separate approval before starting a new review round with local tools.

Stop when the reviewer returns no unresolved Critical or Important findings, the cap is reached, or continuation requires new authority. Report Minor findings and unrelated suite failures without hiding them.

## Artifact and staging rules

- Keep pre-analysis, architectural and implementation decisions, manual E2E results, and required artifacts in both `_evo-output/` and `docs/features/`.
- Verify matching contents before staging; stage only the `docs/features/` copies.
- Keep E2E test code, harnesses, and configuration outside commits unless the user explicitly requests them.
- Stage implementation files inside their owning submodule and feature documentation in the monorepo root.
- Report every staged file, its repository or submodule, and E2E files intentionally left unstaged.

## Skill resolution

Resolve referenced skills from the active skill catalog by name. Do not rely on
machine-specific filesystem paths. If a referenced skill is unavailable, retain
its phase and exit gate using the best available workflow; do not silently drop
the phase.
