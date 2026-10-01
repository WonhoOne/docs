# Voice Contract v0.2

## Scope

Voice Recognition은 자유 대화형 여행 상담이 아니라 Customer GUI의 미리 정의된 여행 선택 명령을 처리합니다. Voice는 예약 초안을 만들거나 수정할 수 있지만 Reservation 제출은 사용자가 Review 후 GUI에서 명시적으로 수행합니다.

## Shared command envelope

Logical payload:

```json
{
  "version": 1,
  "command": "SELECT_THEME",
  "args": {"theme": "GOLF_CHALLENGE"}
}
```

- `version`은 `1`입니다.
- `command`는 아래 canonical command 중 하나입니다.
- `args`는 command별 object입니다.
- Raw transcript와 confidence score는 shared required field가 아닙니다.

## Canonical commands

| Command | Args | Meaning |
| --- | --- | --- |
| `SHOW_TOURS` | optional `theme` | TourProduct discovery 표시. 실제 route/navigation은 Frontend-local |
| `SELECT_THEME` | `theme` | canonical Theme 선택 |
| `SELECT_TOUR_PRODUCT` | `tourProductId` | positive integer `TourProduct.id` 선택 |
| `SELECT_STYLE` | `style` | `CLASSIC`, `GRAND`, `PREMIUM` 중 선택 |
| `SELECT_SCHEDULE` | `scheduleId` | positive integer TourSchedule 선택 |
| `SET_PARTICIPANT_COUNT` | `participantCount` | shared party-size rule에 따른 인원 설정 |
| `CHANGE_HOTEL` | `hotelOption` | canonical HotelOption 선택 |
| `CHANGE_TRANSPORT` | `transportOption` | canonical TransportOption 선택 |
| `CHANGE_MEAL` | `mealOption` | canonical MealOption 선택 |
| `ADD_OPTION` | `extraOption` | canonical ExtraOption 추가 |
| `REMOVE_OPTION` | `extraOption` | canonical ExtraOption 제거 |

Canonical values are defined in [Product Catalog](../requirements/product-catalog.md) and [Domain Model](../requirements/domain-model.md). Dynamic TourProduct/Schedule names may be matched against current Backend-derived choices; the resulting command carries canonical IDs.

## Validation and integration

```text
Speech
→ STT
→ command interpretation
→ canonical VoiceCommand
→ Frontend voice bridge
→ existing Feature action/query/mutation boundary
→ Backend validation where applicable
```

- Voice reuses existing Frontend Feature actions and does not create a parallel source of Business Rules.
- Backend remains final authority for Schedule availability, party size, Theme/Style, transport capacity, TourConfiguration, price, and Reservation creation.
- Invalid, ambiguous, or unrecognized commands make no destructive state change. Unsupported combinations are rejected rather than silently substituted.
- GUI fallback remains available when recognition fails.
- Voice does not access MySQL directly or add a Voice-specific public REST endpoint.

## Explicitly unsupported in v0.2

- `SUBMIT_RESERVATION` or automatic Reservation POST
- LOGIN/SIGNUP credential entry
- `CANCEL_RESERVATION`, REFUND, or PAYMENT
- free-form recommendation/search
- Customer Voice mutation of Employee Console data

## Local implementation choices

STT wrapper, Web Speech API internals, parser/matcher algorithm, synonym dictionary, confidence threshold, retry prompt, and Employee Console component state are ai-console-local. UI route behavior and bridge details are Frontend/integration-local. Tests evaluate canonical command + args interpretation rather than raw transcript equality.
