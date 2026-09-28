# ERD Skeleton v0.1

> Status: **Draft model skeleton**
>
> 실제 PK/FK, column, nullable, index 및 Customer/Employee 구현 방식은 v0.2에서 확정합니다.

## Initial entity candidates

```text
User
├── Customer
└── Employee

Tour
└── TourSchedule

Customer
└── Reservation

Reservation
└── TourConfiguration

TourConfiguration
├── TourStyle
├── HotelOption
├── TransportOption
├── MealOption
└── ExtraOption

InventoryItem
```

## Initial relationships

```text
Customer 1 ───── N Reservation

Tour 1 ───────── N TourSchedule

TourSchedule 1 ─ N Reservation

Reservation 1 ── 1 TourConfiguration

Employee
    └── Tour / Inventory 관리
```

## Persistence requirements

Database는 최소 다음 데이터를 안정적으로 저장/조회할 수 있어야 합니다.

- 회원
- 여행상품
- 여행 일정
- 여행 신청
- 선택 옵션 / 최종 여행 구성
- 여행 이력 조회에 필요한 데이터
- 재고

## TBD

- Customer와 Employee를 단일 User + Role로 구성할지, 별도 모델로 구성할지
- TravelHistory 별도 Table 여부
- Reservation 상태 enum
- 가격 및 할인 저장/계산 모델
- Inventory 차감 시점과 이력
- TourSchedule 상세 column
