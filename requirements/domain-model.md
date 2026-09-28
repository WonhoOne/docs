# Domain Model v0.1

이 문서는 프로젝트 전 영역에서 사용하는 공통 용어의 의미를 정의합니다.

| Term | Meaning | Status |
| --- | --- | --- |
| ThemeTour | 허니문 낭만, 효도 힐링, 골프 챌린지, 아웃도어 트레킹 중 하나의 여행 테마 | Fixed |
| TourStyle | Classic, Grand, Premium 중 하나의 여행 등급 | Fixed |
| TourConfiguration | Theme, Style, Hotel, Transport, Meal 및 추가 옵션을 조합한 고객의 최종 여행 구성 | Fixed |
| Customer | 여행상품을 조회하고 신청하는 고객 사용자 | Fixed |
| Employee | 여행상품과 재고를 관리하는 직원 사용자 | Fixed |
| Reservation | 특정 여행 일정 및 구성으로 고객이 여행 참가를 신청한 기록 | Fixed |
| Inventory | 커플티, 인삼식품, 골프공, 스카프 등 여행상품 제공에 필요한 물품 재고 | Fixed |
| TourSchedule | 특정 Tour의 출발 기간, 모집 및 확정 상태를 표현하는 개념 | Proposed |
| TravelHistory | 완료된 고객 여행 기록/조회 개념. 별도 Entity 여부는 미확정 | Concept fixed / persistence TBD |

## Recommended enum names

공유 문서와 API에서 사용할 후보 이름이며 실제 코드 enum 확정은 API/ERD 상세화 과정에서 결정합니다.

```text
ThemeTour:
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

`TourStyle`은 기본 구성을 제공하며 `TourConfiguration`과 동일한 개념이 아닙니다. 고객은 Style 선택 이후 Hotel, Transport, Meal 등을 변경할 수 있습니다.
