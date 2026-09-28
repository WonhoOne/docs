# Repository Responsibilities v0.1

| Repository | Owner | Responsibilities |
| --- | --- | --- |
| `WonhoOne/docs` | 이한결 관리 / 전원 작성 | Requirements, Business Rules, Architecture, API Contract, ERD, UML, Decisions |
| `WonhoOne/backend` | 이한결 | Spring Boot REST API, Business Logic, DB persistence, Backend tests |
| `WonhoOne/frontend` | 김태우 | Customer GUI, Backend API integration, Frontend tests |
| `WonhoOne/ai-console` | 주원호 | Voice Recognition, Employee Console, Backend integration, E2E integration |

## Boundary rules

### Frontend Agent

- Customer GUI를 구현합니다.
- Backend API Contract를 사용합니다.
- DB에 직접 접근하지 않습니다.
- 핵심 Business Rule을 독립적인 진실의 원천으로 재구현하지 않습니다.

### Backend Agent

- API, Business Logic, DB 모델 및 persistence를 구현합니다.
- Customer GUI를 수정하지 않습니다.
- Voice Recognition 자체를 구현하지 않습니다.

### Voice / Console Agent

- STT 및 제한 명령 해석을 구현합니다.
- Employee Console을 구현합니다.
- Backend 공개 API를 사용합니다.
- DB에 직접 접근하거나 Backend Business Rule을 우회하지 않습니다.

## Shared contract changes

Requirements, Business Rules, API, ERD 또는 Repository boundary를 바꿔야 할 경우 구현을 먼저 변경하지 않습니다. docs 변경 제안 → 영향 범위 확인 → 팀 합의 → 구현 순서를 따릅니다.
