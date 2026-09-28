# Repository Responsibilities v0.1

| Repository | Owner | Responsibilities |
| --- | --- | --- |
| `WonhoOne/docs` | 이한결 관리 / 전원 작성 | Requirements, Business Rules, Architecture, API Contract, ERD, UML, Decisions |
| `WonhoOne/backend` | 이한결 | Spring Boot REST API, Business Logic, DB persistence, Backend tests |
| `WonhoOne/frontend` | 김태우 | Customer GUI, Backend API integration, Frontend tests |
| `WonhoOne/ai-console` | 주원호 | Voice Recognition, Employee Console, Backend integration, E2E integration |

## Cross-repository access policy

각 Agent는 **자기 담당 Repository의 코드에 대해서만 기본 write 권한을 가진다고 가정**합니다.

다른 파트 Repository의 코드는 기본적으로 **read-only**입니다. 다음 목적의 열람은 허용합니다.

- API 사용 방식 확인
- 통합 지점 확인
- 버그 원인 분석
- 데이터 흐름 및 호출 관계 파악
- 상대 파트에 필요한 변경사항을 구체적으로 설명하기 위한 조사

하지만 다른 파트의 파일을 직접 수정하거나, 그 Repository에 commit/PR을 생성하는 것은 기본적으로 금지합니다.

변경이 필요할 경우:

```text
타 파트 변경 필요 발견
→ 해당 Repository Owner에게 Issue / 변경 요청
→ 영향 범위 및 계약 확인
→ 해당 Owner 또는 담당 Agent가 수정
→ 통합 확인
```

단, Repository Owner 또는 팀이 특정 작업을 **명시적으로 위임한 경우**에는 해당 범위에 한해 예외적으로 수정할 수 있습니다.

## Boundary rules

### Frontend Agent

- Customer GUI를 구현합니다.
- Backend API Contract를 사용합니다.
- DB에 직접 접근하지 않습니다.
- 핵심 Business Rule을 독립적인 진실의 원천으로 재구현하지 않습니다.
- Backend 및 ai-console 코드는 read-only로 취급합니다.

### Backend Agent

- API, Business Logic, DB 모델 및 persistence를 구현합니다.
- Customer GUI를 수정하지 않습니다.
- Voice Recognition 자체를 구현하지 않습니다.
- frontend 및 ai-console 코드는 read-only로 취급합니다.

### Voice / Console Agent

- STT 및 제한 명령 해석을 구현합니다.
- Employee Console을 구현합니다.
- Backend 공개 API를 사용합니다.
- DB에 직접 접근하거나 Backend Business Rule을 우회하지 않습니다.
- backend 및 frontend 코드는 read-only로 취급합니다.

## Shared contract changes

Requirements, Business Rules, API, ERD 또는 Repository boundary를 바꿔야 할 경우 구현을 먼저 변경하지 않습니다. docs 변경 제안 → 영향 범위 확인 → 팀 합의 → 구현 순서를 따릅니다.
