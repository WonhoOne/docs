# Business Rules v0.2

공통 비즈니스 규칙의 최종 검증 책임은 Backend에 있습니다. Frontend 또는 Voice가 UI 수준에서 같은 제약을 적용하더라도 Backend 검증을 대체하지 않습니다.

| ID | Rule |
| --- | --- |
| BR-01 | Theme은 `HONEYMOON_ROMANCE`, `PARENTS_HEALING`, `GOLF_CHALLENGE`, `OUTDOOR_TREKKING` 4종이다. Theme은 분류이고 TourProduct와 별개다. |
| BR-02 | TourStyle은 `CLASSIC`, `GRAND`, `PREMIUM` 3종이다. |
| BR-03 | `HONEYMOON_ROMANCE`는 `GRAND` 또는 `PREMIUM`만 선택할 수 있다. |
| BR-04 | `PARENTS_HEALING`은 `GRAND` 또는 `PREMIUM`만 선택할 수 있다. |
| BR-05 | 고객은 기본 Tour Style 선택 이후에도 Hotel, Transport, Meal을 변경할 수 있다. canonical ID와 기본 조합은 [Product Catalog](product-catalog.md)를 따른다. |
| BR-06 | `HONEYMOON_ROMANCE`가 아닌 일반 TourSchedule은 Reservation `participantCount` 합이 3명 이상이면 확정된다. |
| BR-07 | Honeymoon Reservation의 `participantCount`는 2 이상의 짝수이며 `coupleCount = participantCount / 2`다. Schedule은 유효 Reservation들의 coupleCount 합이 2 이상이면 확정된다. |
| BR-08 | 고객 본인의 Travel History는 완료 조건을 만족하는 여행을 최근 순으로 제공한다. |
| BR-09 | Travel History item은 상품, 기간, 예약 당시 Tour Style, 가격을 포함한다. 상세 representation은 API Contract를 따른다. |
| BR-10 | Voice Recognition은 미리 정의한 v0.2 canonical command 집합을 대상으로 한다. |
| BR-11 | 음성인식이 실패하더라도 고객은 GUI를 통해 동일 기능을 수행할 수 있어야 한다. |
| BR-12 | 핵심 Business Rule의 최종 유효성 검사는 Backend가 수행한다. |
| BR-13 | 일반 Reservation 한 건의 `participantCount`는 1 이상의 정수다. 모든 Reservation은 10명 이하이며 Honeymoon은 BR-07의 더 강한 제약을 따른다. 한 Reservation은 여러 명 또는 여러 couple/team을 포함할 수 있다. |
| BR-14 | TourSchedule이 신청 인원 임계치에 처음 도달해 `confirmed`가 false에서 true로 바뀔 때 신청 Customer에게 SMS confirmation event를 발생시킨다. 실제 전송/실패 의미는 아래 경계를 따른다. |
| BR-15 | 새 Reservation 생성 전 완료 Travel History가 1건 이상인 Customer는 subtotal의 5% Loyalty Discount 대상이다. 할인은 KRW 원 단위 버림하며 다른 discount와 중복하지 않는다. |
| BR-16 | Customer와 Employee의 API role은 `CUSTOMER`, `EMPLOYEE`다. Public signup은 `loginId`, `password`, `name`, `address`, `contact`를 받고 CUSTOMER만 생성한다. `contact`/email은 login identifier가 아니다. |
| BR-17 | 각 Theme에 속한 TourProduct 수는 0개 이상이며 각 TourProduct는 정확히 하나의 Theme에 속한다. `tourId`는 `TourProduct.id`다. |
| BR-18 | `participantCount`는 integer 1..10이다. Honeymoon 허용 값은 2, 4, 6, 8, 10이다. Schedule confirmation threshold는 Reservation별 maximum과 다른 개념이다. |
| BR-19 | TourSchedule 모집 값은 일반 `PARTICIPANT` 또는 Honeymoon `COUPLE_TEAM`으로 집계한다. Backend가 `currentCount`, `requiredCount`, `confirmed`, `reservable`을 제공한다. |
| BR-20 | 예약 Transport의 capacity는 `participantCount` 이상이어야 한다. 복수 차량 자동 배정은 지원하지 않는다. |
| BR-21 | TourProduct는 허용된 TourStyle별 양의 integer KRW `unitPrice` 하나를 가진다. `subtotal = unitPrice × participantCount`; option 변경 price delta는 0이다. Backend가 Reservation 생성 시 최종 금액을 계산한다. |
| BR-22 | Reservation price는 `unitPrice`, `subtotal`, `discount`, `total`, `currency` snapshot이다. 할인 적용 시 `discount = {type: LOYALTY, ratePercent: 5, amount: ...}`, 미적용 시 `null`; `total = subtotal - discount.amount`(미적용이면 subtotal)이며 `currency = KRW`다. |
| BR-23 | Travel History 및 Loyalty eligibility에 포함되는 완료 여행은 Schedule `confirmed = true`이고 `endDate`가 Backend business date보다 이전인 경우다. History는 `endDate DESC`, 동률이면 `reservationId DESC`다. |
| BR-24 | v0.2 Inventory는 canonical `itemType`당 현재 재고 aggregate 한 건이며 quantity integer >= 0이다. 조회는 fixed catalog 4종을 반환할 수 있다. Employee POST quantity는 추가할 양수이고 기존 current quantity에 더한다. |
| BR-25 | v0.2에는 Inventory decrement/replace/delete, 예약 연동 차감, stock 0에 따른 상품/예약 차단, SKU 또는 거래 이력이 없다. |
| BR-26 | Reservation 생성은 인증된 Customer의 소유로 처리한다. Backend는 권한, Schedule, `reservable`, party size, Theme/Style, 옵션, transport capacity, extras, 전체 구성 및 가격을 최종 검증한다. Request에서 소유자나 가격을 받지 않는다. |
| BR-27 | 최초 Schedule 확정 event는 해당 시점 Schedule의 모든 distinct Customer를 대상으로 한다. SMS provider 실패는 Reservation/확정을 rollback하지 않고, 한 수신자 실패가 다른 수신자 처리를 중단하지 않는다. 실패 notification은 retry 가능한 상태로 보존한다. |
| BR-28 | Voice는 Reservation draft를 만들거나 바꿀 수 있지만 Review 후 사용자가 GUI에서 명시적으로 submit한다. Voice는 자동 Reservation POST, 인증 credential, 취소/환불/결제, Employee mutation을 수행하지 않는다. |
| BR-29 | API collection은 plain JSON array이며 v0.2 pagination/envelope가 없다. stable ordering은 [REST API Contract](../api/api-spec-draft.md)를 따른다. |
| BR-30 | `TourSchedule.reservable`은 현재 새 Reservation을 받을 수 있는지를 나타내는 Backend-derived boolean이다. v0.2에서는 `startDate`가 Backend business date보다 미래일 때만 true이며, 같은 날이거나 과거이면 false다. `confirmed = true` 자체는 예약 접수를 마감하지 않는다. Reservation 생성 시 Backend는 최신 business date를 기준으로 동일 정책을 다시 최종 검증한다. |

## Price and Loyalty

- `unitPrice`는 TourProduct × 허용 TourStyle별 1 participant 가격입니다. Honeymoon도 participant 기준으로 계산합니다.
- `subtotal = unitPrice × participantCount`이며 Hotel/Transport/Meal/Extra 선택에 따른 price delta는 없습니다.
- Returning Customer는 새 Reservation 생성 이전에 완료 Travel History가 1건 이상인 Customer입니다. Loyalty Discount는 subtotal의 5%이며 KRW 정수화 시 소수 원을 버립니다.
- v0.2에서는 다른 할인과 stacking하지 않습니다. Reservation 생성 후 상품 가격 변경이 기존 price snapshot을 바꾸지 않습니다.

## SMS implementation boundary

- 최종 demo/production 경로는 실제 SMS Provider/API 전송을 제공해야 합니다. Console log나 mock만으로는 FR-14를 충족하지 않습니다. 개발/테스트 중 mock은 허용합니다.
- Trigger, recipient deduplication, 최소 메시지 의미, 실패 처리 및 public API 경계는 [REST API Contract](../api/api-spec-draft.md)의 SMS integration section을 따릅니다.
- Provider, SDK, retry/backoff 횟수, outbox/transaction 구현은 Backend-local입니다. secret은 repository에 하드코딩하지 않습니다.

## Important boundaries

- ReservationStatus, Reservation cancellation/change, payment/refund, schedule lifecycle/capacity(수동 모집 마감 및 일정 취소 포함), inventory deduction, option price delta는 v0.2 shared contract에 없습니다.
- `coupleCount`는 파생 집계 의미이며 Couple/Team Entity가 아닙니다.
- UI 초기 선택값, route, API token storage, physical persistence, transaction/locking 및 외부 provider 구현은 각 repository-local입니다.
