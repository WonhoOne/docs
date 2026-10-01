# Development Baseline v0.2

- Approval status: determined by repository location. This document is a proposal on an unmerged feature branch or PR and becomes the approved successor baseline only when present on `docs/main`.
- Predecessor: [v0.1.2](BASELINE-v0.1.2.md). Baselines v0.1, v0.1.1, and v0.1.2 remain historical records.
- Scope: close the shared API v0.2 contract for Theme/TourProduct identity, authentication, DTOs, party size, configuration, pricing, history, inventory, SMS, errors, collections, and Voice integration.
- Approval rule: `docs/main` is the approved source of truth. A feature branch or unmerged PR remains a proposal.

## Fixed in v0.2

### Domain and reservation

- `Theme` is a fixed classification and `TourProduct` is an Employee-managed product. One Theme may have zero or more products; each product has exactly one Theme. `tourId` identifies `TourProduct.id`; no Theme endpoint is added.
- `participantCount` is an integer from 1 through 10. Honeymoon reservations allow only 2, 4, 6, 8, or 10 participants and derive `coupleCount = participantCount / 2`. General schedules confirm at 3 participants; Honeymoon schedules confirm at 2 couples/teams.
- `TourSchedule` exposes Backend-derived recruitment and reservability. In v0.2 a schedule is reservable only while `startDate` is later than Backend business date; confirmation itself does not close reservations. The only collection filter is `tourId` on the existing schedule collection endpoint.
- `TourConfiguration` uses canonical option IDs. Transport capacity must cover the reservation participant count. Option price deltas and inventory-based availability are not part of v0.2.

### Authentication and REST API

- Authentication uses JWT Bearer Access Tokens. Public signup creates CUSTOMER accounts; EMPLOYEE accounts use Backend-local provisioning. Refresh and logout endpoints are not part of v0.2.
- Existing REST paths and methods remain fixed. Request/response DTOs, stable error codes, HTTP semantics, plain-array collections, and deterministic ordering are defined in the [REST API Contract](../api/api-spec-draft.md).
- Reservation creation takes `scheduleId`, `participantCount`, and `configuration`; authenticated identity supplies the owner. Reservation responses preserve configuration and price snapshots and show current schedule recruitment.

### Price, history, and inventory

- Product price is a per-participant KRW integer for each allowed TourStyle. Reservation subtotal is `unitPrice × participantCount`; Loyalty Discount is 5% for a Customer with at least one previously completed Travel History, rounded down to whole KRW, with no stacking.
- Completed Travel History requires a confirmed schedule whose `endDate` is earlier than Backend business date. History uses the Reservation price snapshot and stable historical product meaning.
- Inventory is one current-stock aggregate per canonical `itemType`. Employee POST adds a positive quantity. Reservation does not deduct stock in v0.2.

### SMS and Voice

- The first `false → true` schedule confirmation triggers one confirmation event for each distinct Customer on the schedule. SMS delivery failure does not roll back Reservation/confirmation; delivery status is not public API data.
- Voice uses version 1 and the canonical command set in the [Voice Contract](../architecture/voice-contract.md). It can update a Reservation draft but cannot submit a Reservation or perform authentication, cancellation, payment, or Employee mutations.

### Implementation boundaries

Shared meanings are fixed in the linked documents. UI routes/default selection, token storage, persistence mapping, JWT library and claims, actual token lifetime, Employee provisioning mechanism, business clock implementation, SMS provider/retry mechanics, STT/parser internals, and physical database details remain repository-local implementation choices. No public endpoints beyond the existing skeleton and the approved `tourId` query are introduced.

## Detailed contracts

- [Requirements](../requirements/requirements.md)
- [Product Catalog](../requirements/product-catalog.md)
- [Domain Model](../requirements/domain-model.md)
- [Business Rules](../requirements/business-rules.md)
- [REST API Contract](../api/api-spec-draft.md)
- [ERD Shared Model](../database/erd-draft.md)
- [Voice Contract](../architecture/voice-contract.md)
