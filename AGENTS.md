# AI Agent Instructions

This repository is the project documentation SSOT.

## Before any implementation task

Read, in order:

1. `baseline/BASELINE-v0.1.md`
2. `requirements/requirements.md`
3. `requirements/domain-model.md`
4. `requirements/business-rules.md`
5. `architecture/system-architecture.md`
6. `architecture/repository-responsibilities.md`
7. `api/api-spec-draft.md`
8. `database/erd-draft.md`
9. `architecture/voice-contract.md` when the task affects Voice.

## Non-negotiable rules

- Do not invent requirements.
- Do not silently change business rules, public API contracts, shared domain terminology, or DB contracts.
- Do not resolve TBD items by assumption.
- If a shared contract must change, propose the docs change first and state the affected repositories.
- Backend is the final authority for business-rule validation.
- Frontend and Voice/Employee Console must access system data through the Backend API, not directly through MySQL.
- Keep changes scoped to the repository responsibility defined in `architecture/repository-responsibilities.md`.
- Every PR must state the related requirement IDs, affected contracts, and test evidence.

If implementation and documentation conflict, stop and surface the conflict instead of choosing an interpretation silently.
