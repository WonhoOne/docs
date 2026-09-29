# Development Baseline v0.1.1

- Status: **Proposal** until merged into `docs/main`; approved implementation baseline after merge.
- Previous approved baseline: [v0.1](BASELINE-v0.1.md), retained as historical record.
- Scope: the decisions below refine v0.1; unlisted requirements and TBD items are not decided by this proposal.

## Fixed in v0.1.1

- The four `Theme` values and the `CLASSIC`, `GRAND`, `PREMIUM` Tour Style catalog, with the default and theme-specific offerings in [Product Catalog](../requirements/product-catalog.md).
- `Theme`, `TourProduct`, `TourSchedule`, `TourStyle`, `TourConfiguration`, `Reservation`, and `Inventory` have distinct meanings in the [Domain Model](../requirements/domain-model.md). `ThemeTour` is renamed to `Theme` in shared terminology.
- A Reservation has an integer `participantCount` of at least 1. Schedule enrollment is the sum of its Reservations' `participantCount` values. Schedules other than `HONEYMOON_ROMANCE` confirm at 3 or more participants; `HONEYMOON_ROMANCE` confirms at 4 or more.
- On the first confirmation of a TourSchedule, SMS is actually sent to its applicant customers. A production/demo path must use a real SMS provider/API; its provider is TBD.
- Customer and Employee signup stores at least `name`, `address`, and `contact`. Logged-in Customer Travel History is most-recent-first and shows product, period, Tour Style, and price.
- Loyalty Discount is a functional requirement; its eligibility and discount rules remain TBD.
- The existing REST API resource naming, endpoint paths, and HTTP methods in [API Spec Draft](../api/api-spec-draft.md) are fixed. Request/response and other details remain for v0.2.
- [Non-functional Requirements](../requirements/non-functional-requirements.md) define the evaluation targets.
- The architecture, repository ownership, limited-command Voice direction, and Git/PR/Agent rules from v0.1 continue to apply.

## Still TBD

- Authentication method and JWT versus session.
- Customer/Employee persistence as User + Role versus separate models.
- Price calculation formula; Loyalty customer criteria, rate, application timing, and stacking.
- Detailed Hotel, Transport, and Meal option lists.
- Reservation status enum and travel cancellation feature.
- Whether TravelHistory needs a separate table.
- SMS provider and its API; Inventory deduction timing and detailed inventory history.
- Final Voice command list and parameters.
- v0.2 API request DTO, response DTO, error format, validation details, pagination, and authentication token format; detailed ERD columns and relationships.

## Contract precedence and change policy

Only the version merged into `docs/main` is approved. Before this proposal merges, implementation uses the approved `docs/main` baseline. After merge, use this baseline with the linked requirements, business rules, domain, API, and ERD documents. Surface conflicts or gaps as docs change proposals before changing implementation; do not resolve TBD by assumption.
