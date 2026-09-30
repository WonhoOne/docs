# Mister World Project Docs

AI 기반 미스터 월드 테마 여행 서비스의 **공통 문서 저장소(SSOT: Single Source of Truth)** 입니다.

이 저장소의 문서는 Frontend, Backend, Voice/Employee Console 및 AI Agent가 동일한 요구사항과 계약을 기준으로 구현하기 위해 사용합니다.

## 🚨 Mandatory reading before implementation

**사람과 AI Agent 모두 코드 작성/수정 전에 아래 문서를 순서대로 반드시 확인해야 합니다.**

승인 기준은 `docs/main`입니다. `docs/main`에 존재하는 최신 Baseline을 사용하며 feature branch 또는 미병합 PR의 Baseline은 proposal입니다. 현재 `docs/main`의 최신 승인 Baseline은 v0.1.2이며, v0.2는 PR 병합 전까지 proposal입니다.

1. 승인된 `docs/main`에 존재하는 최신 Baseline
2. [Requirements](requirements/requirements.md)
3. [Product Catalog](requirements/product-catalog.md)
4. [Domain Model](requirements/domain-model.md)
5. [Business Rules](requirements/business-rules.md)
6. [Non-functional Requirements](requirements/non-functional-requirements.md)
7. [System Architecture](architecture/system-architecture.md)
8. [Repository Responsibilities](architecture/repository-responsibilities.md)
9. [REST API Contract](api/api-spec-draft.md)
10. [ERD Shared Model](database/erd-draft.md)
11. 담당 작업이 Voice 관련이면 [Voice Contract](architecture/voice-contract.md)
12. [CONTRIBUTING.md](CONTRIBUTING.md)
13. [AGENTS.md](AGENTS.md)

### Agent rules

- 공통 요구사항, Business Rule, API Contract, ERD를 임의로 바꾸지 않습니다.
- 문서에 없는 요구사항을 임의로 만들어 구현하지 않습니다.
- 계약 변경이 필요하면 코드 변경보다 먼저 docs 변경을 제안합니다.
- Frontend/Voice/Console은 Backend의 비즈니스 규칙을 우회하거나 DB에 직접 접근하지 않습니다.
- **각 Agent는 자기 담당 Repository 밖의 코드를 기본적으로 read-only로 취급합니다.**
- 다른 파트 코드는 분석, API 확인, 통합 원인 파악을 위해 읽을 수 있지만 직접 수정·커밋·PR 생성하지 않습니다.
- 타 파트 코드 변경이 필요하면 해당 Repository Owner에게 Issue/변경 요청을 전달합니다. 팀/Owner가 명시적으로 작업을 위임한 경우에만 예외적으로 수정할 수 있습니다.
- 구현 결과가 `docs/main`의 승인 Baseline과 충돌하면 승인 Baseline을 우선하고, 변경이 필요하면 팀 합의를 위한 Issue/PR을 만듭니다.

## Repository structure

```text
.
├── README.md
├── AGENTS.md
├── CONTRIBUTING.md
├── baseline/
│   ├── BASELINE-v0.1.md
│   ├── BASELINE-v0.1.1.md
│   ├── BASELINE-v0.1.2.md
│   └── BASELINE-v0.2.md
├── requirements/
│   ├── requirements.md
│   ├── product-catalog.md
│   ├── domain-model.md
│   ├── business-rules.md
│   └── non-functional-requirements.md
├── architecture/
│   ├── system-architecture.md
│   ├── repository-responsibilities.md
│   └── voice-contract.md
├── api/
│   └── api-spec-draft.md
└── database/
    └── erd-draft.md
```

## Responsibility

문서 내용은 팀원이 공동 작성하며, Backend/Database 및 공통문서 담당인 **이한결**이 문서 구조, 버전 및 최종본 통합을 관리합니다.

## Change policy

Baseline은 변경 불가능한 문서가 아닙니다. 다만 구현 중 계약 변경이 필요한 경우 다음 순서를 따릅니다.

```text
변경 필요 발견
→ Issue / 변경 제안
→ 영향 범위 확인
→ docs 변경 및 팀 합의
→ API/ERD/Business Rule 버전 갱신
→ 각 구현 저장소 반영
```
