# Development Baseline v0.1.2

- Status: **Proposal** until merged into `docs/main`.
- Predecessor: [v0.1.1](BASELINE-v0.1.1.md), the approved implementation baseline on `docs/main` when this proposal was prepared.
- Scope: refine the `HONEYMOON_ROMANCE` recruitment unit from a participant threshold to couple/team semantics. All v0.1.1 decisions not explicitly refined here remain in force, including its TBD items.
- Approval rule: only content merged into `docs/main` is approved. After this document is merged, v0.1.2 becomes the latest approved implementation baseline.

## Fixed by this proposal

### Honeymoon reservation unit

For `HONEYMOON_ROMANCE`, one couple/team consists of 2 participants. Reservations continue to use `participantCount`. Each Honeymoon Reservation must have an integer `participantCount >= 2` that is even.

One Reservation may include multiple couples/teams. For each valid Honeymoon Reservation:

```text
coupleCount = participantCount / 2
```

Examples: `participantCount = 2` represents 1 couple/team; `4` represents 2; and `6` represents 3. Couple/team is a derived recruitment meaning, not a new shared Entity. No `Couple`, `Team`, or equivalent persistence Entity is introduced.

### Honeymoon schedule confirmation

A Honeymoon TourSchedule confirms when its valid Reservations contain at least 2 couples/teams in total. The recruitment total is the sum of each valid Reservation's derived `coupleCount`.

The resulting minimum is numerically 4 participants when all Reservations are valid. The rule means **2 couples/teams**, not any arbitrary combination of 4 participants. For example, Reservations with counts 1 and 3 do not constitute valid Honeymoon input: each Honeymoon Reservation must independently satisfy the even-number pair invariant.

### General schedules

For Themes other than `HONEYMOON_ROMANCE`, confirmation remains at a sum of Reservation `participantCount` values of 3 or more. A general Reservation may contain one or more participants.

## Existing contract and TBD items

All v0.1.1 contracts not explicitly refined above remain unchanged. This proposal does not decide participant-count UI placement, default or maximum; schedule capacity; availability details; participant-count effects on price or option availability; API DTO fields; authentication; detailed prices or Loyalty rules; Reservation status or cancellation; or SMS provider/retry policy. Whether an API response exposes `coupleCount` as a separate field remains for API v0.2.

The existing Theme and TourStyle catalogs, Theme/TourProduct distinction, TourSchedule concept, SMS requirement, Loyalty Discount requirement, minimum Customer/Employee signup data, Travel History behavior, REST paths and methods, Voice direction, NFR targets, and cancellation scope remain as defined by v0.1.1 and its linked documents.
