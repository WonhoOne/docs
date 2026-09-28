# Business Rules v0.1

공통 비즈니스 규칙의 최종 검증 책임은 Backend에 있습니다. Frontend 또는 Voice가 UI 수준에서 같은 제약을 적용하더라도 Backend 검증을 대체하지 않습니다.

| ID | Rule |
| --- | --- |
| BR-01 | Theme Tour는 허니문 낭만, 효도 힐링, 골프 챌린지, 아웃도어 트레킹 4종이다. |
| BR-02 | Tour Style은 Classic, Grand, Premium 3종이다. |
| BR-03 | Honeymoon Romance는 Classic을 선택할 수 없다. |
| BR-04 | Parents Healing은 Classic을 선택할 수 없다. |
| BR-05 | 고객은 기본 Tour Style 선택 이후에도 Hotel, Transport, Meal을 변경할 수 있다. |
| BR-06 | 일반 여행 일정은 총 신청 인원이 3명 이상이면 확정된다. |
| BR-07 | Honeymoon 여행 일정은 총 신청 인원이 4명 이상이면 확정된다. |
| BR-08 | 로그인 고객의 여행 이력은 최근 여행 순으로 제공한다. |
| BR-09 | 여행 이력에는 최소 상품, 기간, 등급, 가격을 포함한다. |
| BR-10 | Voice Recognition은 미리 정의된 주요 명령 집합을 대상으로 한다. |
| BR-11 | 음성인식이 실패하더라도 고객은 GUI를 통해 동일 기능을 수행할 수 있어야 한다. |
| BR-12 | 핵심 Business Rule의 최종 유효성 검사는 Backend가 수행한다. |

## Important

- Agent는 위 규칙을 편의상 완화하거나 변경하면 안 됩니다.
- 가격, 할인, 재고 차감 시점처럼 이 문서에 없는 정책은 TBD입니다.
