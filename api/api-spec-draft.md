# REST API Contract v0.2

> Status: v0.2 shared contract proposal. `docs/main` remains the approved SSOT until this change is merged. The filename and existing endpoint paths are retained because other repositories reference this path.

## 1. General conventions

- Base path: `/api/v1`
- Request and response bodies use `application/json` unless a response is empty.
- IDs (`id`, `tourId`, `scheduleId`, `reservationId`) are positive integers. `tourId` identifies `TourProduct.id`.
- Dates use ISO-8601 calendar date format `YYYY-MM-DD`.
- Currency is `KRW`; monetary amounts are integer KRW.
- Collection responses are plain JSON arrays. No envelope or pagination metadata in v0.2. Empty results return `200 OK` and `[]`.
- Collections use deterministic ordering described below.
- Authenticated requests send `Authorization: Bearer <accessToken>`.
- Backend is the final authority for authentication, business validation, schedule state, configuration, and price.

### Roles and access

| Access | Endpoints |
| --- | --- |
| Public | signup, login, `GET /tours`, `GET /tours/{tourId}`, `GET /tour-schedules`, `GET /tour-schedules/{scheduleId}` |
| CUSTOMER | `POST /reservations`, `GET /reservations/{reservationId}` (own only), `GET /customers/me/travel-history` |
| EMPLOYEE | `/employee/tours` and `/employee/inventory` endpoints below |

Missing, invalid, or expired access token returns 401. Authenticated callers lacking the required role return 403. CUSTOMER reading another Customer's reservation receives 404 `RESERVATION_NOT_FOUND` so resource existence is not exposed.

## 2. Authentication

Authentication uses JWT Bearer Access Token. v0.2 has no Refresh Token, refresh endpoint, or logout endpoint. Public signup always creates `CUSTOMER`; it has no role input. EMPLOYEE accounts are provisioned outside public signup; the provisioning mechanism is Backend-local. `contact`/email is not a login identifier.

### `POST /api/v1/auth/signup`

Request:

```json
{
  "loginId": "customer01",
  "password": "example-password",
  "name": "홍길동",
  "address": "서울시",
  "contact": "010-0000-0000"
}
```

All five fields are required strings. Success is `201 Created`; signup does not issue an access token.

Response:

```json
{
  "id": 101,
  "role": "CUSTOMER",
  "name": "홍길동"
}
```

### `POST /api/v1/auth/login`

Request:

```json
{
  "loginId": "customer01",
  "password": "example-password"
}
```

Success is `200 OK`.

Response:

```json
{
  "accessToken": "<accessToken>",
  "tokenType": "Bearer",
  "expiresIn": 3600,
  "user": {
    "id": 101,
    "role": "CUSTOMER",
    "name": "홍길동"
  }
}
```

`expiresIn` is the remaining Access Token lifetime in seconds. The numeric lifetime is Backend-local configuration. Password is never returned. Token storage on Frontend/AI Console is repository-local.

## 3. TourProduct

`tours` in these existing paths names the `TourProduct` resource. A Theme is a classification, not a product; each product has exactly one Theme and a Theme may have zero products. There is no Theme endpoint.

TourProduct representation:

```json
{
  "id": 201,
  "theme": "GOLF_CHALLENGE",
  "name": "제주 골프 여행",
  "description": "골프 리조트 여행",
  "availableStyles": ["CLASSIC", "GRAND", "PREMIUM"],
  "stylePrices": [
    {"style": "CLASSIC", "amount": 1200000, "currency": "KRW"},
    {"style": "GRAND", "amount": 1800000, "currency": "KRW"},
    {"style": "PREMIUM", "amount": 2500000, "currency": "KRW"}
  ]
}
```

`availableStyles` is computed by Backend from Theme rules and is response-only. `stylePrices` contains exactly one positive integer KRW price for each allowed style and no disallowed style. Employee writes use the same `{ "style", "amount", "currency" }` entry shape, with `currency` equal to `KRW`. Duplicate styles are invalid.

Allowed styles:

| Theme | Styles |
| --- | --- |
| `HONEYMOON_ROMANCE` | `GRAND`, `PREMIUM` |
| `PARENTS_HEALING` | `GRAND`, `PREMIUM` |
| `GOLF_CHALLENGE` | `CLASSIC`, `GRAND`, `PREMIUM` |
| `OUTDOOR_TREKKING` | `CLASSIC`, `GRAND`, `PREMIUM` |

### Public read

- `GET /api/v1/tours` — public, `200` and array ordered by `id ASC`.
- `GET /api/v1/tours/{tourId}` — public, `200` with TourProduct or `404 TOUR_PRODUCT_NOT_FOUND`.

TourProduct response does not contain Schedule, participant/recruitment state, Reservation, configuration, image asset, destination, duration, or lifecycle fields. Editorial/image assets and customer routes remain Frontend-local.

### Employee write/read

- `GET /api/v1/employee/tours` — EMPLOYEE, array ordered by `id ASC`.
- `POST /api/v1/employee/tours` — EMPLOYEE; request fields: `theme`, `name`, `description`, `stylePrices`; success `201 Created` with TourProduct representation.
- `PUT /api/v1/employee/tours/{tourId}` — EMPLOYEE; same request fields; success `200 OK` with updated TourProduct representation.

Example write request:

```json
{
  "theme": "GOLF_CHALLENGE",
  "name": "제주 골프 여행",
  "description": "골프 리조트 여행",
  "stylePrices": [
    {"style": "CLASSIC", "amount": 1200000, "currency": "KRW"},
    {"style": "GRAND", "amount": 1800000, "currency": "KRW"},
    {"style": "PREMIUM", "amount": 2500000, "currency": "KRW"}
  ]
}
```

Do not send `availableStyles` in a write request.

## 4. TourSchedule

Representation:

```json
{
  "id": 501,
  "tourId": 201,
  "startDate": "2026-11-10",
  "endDate": "2026-11-14",
  "reservable": true,
  "recruitment": {
    "unit": "PARTICIPANT",
    "currentCount": 2,
    "requiredCount": 3,
    "confirmed": false
  }
}
```

Dates are calendar dates and `startDate <= endDate`. `recruitment.unit` is `PARTICIPANT` for non-Honeymoon schedules and `COUPLE_TEAM` for `HONEYMOON_ROMANCE`. `currentCount` is the aggregate in that unit; `requiredCount` is 3 participants or 2 couples/teams respectively. `confirmed` is Backend's schedule confirmation truth. `reservable` is Backend's current decision whether a new Reservation can be submitted and is distinct from `confirmed`. Client does not derive Honeymoon recruitment by dividing participants.

- `GET /api/v1/tour-schedules` — public; no query returns all customer-visible schedules, ordered `startDate ASC, id ASC`.
- `GET /api/v1/tour-schedules?tourId={tourProductId}` — the only approved optional query. `tourId` must be a positive integer. A valid ID with no matching schedules returns `200 []`; malformed or invalid value returns `400 INVALID_QUERY_PARAMETER`.
- `GET /api/v1/tour-schedules/{scheduleId}` — public; same representation as a collection item; not found returns `404 TOUR_SCHEDULE_NOT_FOUND`.

v0.2 does not define a `CLOSED`/`CANCELLED` Schedule status enum or a duplicate public `totalParticipantCount` field. There are no TourSchedule create/update/delete endpoints. Initial/demo schedule provisioning is outside the public API and implementation-local.

## 5. TourConfiguration and options

Reservation `configuration` has this shape:

```json
{
  "style": "GRAND",
  "hotelOption": "HOTEL_4_STAR",
  "transportOption": "PRIVATE_LUXURY_CAR_2",
  "mealOption": "LOCAL_RESTAURANT",
  "extraOptions": []
}
```

Canonical IDs:

- Hotel: `HOTEL_3_STAR`, `HOTEL_4_STAR`, `HOTEL_5_STAR`
- Transport: `PRIVATE_LUXURY_CAR_2` (capacity 2), `PREMIUM_VAN_10` (capacity 10)
- Meal: `LUNCH_BOX`, `LOCAL_RESTAURANT`, `PREMIUM_RESTAURANT`
- Extra: `CHAMPAGNE`, `COFFEE`

Theme defaults: Honeymoon uses `PRIVATE_LUXURY_CAR_2`; Parents/Golf/Outdoor use `PREMIUM_VAN_10`. Style defaults are in [Product Catalog](../requirements/product-catalog.md). `PREMIUM` defaults to `CHAMPAGNE` selected, but the Customer may remove it. Extras are an unordered unique selection set; duplicates are invalid. The selected Transport capacity must be at least `participantCount`. Multiple-vehicle assignment, option CRUD/catalog endpoints, inventory availability linkage, and option price deltas are not in v0.2.

## 6. Reservation create and detail

Reservation is owned by the authenticated Customer. Backend resolves `scheduleId → TourSchedule → TourProduct → Theme` and validates role, existence, `reservable`, party size, Theme/Style, canonical options, capacity, extra uniqueness, full configuration, and price.

### `POST /api/v1/reservations`

CUSTOMER only. Request:

```json
{
  "scheduleId": 501,
  "participantCount": 2,
  "configuration": {
    "style": "GRAND",
    "hotelOption": "HOTEL_4_STAR",
    "transportOption": "PRIVATE_LUXURY_CAR_2",
    "mealOption": "LOCAL_RESTAURANT",
    "extraOptions": []
  }
}
```

`participantCount` is an integer 1..10. Honeymoon allows only 2, 4, 6, 8, 10. Request does not include `customerId`, `tourId`, `theme`, `price`, `discount`, applicant contact fields, or `coupleCount`. Success is `201 Created`; when available, `Location` is `/api/v1/reservations/{id}`. Reservation mutations have no automatic retry; v0.2 has no `Idempotency-Key` contract.

### Reservation representation

POST success body and `GET /api/v1/reservations/{reservationId}` detail use the same representation:

```json
{
  "id": 801,
  "participantCount": 2,
  "tourProduct": {
    "id": 201,
    "theme": "HONEYMOON_ROMANCE",
    "name": "제주 허니문"
  },
  "schedule": {
    "id": 501,
    "startDate": "2026-11-10",
    "endDate": "2026-11-14",
    "recruitment": {
      "unit": "COUPLE_TEAM",
      "currentCount": 1,
      "requiredCount": 2,
      "confirmed": false
    }
  },
  "configuration": {
    "style": "GRAND",
    "hotelOption": "HOTEL_4_STAR",
    "transportOption": "PRIVATE_LUXURY_CAR_2",
    "mealOption": "LOCAL_RESTAURANT",
    "extraOptions": []
  },
  "price": {
    "unitPrice": 1800000,
    "subtotal": 3600000,
    "discount": {"type": "LOYALTY", "ratePercent": 5, "amount": 180000},
    "total": 3420000,
    "currency": "KRW"
  }
}
```

`schedule.recruitment` uses the D-05 shape and represents the Schedule's current state. In the POST response it reflects the just-created Reservation. `configuration` and `price` are Reservation-time snapshots. `price.discount` is `null` when no discount applies; when applied, its shape is `{ "type": "LOYALTY", "ratePercent": 5, "amount": <integer KRW> }`. `total = subtotal - discount.amount`, or subtotal when discount is null. Honeymoon price uses participants, not couples. Price is finalized by Backend at Reservation creation; future TourProduct price changes do not change it.

- `GET /api/v1/reservations/{reservationId}` — CUSTOMER only; own Reservation returns `200`; absent or another Customer's resource returns `404 RESERVATION_NOT_FOUND`.
- No ReservationStatus field/enum, cancellation/update endpoint, or SMS delivery field is defined in v0.2.

## 7. Travel History

`GET /api/v1/customers/me/travel-history` is CUSTOMER only and returns the authenticated Customer's completed Travel History as a plain array, ordered `endDate DESC, reservationId DESC`. Empty result is `200 []`.

An item is eligible only when its Schedule has `confirmed = true` and `endDate` is earlier than Backend business date. An unconfirmed past Schedule is excluded from History and Loyalty count. Item shape:

```json
{
  "reservationId": 801,
  "tourProduct": {
    "id": 201,
    "theme": "HONEYMOON_ROMANCE",
    "name": "제주 허니문"
  },
  "startDate": "2026-06-10",
  "endDate": "2026-06-14",
  "style": "GRAND",
  "price": {"amount": 3420000, "currency": "KRW"}
}
```

`style` is the reserved `configuration.style`; `price.amount` is the Reservation snapshot `price.total`. Historical product name, Theme, dates, Style, and price meaning remain stable after later TourProduct edits. How Backend persists snapshots is implementation-local; a separate TravelHistory Entity/Table is not required. The API omits image, options, and `participantCount`. `reservationId` does not require a History detail route.

## 8. Inventory

Inventory representation:

```json
{"id": 1, "itemType": "COUPLE_TSHIRT", "quantity": 12}
```

`id` is a positive integer. `quantity` is current stock and an integer >= 0. Canonical `itemType`: `COUPLE_TSHIRT`, `GINSENG_GIFT`, `GOLF_BALL`, `SCARF`; one aggregate record exists per item type.

- `GET /api/v1/employee/inventory` — EMPLOYEE only; plain array ordered `id ASC`; the fixed four item types may all be returned, including quantity 0.
- `POST /api/v1/employee/inventory` — EMPLOYEE only; request `{ "itemType": "COUPLE_TSHIRT", "quantity": 5 }`. Request quantity is a positive integer to add, not a replacement balance. Backend atomically adds it to current quantity. Success is `200 OK` with updated Inventory representation.

No decrement, replacement, negative adjustment, PUT, DELETE, transaction ledger, or automatic Reservation/confirmation stock deduction/blocking is defined in v0.2. Overflow and concurrency implementation are Backend-local.

## 9. SMS confirmation integration

When a Schedule transitions from `confirmed = false` to `true` after a Reservation is applied, one confirmation event occurs. Later Reservations on an already confirmed Schedule do not create another confirmation event. Recipients are all distinct Customers with a Reservation on that Schedule at the transition, including the triggering Customer; each Customer receives at most one send for that event, using their currently stored contact.

The message communicates that departure is confirmed and includes TourProduct name and Schedule `startDate`/`endDate`. It need not include price and excludes credentials/tokens and unnecessary personal data.

Reservation and confirmation business transaction succeeds independently of SMS provider delivery. Provider failure does not roll it back; one recipient failure does not stop other recipients; failed notification is retained in retryable state. Duplicate successful send is minimized by Schedule confirmation event + Customer identity. Exact transaction/outbox/retry/backoff/provider implementation is Backend-local. Final demo/production requires a real SMS Provider/API; mocks are for development/test. No `/sms` endpoint or public delivery status is defined. Clients show confirmation truth and SMS notification expectation, not delivery success.

## 10. Common errors and validation

Every `application/json` API error has at least this body. `fieldErrors` is always an array, including `[]` when no field error applies.

```json
{
  "code": "VALIDATION_FAILED",
  "message": "요청 값이 유효하지 않습니다.",
  "fieldErrors": [
    {"field": "participantCount", "code": "OUT_OF_RANGE", "message": "..."}
  ]
}
```

`code` is stable and machine-readable. `message` is human-readable and must not control client flow. `field` is a request DTO dotted path. `timestamp`, `path`, `requestId`, and `correlationId` are not required. Never expose stack trace, exception class, SQL detail, secrets, or internal implementation detail.

| HTTP | Meaning | Stable codes / examples |
| ---: | --- | --- |
| 400 | Malformed JSON, type/format parse failure, invalid path/query parameter format | `MALFORMED_REQUEST`, `INVALID_QUERY_PARAMETER` |
| 401 | Login credential failure; missing, invalid, or expired token | `LOGIN_FAILED`, `AUTHENTICATION_REQUIRED`, `INVALID_ACCESS_TOKEN`, `ACCESS_TOKEN_EXPIRED` |
| 403 | Authenticated but insufficient role/permission | `FORBIDDEN` |
| 404 | Missing or intentionally hidden resource | `TOUR_PRODUCT_NOT_FOUND`, `TOUR_SCHEDULE_NOT_FOUND`, `RESERVATION_NOT_FOUND` |
| 409 | Valid request conflicts with current state | `SCHEDULE_NOT_RESERVABLE`, `LOGIN_ID_ALREADY_EXISTS` |
| 422 | Syntactically valid but Domain/Business validation failed | `VALIDATION_FAILED` |
| 500 | Unexpected server failure | `INTERNAL_ERROR` |

Minimum `fieldErrors[].code` vocabulary: `REQUIRED`, `INVALID_FORMAT`, `INVALID_VALUE`, `OUT_OF_RANGE`, `DUPLICATE_VALUE`, `NOT_ALLOWED`, `CAPACITY_EXCEEDED`. Add new codes docs-first. Clients use HTTP status + stable `code`, never parse `message`.

## 11. Explicitly unsupported in v0.2

The following are not public endpoints/contracts:

- `/api/v1/themes`
- `/api/v1/options`
- `/api/v1/price` or quote/reprice endpoint
- refresh-token or logout endpoint
- `/api/v1/sms`
- TourSchedule CRUD
- Reservation cancel/update, payment, or refund
- Travel History detail endpoint
- Inventory PUT/DELETE/decrement
- Reservation automatic submit from Voice

Do not add an endpoint or change an approved path/method without a docs-first shared contract change.
