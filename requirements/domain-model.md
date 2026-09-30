# Domain Model v0.2

이 문서는 프로젝트 전 영역에서 사용하는 공통 용어의 의미를 정의합니다. Shared concept/enum은 persistence Entity와 구분합니다.

| Term | Meaning | Status |
| --- | --- | --- |
| Theme | 고정 분류 `HONEYMOON_ROMANCE`, `PARENTS_HEALING`, `GOLF_CHALLENGE`, `OUTDOOR_TREKKING` | Fixed enum |
| TourProduct | Employee가 관리하는 실제 여행상품. 정확히 하나의 Theme에 속하고 이름, 설명 및 허용 Style별 가격을 가짐 | Fixed concept; persistence mapping local |
| TourSchedule | 특정 TourProduct의 실제 출발 일정. 기간, 예약 가능 여부, 모집 projection과 확정 여부를 가짐 | Fixed concept; detailed persistence local |
| TourStyle | `CLASSIC`, `GRAND`, `PREMIUM` 중 하나이며 초기 Hotel/Meal 구성을 결정 | Fixed enum |
| TourConfiguration | Reservation에 선택한 `style`, `hotelOption`, `transportOption`, `mealOption`, `extraOptions` | Fixed value concept |
| Customer | 상품을 조회하고 예약하는 사용자. public signup 최소 정보는 `loginId`, `password`, `name`, `address`, `contact` | Fixed role/concept |
| Employee | TourProduct와 Inventory를 관리하는 사용자. 저장 최소 정보는 `name`, `address`, `contact`; v0.2 public Employee signup 없음 | Fixed role/concept |
| UserRole | API authorization 역할 `CUSTOMER`, `EMPLOYEE` | Fixed enum |
| Reservation | 인증된 Customer가 Schedule에 신청한 기록. `participantCount` 및 당시 최종 TourConfiguration/price snapshot 포함 | Fixed concept; status/cancel contract 없음 |
| RecruitmentUnit | 모집 집계 단위 `PARTICIPANT` 또는 `COUPLE_TEAM` | Fixed enum |
| Inventory | item type별 현재 재고 aggregate. `id`, `itemType`, `quantity` | Fixed concept; one aggregate per type |
| TravelHistory | 완료된 Reservation 여행 정보를 제공하는 Customer query/projection 개념 | Fixed; 별도 Entity/Table 불필요 |
| Price | `unitPrice`, `subtotal`, `discount`, `total`, `currency`의 Reservation snapshot | Fixed value concept |

## Shared value catalog

```text
Theme: HONEYMOON_ROMANCE, PARENTS_HEALING, GOLF_CHALLENGE, OUTDOOR_TREKKING
TourStyle: CLASSIC, GRAND, PREMIUM
UserRole: CUSTOMER, EMPLOYEE
RecruitmentUnit: PARTICIPANT, COUPLE_TEAM
HotelOption: HOTEL_3_STAR, HOTEL_4_STAR, HOTEL_5_STAR
MealOption: LUNCH_BOX, LOCAL_RESTAURANT, PREMIUM_RESTAURANT
TransportOption: PRIVATE_LUXURY_CAR_2, PREMIUM_VAN_10
ExtraOption: CHAMPAGNE, COFFEE
InventoryItemType: COUPLE_TSHIRT, GINSENG_GIFT, GOLF_BALL, SCARF
```

REST API identifier (`id`, `tourId`, `scheduleId`, `reservationId`)는 양의 정수입니다. `tourId`는 `TourProduct.id`입니다.

## Relationship and semantic notes

- `Theme 1:N TourProduct`; Theme당 TourProduct가 0개일 수 있고 각 TourProduct는 정확히 하나의 Theme를 가집니다. Theme은 별도 REST resource가 아닙니다.
- `TourProduct 1:N TourSchedule`; 한 Schedule은 하나의 TourProduct에 속합니다.
- Customer는 여러 Reservation을 만들 수 있고 각 Reservation은 한 Customer와 한 TourSchedule에 연결됩니다.
- `TourStyle`은 기본 구성이며 `TourConfiguration`과 다릅니다. Reservation의 configuration은 예약 당시 최종 선택 snapshot입니다.
- 일반 Reservation의 `participantCount`는 1..10 정수입니다. Honeymoon은 2, 4, 6, 8, 10이며 `coupleCount = participantCount / 2`로 계산합니다. `coupleCount`는 파생 값이며 Couple/Team Entity를 도입하지 않습니다.
- `TourSchedule.recruitment`는 `unit`, `currentCount`, `requiredCount`, `confirmed`를 포함합니다. 일반 단위는 `PARTICIPANT`(확정 기준 3), Honeymoon 단위는 `COUPLE_TEAM`(확정 기준 2)입니다. Backend가 제공한 값을 Client가 재계산하지 않습니다.
- `reservable`은 현재 새 Reservation을 받을 수 있는지 나타내며 `confirmed`와 별개입니다.
- `TourProduct`는 허용된 TourStyle별 1인 `unitPrice`를 가지며 Reservation은 계산 당시 가격 snapshot을 보존합니다.
- TravelHistory는 confirmed Schedule의 `endDate`가 Backend business date보다 이전인 Reservation에서 제공됩니다. 과거 표시 의미는 이후 TourProduct 수정으로 바뀌지 않아야 합니다. 구체 snapshot/persistence 구현은 Backend-local입니다.
- Inventory는 품목별 aggregate 한 건입니다. `quantity`는 0 이상의 현재고이며 API의 add 요청은 양수만 받습니다.

세부 규칙과 wire field는 [Business Rules](business-rules.md)와 [REST API Contract](../api/api-spec-draft.md)를 따릅니다.
