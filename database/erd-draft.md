# ERD Skeleton v0.1.1

> Status: **Approved shared model skeleton**. Detailed persistence design remains a draft for v0.2.
>
> 실제 PK/FK, 상세 column, nullable, index 및 Customer/Employee 구현 방식은 v0.2에서 확정합니다. 아래는 공통 Domain 개념과 최소 필드만 나타냅니다.

## Initial entity candidates

```text
Customer (signup: name, address, contact)
Employee (signup: name, address, contact)
Theme (four fixed values)
TourProduct (one Theme; product name and basic information)
TourSchedule (one TourProduct; period, recruitment state, participant total)
TourStyle (CLASSIC / GRAND / PREMIUM defaults)
TourConfiguration (Theme/TourProduct, TourStyle, Hotel, Transport, Meal, extras)
Reservation (one Customer and TourSchedule; participantCount >= 1 integer; Honeymoon: >= 2 and even)
Inventory (id, itemType, quantity)

TravelHistory: Customer query concept; separate table TBD
```

## Initial relationships

```text
Theme 1 ───────── N TourProduct
TourProduct 1 ─── N TourSchedule
Customer 1 ───── N Reservation
TourSchedule 1 ─ N Reservation
Reservation 1 ── 1 TourConfiguration (conceptual skeleton)
Employee ─────── TourProduct / Inventory 관리
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

일반 TourSchedule의 신청 인원은 연결된 Reservation들의 `participantCount` 합으로 계산하고 총 3명 이상일 때 확정됩니다. Honeymoon 모집 합계는 유효한 Reservation마다 파생되는 `coupleCount = participantCount / 2`의 합으로 계산하며, 총 2 couples/teams 이상일 때 확정됩니다. 따라서 각 Honeymoon Reservation의 `participantCount`는 2 이상의 짝수여야 합니다. 이 의미 정리는 ERD 구조나 별도 Couple/Team Entity를 추가하지 않으며, 구체적인 persistence 표현은 미결정 상태로 둡니다. 취소 기능이나 취소 상태는 설계하지 않습니다.

`Inventory`는 품목 종류와 현재 수량을 나타냅니다. 예시 `itemType`은 `COUPLE_TSHIRT`, `GINSENG_GIFT`, `GOLF_BALL`, `SCARF`입니다. 향후 SKU 상세, 입출고 이력, 재고 이동 기록이 필요해지면 `InventoryItem`/`InventoryTransaction`으로 분리할 수 있으나 v0.1.1에서는 분리하지 않습니다.

## TBD

- Customer와 Employee를 단일 User + Role로 구성할지, 별도 모델로 구성할지
- TravelHistory 별도 Table 여부
- Reservation 상태 enum
- 가격 및 할인 저장/계산 모델
- Inventory 차감 시점과 상세 이력
- TourSchedule 상세 column
