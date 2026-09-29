# AI Agent Instructions

This repository is the project documentation SSOT. `docs/main` is the approved SSOT. A docs feature branch is a proposal until merged. Implementation Agents must not implement against an unapproved proposal.

## Before any implementation task

Read the latest Baseline present on approved `docs/main` in order. If v0.1.2 is present on `docs/main`, it is the latest approved implementation baseline; a v0.1.2 document on a feature branch is only a proposal until merged. Until then, v0.1.1 remains the approved baseline. Before a proposal is merged, follow the mandatory reading list on the approved `docs/main` branch:

1. The latest approved baseline on `docs/main` (currently `baseline/BASELINE-v0.1.1.md`; use `baseline/BASELINE-v0.1.2.md` after it is merged)
2. `requirements/requirements.md`
3. `requirements/product-catalog.md`
4. `requirements/domain-model.md`
5. `requirements/business-rules.md`
6. `requirements/non-functional-requirements.md`
7. `architecture/system-architecture.md`
8. `architecture/repository-responsibilities.md`
9. `api/api-spec-draft.md`
10. `database/erd-draft.md`
11. `architecture/voice-contract.md` when the task affects Voice.
12. `CONTRIBUTING.md`
13. `AGENTS.md`

## Non-negotiable rules

- Do not invent requirements.
- Do not silently change business rules, public API contracts, shared domain terminology, or DB contracts.
- Do not resolve TBD items by assumption.
- If a shared contract must change, propose the docs change first and state the affected repositories.
- Backend is the final authority for business-rule validation.
- Frontend and Voice/Employee Console must access system data through the Backend API, not directly through MySQL.
- Keep changes scoped to the repository responsibility defined in `architecture/repository-responsibilities.md`.
- **Treat code outside your assigned repository as read-only by default.**
- Cross-repository code may be inspected for understanding, debugging, API verification, and integration analysis, but must not be modified, committed, or included in a PR by that Agent.
- If another repository needs a code change, open or request an Issue/change from that repository's Owner instead of modifying it directly.
- Cross-repository modification is allowed only when the relevant Owner or team explicitly delegates that task.
- Every PR must state the related requirement IDs, affected contracts, and test evidence.

If implementation and documentation conflict, stop and surface the conflict instead of choosing an interpretation silently.
