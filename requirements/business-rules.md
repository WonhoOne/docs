# Business Rules v0.1.2

공통 비즈니스 규칙의 최종 검증 책임은 Backend에 있습니다. Frontend 또는 Voice가 UI 수준에서 같은 제약을 적용하더라도 Backend 검증을 대체하지 않습니다.

| ID | Rule |
| --- | --- |
| BR-01 | Theme은 `HONEYMOON_ROMANCE`, `PARENTS_HEALING`, `GOLF_CHALLENGE`, `OUTDOOR_TREKKING` 4종이다. |
| BR-02 | TourStyle은 `CLASSIC`, `GRAND`, `PREMIUM` 3종이다. |
| BR-03 | `HONEYMOON_ROMANCE`는 `GRAND` 또는 `PREMIUM`만 선택할 수 있다. |
| BR-04 | `PARENTS_HEALING`은 `GRAND` 또는 `PREMIUM`만 선택할 수 있다. |
| BR-05 | 고객은 기본 Tour Style 선택 이후에도 Hotel, Transport, Meal을 변경할 수 있다. |
| BR-06 | `HONEYMOON_ROMANCE`가 아닌 일반 TourSchedule은 해당 일정의 Reservation `participantCount` 합이 3명 이상이면 확정된다. |
| BR-07 | 확정된 팀 결정: `HONEYMOON_ROMANCE`에서 1 couple/team은 2 participants다. 각 Honeymoon Reservation의 `participantCount`는 2 이상의 짝수여야 하며, 해당 Reservation의 `coupleCount = participantCount / 2`다. Honeymoon TourSchedule은 유효한 Reservation들의 `coupleCount` 합이 2 이상이면 확정된다. 따라서 `participantCount` 1과 3인 Reservation의 합계 4는 유효한 Honeymoon 모집 입력이 아니다. |
| BR-08 | 로그인 고객의 여행 이력은 최근 여행 순으로 제공한다. |
| BR-09 | Travel History에는 최소 상품, 기간, Tour Style, 가격을 포함한다. |
| BR-10 | Voice Recognition은 미리 정의된 주요 명령 집합을 대상으로 한다. |
| BR-11 | 음성인식이 실패하더라도 고객은 GUI를 통해 동일 기능을 수행할 수 있어야 한다. |
| BR-12 | 핵심 Business Rule의 최종 유효성 검사는 Backend가 수행한다. |
| BR-13 | 일반 Reservation 한 건의 `participantCount`는 1 이상의 정수이며, 한 건으로 여러 명을 예약할 수 있다. `HONEYMOON_ROMANCE` Reservation에는 BR-07의 더 강한 pair-unit 제약이 추가 적용된다. |
| BR-14 | TourSchedule이 신청 인원 임계치에 도달해 최초 확정될 때 신청 고객에게 SMS를 실제로 전송한다. |
| BR-15 | Loyalty Discount 기능은 요구사항이다. 단골 고객 판정 기준과 할인율은 TBD이며, 적용 시점과 중복 여부도 임의로 확정하지 않는다. |

## SMS implementation boundary

- 최종 production/demo 경로에는 실제 SMS Provider/API를 통한 전송이 있어야 합니다. Console log 또는 mock만으로 FR-14를 충족하지 않습니다.
- 개발/테스트 중 mock adapter는 허용됩니다. SMS Provider 자체는 TBD입니다.
- API key와 secret은 repository에 하드코딩하지 않고 환경변수 또는 secret configuration을 사용합니다.
- 여행 취소 기능이나 Reservation 취소 상태는 현재 요구사항이 아니므로 설계하지 않습니다.

## Important

- Agent는 위 규칙을 편의상 완화하거나 변경하면 안 됩니다.
- 가격 계산, Loyalty Discount 상세 기준, 재고 차감 시점처럼 이 문서에 없는 정책은 TBD입니다.
