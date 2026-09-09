---
name: arba-omni
description: Plan and drive substantial feature work through architecture, test strategy, implementation slices, diagnosis, simplification, and code review. Use when starting a new feature or multi-step engineering task; skip for read-only questions and isolated trivial edits.
---

# Arba Omni

Drive feature work through explicit analysis, implementation, validation, review, approval, and handoff gates. Use the [delivery playbook](references/delivery-playbook.md) for phase triggers, skills, and detailed exit gates.

## Start

1. Discover repository instructions, existing artifacts, dirty-worktree boundaries, and the implementation baseline.
2. Classify the task as design-only, implementation-ready, already implemented and needing validation, or trivial/read-only. Apply the full flow to features and substantial fixes only.
3. Create or update one plan artifact covering requirements, invariants, exclusions, slices, dependencies, acceptance scenarios, testing, risks, rollout when relevant, and Definition of Done.
4. For feature work, complete pre-analysis before implementation. Add architecture analysis when persistence, contracts, authorization, realtime, shared state, concurrency, or module boundaries change. Add implementation analysis before coding to define seams, slice order, compatibility, regression coverage, and decisions to record.
5. Before user validation, create the manual E2E guide in the feature documentation. Each case needs priority, requirement, preconditions, steps, expected result, observed result, status, and evidence. Include supported roles, regression-sensitive flows, and paused or out-of-scope behavior.

## Drive

- Keep one source of truth for the plan and update it when scope or decisions change.
- Execute slices through their exit gates and prevent regressions with focused and proportional checks. Separate preexisting failures from feature failures.
- After implementation, every applicable E2E case must be approved, explicitly paused, blocked with cause, or marked out of scope by the user.
- Treat feedback from another agent as unverified: validate each finding, classify false positives with evidence, analyze valid fixes and alternatives, and obtain user approval before changing code.
- If a High/Critical finding affects architecture, persistence, authorization, contracts, events, concurrency, or shared state, reassess the approach before fixing and record the decision.
- After approved review feedback, fix the approved findings, rerun affected verification, and respect any iteration cap.

## Handoff

Follow this order before staging: applicable manual E2E completion → user approval of E2E results → CR → approved fixes and required architectural reassessment → final verification → user's final approval → selective `git add`.

Keep E2E test code, harnesses, and configuration local unless the user explicitly requests them in a commit. Keep pre-analysis, architectural and implementation decisions, manual E2E results, and required artifacts in both `_evo-output/` and `docs/features/`; verify matching contents and stage only the `docs/features/` copies. Stage implementation files inside their owning submodule and documentation in the monorepo root. Do not stage unrelated files or submodule pointers indiscriminately.

At handoff, report the current phase, completed gates, unresolved findings, verification evidence, working-tree state, every staged file with its repository/submodule, and E2E files intentionally left unstaged.

Delivery is complete only when the plan's Definition of Done is satisfied or the user explicitly narrows it.
