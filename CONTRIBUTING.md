# Contributing

## Workflow

```text
Backlog → In Progress → Review → Done
```

1. 구현 작업은 Issue 단위로 나눕니다.
2. Issue에는 목적, 담당자, Acceptance Criteria, 관련 Requirement ID를 기록합니다.
3. 기능별 Branch에서 작업하고 `main` 직접 수정은 최소화합니다.
4. Pull Request에는 변경 요약, 테스트 결과, 영향 범위, 관련 Issue를 기록합니다.
5. 최소 1인의 검토 후 병합합니다.

## Contract-first rule

다음 항목의 변경은 구현 코드보다 docs 변경이 선행되어야 합니다.

- Functional Requirement
- Business Rule
- 공통 Domain 용어/의미
- 공개 REST API 요청/응답 계약
- 공유 DB/ERD 구조
- Repository responsibility

내부 클래스 구조, 함수 분리, 패키지 구조 등 외부 계약에 영향을 주지 않는 구현 세부사항은 각 Repository Owner가 결정할 수 있습니다.

## AI-generated work

AI Agent가 작성한 코드와 문서도 동일한 Issue/Branch/PR/Test 절차를 따릅니다. Agent는 문서에 없는 요구사항을 임의로 추가하거나 TBD를 임의 확정해서는 안 됩니다.
