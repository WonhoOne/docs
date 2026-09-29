# Domain Model v0.1.1 (proposal)

> `docs/main`에 병합되기 전까지 이 변경은 proposal입니다.

이 문서는 프로젝트 전 영역에서 사용하는 공통 용어의 의미를 정의합니다.

| Term | Meaning | Status |
| --- | --- | --- |
| Theme | 4가지 여행 테마를 나타내는 분류: `HONEYMOON_ROMANCE`, `PARENTS_HEALING`, `GOLF_CHALLENGE`, `OUTDOOR_TREKKING` | Fixed |
| TourProduct | Employee가 기획·관리하는 실제 여행상품. 하나의 Theme을 가지며 상품명과 기본 정보를 가진다. | Fixed concept; fields TBD |
| TourSchedule | 특정 TourProduct의 실제 출발 일정. 여행 기간, 모집 상태 및 신청 인원 집계의 기준 단위 | Fixed concept; detailed fields TBD |
| TourStyle | `CLASSIC`, `GRAND`, `PREMIUM` 중 하나. Hotel/Meal 등의 기본 구성을 결정한다. | Fixed |
| TourConfiguration | 고객이 선택한 최종 구성: Theme/선택 TourProduct, TourStyle, Hotel, Transport, Meal, 추가 옵션의 선택 결과 | Fixed concept; detailed fields TBD |
| Customer | TourProduct를 조회하고 예약하는 고객. 가입 시 최소 `name`, `address`, `contact`를 저장한다. | Fixed concept; persistence TBD |
| Employee | TourProduct와 Inventory를 관리하는 직원. 가입 시 최소 `name`, `address`, `contact`를 저장한다. | Fixed concept; persistence TBD |
| Reservation | Customer가 특정 TourSchedule에 신청한 기록. 한 건에 1명 이상을 포함할 수 있고 `participantCount`는 1 이상의 정수이다. | Fixed concept; status enum TBD |
| Inventory | 여행상품 제공에 필요한 재고 품목과 현재 수량을 관리한다. 최소 개념은 `id`, `itemType`, `quantity`이다. | Fixed concept; detailed fields TBD |
| TravelHistory | 완료된 고객 여행 기록/조회 개념. 별도 Entity 여부는 미확정 | Concept fixed / persistence TBD |

## Shared values

공통 분류 값은 다음과 같습니다. 실제 코드의 enum 구현 방식은 상세 설계에서 결정합니다.

```text
Theme:
- HONEYMOON_ROMANCE
- PARENTS_HEALING
- GOLF_CHALLENGE
- OUTDOOR_TREKKING

TourStyle:
- CLASSIC
- GRAND
- PREMIUM
```

## Modeling note

`TourStyle`은 기본 구성을 제공하며 `TourConfiguration`과 동일한 개념이 아닙니다. 고객은 Style 선택 이후 Hotel, Transport, Meal 등을 변경할 수 있습니다. 상품별 제공 항목과 Style 기본 구성은 [Product Catalog](product-catalog.md)를 따릅니다.

TourSchedule의 총 신청 인원은 그 일정에 연결된 Reservation들의 `participantCount` 합입니다. `HONEYMOON_ROMANCE`가 아닌 일반 일정은 3명 이상, `HONEYMOON_ROMANCE` 일정은 4명 이상이면 확정됩니다. 여행 취소 기능과 Reservation 취소 상태는 정의하지 않습니다.

로그인한 Customer의 Travel History는 최근 여행 순으로 상품, 기간, Tour Style, 가격을 보여줍니다. 별도 Table 필요 여부는 TBD입니다.

### Migration note

기존 문서의 `ThemeTour`는 공통 Domain 용어에서 `Theme`으로 정리합니다. 기존 `Tour`는 실제 직원 관리 상품을 의미할 때 `TourProduct`로 명확히 합니다. 기존 `InventoryItem`은 v0.1.1 범위에서 `Inventory`로 정리합니다. 향후 SKU 상세, 입출고 이력, 재고 이동 기록이 필요해지면 `InventoryItem` 또는 `InventoryTransaction`으로 분리할 수 있으나, 이번 범위에서는 분리하지 않습니다.
