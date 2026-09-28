# Development Baseline v0.1

- Status: **Implementation Baseline**
- Purpose: Frontend / Backend / Voice·Employee Console / AI Agent가 동일한 가정을 공유하도록 하는 최초 개발 기준선
- Change policy: 변경 가능하나 docs Issue/PR과 영향 범위 검토 후 반영

## Fixed in v0.1

- 시스템의 주요 기능 범위
- 공통 Domain 용어
- 핵심 Business Rules
- Frontend / Backend / DB / Voice·Console 책임 경계
- Repository별 Owner 및 책임
- REST API endpoint skeleton
- ERD skeleton
- 제한 명령 기반 Voice 방향
- Git/PR/AI Agent 협업 규칙

## Not fixed in v0.1

아래 항목은 아직 TBD이며 Agent가 임의로 확정하면 안 됩니다.

- 로그인 인증 방식
- Customer / Employee의 실제 DB 상속·분리 구조
- TourSchedule 상세 데이터 구조
- 가격 계산 공식
- 실제 Hotel / Transport / Meal 옵션 목록
- 단골 고객 기준 및 할인율
- 여행 확정 알림의 실제 구현 방식
- Inventory 차감 시점
- 여행 취소 기능
- Reservation 상태 종류
- TravelHistory 별도 Entity/Table 여부
- Voice의 최종 명령 목록과 파라미터 세부 형식

## Contract precedence

구현 중 해석이 충돌할 경우 다음 순서로 확인합니다.

1. 승인된 요구사항 / Business Rules
2. Domain Model
3. API / ERD 등 공유 Contract
4. Repository별 내부 구현

충돌 또는 빈틈을 발견하면 추측하지 말고 TBD 또는 변경 제안으로 올립니다.
