# Repository Responsibilities — Voice V0 repository responsibility reconciliation

## Historical records and current implementation responsibility

제출된 프로젝트 계획서 및 과거 기록의 역할 배정(예: 주원호 = AI Voice + Employee Console)은 당시의 historical assignment입니다. 이 문서는 현재 구현 책임을 별도로 기록하며 제출된 계획서의 역할 항목이나 과거 기록을 소급 수정하지 않습니다.

현재 운영 근거는 누적 이슈의 최신 ownership update입니다. 이전의 상충하는 댓글은 historical/superseded로 취급합니다.

- [frontend #6 — 현재 Frontend / Console / Voice 담당](https://github.com/WonhoOne/frontend/issues/6#issuecomment-5978230895)
- [backend #8 — Backend owner의 AI Voice 담당](https://github.com/WonhoOne/backend/issues/8#issuecomment-5978231988)
- [ai-console #3 — 이전 결합 역할 배정 대체](https://github.com/WonhoOne/ai-console/issues/3#issuecomment-5978231420)

| 담당자 | Current implementation responsibility |
| --- | --- |
| 이한결 | Backend, Database, Shared docs management, AWS / SOLAPI runtime, AI Voice |
| 김태우 | Customer Frontend, Employee Console, Frontend ↔ Backend live integration |
| 주원호 | 현재 primary implementation scope 없음 |

## Current repository implementation boundary

| Repository | Owner | Responsibilities |
| --- | --- | --- |
| `WonhoOne/docs` | 이한결 관리 / 전원 작성 | Requirements, Business Rules, Architecture, API Contract, ERD, UML, Decisions |
| `WonhoOne/backend` | 이한결 | Spring Boot REST API, Backend services, Business Rules / validation, DB persistence, Backend tests |
| `WonhoOne/frontend` | 김태우 (일반 Frontend 책임) / 이한결 (아래 Voice 디렉터리 범위) | Customer GUI, Employee Console, Frontend ↔ Backend live integration, Frontend tests; AI Voice browser runtime 및 Voice → Frontend bridge는 명시된 디렉터리에 한정 |
| `WonhoOne/ai-console` | Historical / legacy repository; 현재 신규 구현 주 담당 배정 없음 | 과거 Voice + Employee Console 저장소로 유지. 기본적으로 새 active feature를 구현하지 않으며 이 gate에서 삭제·이름 변경·archive·용도 변경하지 않음 |

### Voice-owned frontend directories

AI Voice의 현재 구현 저장소는 `WonhoOne/frontend`입니다. Frontend 전체 Owner는 계속 김태우이며, 이한결의 Voice 구현 범위는 다음 디렉터리로 한정합니다.

| Directory | 구현 담당 | Boundary |
| --- | --- | --- |
| `src/integrations/voice/**` | 이한결 — AI Voice | Browser/runtime boundary: STT adapter, normalization, interpreter/parser, matcher, canonical `VoiceCommand` implementation |
| `src/features/voice-bridge/**` | 이한결 — AI Voice; 김태우의 기존 Feature 경계와 조율 | Voice ↔ Frontend feature integration: canonical command를 기존 Feature action/query/mutation 및 `ReservationDraft` 상태에 연결 |

이 범위는 Frontend 저장소 내 명시적인 Voice 구현 예외입니다. 기존 Feature, GUI, Employee Console, API adapter 등 두 디렉터리 밖의 파일은 이 예외에 포함되지 않습니다. 해당 변경은 김태우에게 handoff하거나 별도의 명시적 위임을 받아야 합니다.

Web Speech API는 browser runtime에서 실행되고 `ReservationDraft`도 이미 Frontend runtime에 있습니다. Frontend에는 `src/integrations/voice/.gitkeep`와 `src/features/voice-bridge/.gitkeep`가 준비되어 있습니다. Voice engine을 ai-console로 분리하면 불필요한 package/artifact/version 동기화가 생기므로, 같은 Frontend runtime에 두어 통합 복잡도를 줄이고 발표 시 코드 흐름을 명확히 합니다. 이는 새 기술/framework 결정이 아니라 구현 위치와 책임의 정리입니다.

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

위 Voice 디렉터리 배정은 그러한 제한된 예외입니다. 같은 사람이 Backend와 AI Voice를 담당하더라도 각 작업의 write scope는 구분합니다. Voice 작업이 Backend 코드 변경을 자동 허용하지 않으며, 다른 Owner 또는 명시된 디렉터리 밖의 변경은 handoff/위임 원칙을 계속 따릅니다. 이 docs-only gate에서는 docs만 수정하며 frontend/backend/ai-console은 모두 read-only입니다.

## Boundary rules

### Frontend Agent

- Customer GUI, Employee Console 및 Frontend ↔ Backend live integration을 구현합니다.
- Backend API Contract를 사용합니다.
- DB에 직접 접근하지 않습니다.
- 핵심 Business Rule을 독립적인 진실의 원천으로 재구현하지 않습니다.
- Backend 및 ai-console 코드는 read-only로 취급합니다.
- Voice-owned 디렉터리와 기존 Feature의 연결은 AI Voice 담당자와 조율합니다.

### Backend Agent

- API, Business Logic, DB 모델 및 persistence를 구현합니다.
- Customer GUI를 수정하지 않습니다.
- Backend 작업 범위에서 Voice Recognition을 구현하지 않습니다. 같은 담당자의 AI Voice 작업은 위 Frontend 디렉터리 범위로 구분합니다.
- frontend 및 ai-console 코드는 read-only로 취급합니다.

### AI Voice Agent

- 위 Voice-owned Frontend 디렉터리에서 STT 및 제한 명령 해석과 Voice bridge를 구현합니다.
- Employee Console 구현은 김태우의 Frontend 책임입니다.
- 기존 Frontend Feature 경계를 통해 Backend 공개 API를 사용합니다.
- DB에 직접 접근하거나 Backend Business Rule을 우회하지 않습니다.
- backend, ai-console 및 명시된 Voice 디렉터리 밖의 frontend 코드는 기본적으로 read-only로 취급합니다. 필요한 변경은 해당 Owner에게 handoff/위임 요청합니다.

## Preserved Voice semantic boundary

[Voice Contract](voice-contract.md)의 승인된 의미 흐름은 그대로 유지합니다.

```text
Speech
→ STT
→ command interpretation
→ canonical VoiceCommand
→ Frontend voice bridge
→ existing Feature action/query/mutation boundary
→ Backend validation where applicable
```

- Voice는 `ReservationDraft` 상태를 생성·변경할 수 있습니다.
- Voice는 Reservation을 자동 제출·생성하거나 자동 Reservation POST를 수행할 수 없습니다. Review 후 사용자가 GUI에서 명시적으로 submit합니다(FR-12, FR-13, BR-28).
- Voice는 별도의 Backend Business Rule authority가 아닙니다. Backend는 Business Rule과 Reservation 생성 검증의 최종 권한을 유지합니다(BR-12, BR-26).
- Voice는 MySQL에 직접 접근하거나 Voice 전용 공개 API 경로를 만들지 않습니다.

## Change scope and remaining guidance reconciliation

이 gate는 repository responsibility / implementation-boundary 문서를 변경합니다. Voice command semantics, command 목록/args, Backend public API, Domain semantics, Business Rules, Product Catalog 및 제출된 historical proposal 역할 항목은 변경하지 않습니다.

`voice-contract.md`의 Local implementation choices에 남은 `ai-console-local` 위치 표기는 이전 저장소 배정을 반영합니다. 현재 구현 위치/담당은 이 문서의 디렉터리 경계를 따르며, STT/parser 등의 내부 구현 선택이 local이라는 의미와 command/test 의미는 그대로 유지합니다. 해당 오래된 위치 참조는 이번 gate에서 변경하지 않고 후속 문서 정리 대상으로 보고합니다.

이 변경도 unmerged docs branch/PR에서는 제안이며, `docs/main` 병합 후 승인된 책임 경계가 됩니다. 다음 gate는 **Voice V0-B — frontend / ai-console local AGENTS responsibility reconciliation**입니다. 병합 후 해당 저장소 Owner/위임 담당자가 로컬 지침을 조정하며, 이 gate에서는 Voice source code나 다른 저장소 지침을 수정하지 않습니다.

## Shared contract changes

Requirements, Business Rules, API, ERD 또는 Repository boundary를 바꿔야 할 경우 구현을 먼저 변경하지 않습니다. docs 변경 제안 → 영향 범위 확인 → 팀 합의 → 구현 순서를 따릅니다.
