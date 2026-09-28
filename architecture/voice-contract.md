# Voice Contract v0.1

## Scope

Voice Recognition은 자유 대화형 여행 상담이 아니라 **미리 정의된 여행 선택 명령**을 처리합니다.

## Processing flow

```text
Speech
→ Speech-to-Text
→ Command interpretation
→ Customer GUI state update and/or Backend API
→ Backend validation
```

## Initial command candidates

아래 목록은 v0.1의 방향을 공유하기 위한 후보이며 최종 명령 목록과 파라미터는 TBD입니다.

```text
SELECT_THEME
SELECT_STYLE
CHANGE_HOTEL
CHANGE_MEAL
CHANGE_TRANSPORT
ADD_OPTION
SHOW_TOUR
```

예시:

```text
"골프 여행 보여줘"
→ SELECT_THEME(GOLF_CHALLENGE)

"프리미엄으로 해줘"
→ SELECT_STYLE(PREMIUM)

"5성급 호텔로 바꿔줘"
→ CHANGE_HOTEL(...)
```

## Rules

- Voice Agent는 Tour Style 허용 여부, 확정 인원 등 Business Rule의 최종 판정을 직접 소유하지 않습니다.
- Backend API와 Business Rules를 기준으로 동작합니다.
- Voice 인식 실패 시 동일 기능을 GUI에서 수행할 수 있어야 합니다.
