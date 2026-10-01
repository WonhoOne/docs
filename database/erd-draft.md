# ERD Shared Model v0.2

> Status: Approval follows the Baseline rule: content on `docs/main` is approved SSOT; feature branch or unmerged PR content is a proposal. This document defines Domain relationships and persistence meanings, not Backend's physical JPA schema.

## Logical entities and value concepts

```text
Customer (or User with role=CUSTOMER)
  1 ───────── N Reservation N ───────── 1 TourSchedule N ───────── 1 TourProduct N ───────── 1 Theme
                         1 ───────── 1 TourConfiguration (reservation-time selection)
                         1 ───────── 1 ReservationPriceSnapshot

Employee (or User with role=EMPLOYEE) ── manages ── TourProduct / Inventory

Inventory: one current-stock aggregate per InventoryItemType
TravelHistory: query/projection over eligible Reservation + TourSchedule data; no separate table required
```

`Theme` and `TourStyle` may be fixed value catalogs/enums rather than tables. `TourConfiguration` and `ReservationPriceSnapshot` describe shared logical data and do not force a specific table or embedded-object mapping.

## Relationship and field meaning

- **Theme / TourProduct:** Theme 1:N TourProduct. A Theme can have zero products; every TourProduct has exactly one Theme. TourProduct carries product name/description and one positive integer KRW `unitPrice` for each Theme-allowed TourStyle. `availableStyles` is derived from Theme rules.
- **TourProduct / TourSchedule:** one TourProduct has zero or more schedules; each Schedule references one TourProduct and has `startDate`, `endDate`, and Backend-derived `reservable`/`recruitment` view. Recruitment projection need not be stored as duplicated aggregate columns.
- **Customer / Reservation / TourSchedule:** each Reservation belongs to one Customer and one Schedule. One Customer and one Schedule may each relate to multiple Reservations.
- **Reservation party:** `participantCount` is validated 1..10; Honeymoon is 2, 4, 6, 8, or 10. `coupleCount = participantCount / 2` is derived for Honeymoon recruitment. No Couple/Team entity is introduced.
- **TourConfiguration:** Reservation preserves its final `style`, `hotelOption`, `transportOption`, `mealOption`, and unique `extraOptions` as historical selection meaning.
- **Price snapshot:** Reservation preserves `unitPrice`, `subtotal`, `discount` (or null), `total`, and `currency` as created. A later TourProduct price change does not alter the reservation snapshot.
- **TravelHistory:** eligible when Schedule is confirmed and its `endDate` is before Backend business date. Historical product name, Theme, dates, Style, and price meaning must remain stable. The logical contract does not require `TravelHistory` entity/table; Backend chooses persistence/projection strategy.
- **Employee / Inventory:** API role values are `CUSTOMER` and `EMPLOYEE`. Backend may use separate models or a shared User/Role structure. Inventory has one current aggregate per canonical `itemType`, with `quantity >= 0`.

## Canonical catalog references

- Theme: `HONEYMOON_ROMANCE`, `PARENTS_HEALING`, `GOLF_CHALLENGE`, `OUTDOOR_TREKKING`
- TourStyle: `CLASSIC`, `GRAND`, `PREMIUM`
- HotelOption: `HOTEL_3_STAR`, `HOTEL_4_STAR`, `HOTEL_5_STAR`
- MealOption: `LUNCH_BOX`, `LOCAL_RESTAURANT`, `PREMIUM_RESTAURANT`
- TransportOption: `PRIVATE_LUXURY_CAR_2`, `PREMIUM_VAN_10`
- ExtraOption: `CHAMPAGNE`, `COFFEE`
- InventoryItemType: `COUPLE_TSHIRT`, `GINSENG_GIFT`, `GOLF_BALL`, `SCARF`

See [Product Catalog](../requirements/product-catalog.md) and [REST API Contract](../api/api-spec-draft.md) for canonical meanings and public DTOs.

## Persistence boundaries

- This is not a physical schema prescription: PK/FK representation, columns, nullability, indexes, migrations, and Customer/Employee mapping are Backend-local.
- No JWT/Refresh Token table is required by the shared model. v0.2 has no Refresh Token contract.
- SMS outbox/retry persistence, transaction/locking strategy, and audit/transaction ledger are Backend-local; the shared ERD does not require physical tables for them.
- Inventory decrement, automatic stock deduction, stock-based reservation blocking, and SKU/transaction history are outside v0.2.
- Reservation status/cancellation fields and TravelHistory table are not required by the shared contract.
