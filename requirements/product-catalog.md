# Product Catalog v0.2

> Status: v0.2 shared catalog proposal. It becomes approved only when present on `docs/main` under the Baseline approval rule.

## Theme offerings

| Theme | Included offering |
| --- | --- |
| `HONEYMOON_ROMANCE` | 2인 전용 로맨틱 스페셜 룸 데코레이션; 커플 기념 티셔츠; 2인 전용 고급차량 |
| `PARENTS_HEALING` | 고품격 안마/지압 서비스; 건강 인삼 기념품; 10인 승합 고급차량 |
| `GOLF_CHALLENGE` | 유명 골프 리조트 테마; 골프 액세서리 / 골프공; 10인 승합 고급차량 |
| `OUTDOOR_TREKKING` | 트레킹 / 산악 / 어드벤처 테마; 아웃도어 기념품 스카프; 10인 승합 고급차량 |

Theme은 분류 값이고 실제 직원 관리 상품은 `TourProduct`입니다. 하나의 Theme에는 0개 이상의 TourProduct가 있을 수 있습니다.

## Tour Style defaults and availability

| TourStyle | Default composition |
| --- | --- |
| `CLASSIC` | `HOTEL_3_STAR`; `LUNCH_BOX` |
| `GRAND` | `HOTEL_4_STAR`; `LOCAL_RESTAURANT` |
| `PREMIUM` | `HOTEL_5_STAR`; `PREMIUM_RESTAURANT`; `CHAMPAGNE` 기본 선택 |

`HONEYMOON_ROMANCE`와 `PARENTS_HEALING`은 `GRAND` 또는 `PREMIUM`만 선택할 수 있습니다. `GOLF_CHALLENGE`와 `OUTDOOR_TREKKING`은 세 Style을 모두 허용합니다. 고객은 Style 선택 후 Hotel, Transport, Meal을 변경할 수 있습니다. Premium의 기본 `CHAMPAGNE`는 고객이 해제할 수 있습니다.

## Canonical options

### HotelOption

| ID | Meaning |
| --- | --- |
| `HOTEL_3_STAR` | 3성급 호텔 |
| `HOTEL_4_STAR` | 4성급 호텔 |
| `HOTEL_5_STAR` | 5성급 호텔 |

### MealOption

| ID | Meaning |
| --- | --- |
| `LUNCH_BOX` | 도시락 식사 |
| `LOCAL_RESTAURANT` | 현지식 레스토랑 |
| `PREMIUM_RESTAURANT` | 고급 레스토랑 |

### TransportOption

| ID | Capacity (participants) | Theme default |
| --- | ---: | --- |
| `PRIVATE_LUXURY_CAR_2` | 2 | `HONEYMOON_ROMANCE` |
| `PREMIUM_VAN_10` | 10 | `PARENTS_HEALING`, `GOLF_CHALLENGE`, `OUTDOOR_TREKKING` |

고객은 두 TransportOption 사이에서 변경할 수 있습니다. 선택 차량의 capacity는 해당 Reservation의 `participantCount` 이상이어야 합니다. 여러 대의 차량 자동 배정은 v0.2에 포함하지 않습니다.

### ExtraOption

| ID | Meaning |
| --- | --- |
| `CHAMPAGNE` | 샴페인 |
| `COFFEE` | 커피 |

Extra는 0개 이상 선택할 수 있고 중복 선택할 수 없습니다. REST 값은 표시 문구가 아닌 위 canonical ID입니다.

## Price and inventory boundary

TourProduct별 허용 TourStyle의 1인 가격은 Employee가 관리하는 KRW 정수입니다. Option 변경에는 가격 차액이 없습니다. Inventory 품목 종류와 현재 수량 계약은 [Business Rules](business-rules.md) 및 [Domain Model](domain-model.md)을 따릅니다. Option availability는 Inventory와 연결하지 않습니다.

`/options` endpoint, Option Entity CRUD, 동적 Option catalog는 v0.2에 포함하지 않습니다.
