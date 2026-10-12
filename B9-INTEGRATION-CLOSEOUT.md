# B9 Integration Gate — Final Closeout

> Approval status: this record is a proposal while on an unmerged docs branch/PR. It becomes the approved B9 closeout record only when merged into `docs/main`. The existing approved Baseline v0.2 and linked Shared Contracts remain unchanged.

## 1. Purpose

Mister World의 **B9 Integration Gate documentation closeout**입니다. 완료된 Backend, Frontend, Voice, cross-repository E2E 및 최종 실제 Chrome microphone smoke의 근거와 최종 판정을 한 곳에 기록합니다.

GitHub PR/commit/run identity와 성공 결과는 저장소 근거로 확인했습니다. 실제 microphone/handset 검증과 local runtime incident는 프로젝트 담당자가 제공한 완료 evidence를 기록한 것이며, 이 docs 작업에서 다시 실행한 테스트가 아닙니다. 기록 작성일은 2026-10-12 (Asia/Seoul)이며 개별 수동 검증의 실행 일시/Chrome 세부 버전을 새로 추정하지 않습니다.

## 2. Scope / Non-goals

변경 범위는 `WonhoOne/docs`의 closeout 문서와 README discovery link입니다. `frontend`, `backend`, `ai-console`은 모두 read-only입니다. 코드 구현, 새 B9 구현, Shared Contract 변경 및 다른 repository의 commit/PR 생성은 범위 밖입니다.

Requirements/Business Rules 의미, API, ERD, Product Catalog, `ReservationDraft` schema, `VoiceCommand` version 1 및 canonical command 11개, Backend public API, Reservation submit 의미와 repository ownership은 유지합니다.

현재 책임과 Voice 디렉터리 경계는 [Repository Responsibilities](architecture/repository-responsibilities.md)를 그대로 참조합니다. 이 문서는 담당자 배정이나 historical assignment를 소급 변경하지 않습니다. Backend는 business validation/persistence의 최종 authority이고, Voice는 기존 Frontend feature 경계를 사용합니다.

관련 requirement evidence: FR-01–09, FR-12–14; BR-05, BR-10–14, BR-16, BR-18, BR-20, BR-26–28; NFR-06. 이 closeout은 모든 NFR에 대한 새 정량 인증이나 실제 screen-reader 검증을 주장하지 않습니다.

## 3. Approved reference SHAs

| Reference | SHA / meaning |
| --- | --- |
| Approved docs/main; starting main | `c5b763253bb4acd8e4e4c6db0a736be8f1c247fc` |
| Latest approved Baseline | [Baseline v0.2](baseline/BASELINE-v0.2.md), present on the starting docs/main |
| Approved Backend snapshot | [`597cf92f1baf93211862d4ea5dbfa8a199b808a2`](https://github.com/WonhoOne/backend/commit/597cf92f1baf93211862d4ea5dbfa8a199b808a2) |
| B9 final Frontend integration snapshot | [`1e930fa88767dce065d838a75cc9ee38ad7bceeb`](https://github.com/WonhoOne/frontend/commit/1e930fa88767dce065d838a75cc9ee38ad7bceeb) |
| Frontend final merge first parent: #42 Review identity fix | `f6a356ef3b2b86f492d56aff0f4ef31ec319671a` |
| Frontend final merge second parent: #41 rebased E2E head | `d7e0ba963ceaaf29183f1a0f36d1c5e01d3a73bd` |

Frontend final SHA는 B9 완료 시점의 고정 evidence snapshot이며 이후 main 변경을 추적하는 floating reference가 아닙니다.

현재 계약 참조:

- [Requirements](requirements/requirements.md)
- [Product Catalog](requirements/product-catalog.md)
- [Business Rules](requirements/business-rules.md)
- [REST API Contract](api/api-spec-draft.md)
- [ERD Shared Model](database/erd-draft.md)
- [Voice Contract](architecture/voice-contract.md)
- [Repository Responsibilities](architecture/repository-responsibilities.md)

## 4. B9-A Backend evidence

**B9-A Backend Readiness: ✅ PASS**

| Milestone | Result |
| --- | --- |
| B9-A1 Actual MySQL | ✅ PASS |
| B9-A2.1 Demo / SMS readiness | ✅ PASS |
| B9-A2.2 Real SOLAPI / handset | ✅ PASS |
| Backend current v0.2 integration gate | ✅ PASS |

Approved Backend SHA: `597cf92f1baf93211862d4ea5dbfa8a199b808a2`.

Demo/runtime은 Spring Boot + 실제 MySQL + real persistence입니다. Billable SMS는 test/E2E에서 disabled이며 실제 SMS readiness와 SOLAPI/handset verification은 별도 완료 evidence입니다. SMS disabled E2E 자체를 실제 SMS delivery 증거로 해석하지 않습니다.

## 5. B9-B Frontend Live Integration evidence

**B9-B Frontend Live Integration: ✅ PASS**

| Evidence | Merge SHA |
| --- | --- |
| [Frontend PR #26 — authenticated Backend integration / live journey](https://github.com/WonhoOne/frontend/pull/26) | `3bccb0aa8a2d08658bbd4707904b24087b4c1590` |
| [Frontend PR #37 — Extras closure](https://github.com/WonhoOne/frontend/pull/37) | `516787e9b1e78bcece78db8656a8591e33452b5e` |

완료 evidence는 real Backend integration, ReservationDraft workflow, Review, Authentication, explicit Reservation submit, Reservation detail/history path 및 real Backend/MySQL E2E입니다.

확인된 Style defaults:

| Style | Initial composition |
| --- | --- |
| CLASSIC | `HOTEL_3_STAR` / `LUNCH_BOX` |
| GRAND | `HOTEL_4_STAR` / `LOCAL_RESTAURANT` |
| PREMIUM | `HOTEL_5_STAR` / `PREMIUM_RESTAURANT` / `CHAMPAGNE` |

Frontend-local Draft의 transport는 `BEGIN_CONFIGURE` 시 `null`입니다. 이는 중간 Draft 상태에 대한 구현 evidence이며 Product Catalog의 Theme default나 Backend가 요구하는 최종 configuration을 바꾸지 않습니다.

Premium Champagne는 처음 한 번 default initialize되고 사용자가 제거할 수 있습니다. 제거 후 `SET_EXTRAS`, `RESTORE_DRAFT`, `SELECT_TOUR_STYLE` 등에서 부활하지 않습니다.

## 6. B9-C Voice milestones / evidence

**B9-C Voice Live Integration: ✅ FINAL PASS**

| Milestone | Completion evidence | Result |
| --- | --- | --- |
| V1 | Canonical Voice command model | PASS / merged |
| V2 | Transcript normalizer, lexicon, interpreter; 11/11 canonical commands | PASS / merged |
| V3 | Dynamic context, TourProduct/TourSchedule matching, `selectedTourProductId` schedule scoping | PASS / merged |
| V4 | Browser SpeechRecognition adapter | PASS |
| V5 | Runtime orchestration: decoder → interpreter → dynamic matcher | PASS / merged |
| V6-A | Generic Voice command bridge; 11/11 mapping | PASS / merged |
| V6-B | ReservationDraft adapter; [PR #28](https://github.com/WonhoOne/frontend/pull/28), merge `48289511e3d87f14eb0a36edd392308126c9b99c` | PASS |
| V6-C | Live Voice wiring; [PR #38](https://github.com/WonhoOne/frontend/pull/38), merge `02d3e2dd54eda6189aaa69106e28d124962116df` | PASS |
| V9 | Browser / Accessibility QA; [PR #39](https://github.com/WonhoOne/frontend/pull/39), resulting main `278394dbbd3ff49f61daffa29d7e6b40834bb5cf` | PASS |
| V9.1 / B9-C1 | Korean STT compatibility; [PR #40](https://github.com/WonhoOne/frontend/pull/40), merged main `a9cc9c16e0f26cae0c8cdc12cf1f4010e3bcbe23` | PASS |

V9 CI [#420 / 37891395262](https://github.com/WonhoOne/frontend/actions/runs/37891395262): **SUCCESS**. Reported validation: 108 unit files / 1702 tests, Playwright 196/196, real Spring/MySQL authenticated E2E, build PASS and audit PASS.

V9.1 branch: `fix/voice-korean-stt-compat-v9-1`; head: `e13c4c4dfa6c90c14e0535b0d4531c4bb69d41fb`; CI [#421 / 37950455634](https://github.com/WonhoOne/frontend/actions/runs/37950455634): **SUCCESS**.

V9.1 adds local Korean STT compatibility:

- `SHOW_TOURS`: spaced “보여 줘”.
- 3-star hotel aliases: “삼성급”, “삼성급 호텔”.
- 4-star hotel aliases: “사성급”, “사성급 호텔”.
- 5-star hotel aliases: “오성급”, “오성급 호텔”.
- Meal phrase: `식사 ${alias}으로 변경`.
- Unsupported booking safety phrases: “예약해줘”, “예약 해줘”, “예약해 줘”, “예약 해 줘”.

Global whitespace/number normalization was not added. These aliases are implementation-local; the Shared Voice contract did not change.

### Post-V9.1 real Chrome microphone QA

Project-supplied manual evidence used actual Chrome and an actual microphone.

| Manual check | Result |
| --- | --- |
| SHOW_TOURS | ✅ PASS |
| SHOW_TOURS(theme) | ✅ PASS |
| 3성급 / 삼성급 hotel recognition | ✅ PASS |
| 5성급 / 오성급 hotel recognition | ✅ PASS |
| “식사 고급 레스토랑으로 변경” | ✅ PASS |
| “예약해줘” and spaced booking variants safety | ✅ PASS |

Voice updates ReservationDraft only. It cannot automatically create/submit a Reservation. GUI Review followed by explicit GUI submit is the Reservation creation path (FR-12, FR-13, BR-28). Deterministic fake recognition is separate from this real microphone evidence.

## 7. B9-D Cross-Repo E2E evidence

**B9-D Cross-Repo E2E: ✅ PASS**

Proven boundary:

```text
SpeechRecognition deterministic stub
→ production Voice adapter/runtime/interpreter/bridge
→ ReservationDraft
→ GUI
→ Review
→ explicit GUI submit
→ real Spring Backend
→ real MySQL
```

The scenario does not directly write Draft in place of Voice, use Backend mock/MSW, create the final Reservation through an API during setup, send billable SMS, or auto-submit from Voice. Real microphone evidence remains separate B9-C manual QA.

### B9-D2 final test identity and CI

[Frontend PR #41 — test: prove B9-D cross-repo Voice reservation journey](https://github.com/WonhoOne/frontend/pull/41):

| Item | Value |
| --- | --- |
| Initial test head | `f8be611d8d4bee3f500edf9cf85cc6174b56f21c` |
| Base after #42 merge | `f6a356ef3b2b86f492d56aff0f4ef31ec319671a` |
| Rebased final head | `d7e0ba963ceaaf29183f1a0f36d1c5e01d3a73bd` |
| Test file | `tests/f2-mysql/voice-reservation.spec.ts` |
| Test Git blob before and after rebase | `9db3b7c8a6f9872721b6dfd7bd8cda5ca83aa5b4` |
| Merge commit / final B9 Frontend main | `1e930fa88767dce065d838a75cc9ee38ad7bceeb` |

The identical test blob confirms no assertion/test logic weakening during rebase.

Final CI [38032734779 — D11 Reservation Journey Verification](https://github.com/WonhoOne/frontend/actions/runs/38032734779): **SUCCESS**.

| Job | Result |
| --- | --- |
| verify | ✅ SUCCESS |
| Real Backend public read smoke | ✅ SUCCESS |
| Real Backend + MySQL authenticated customer journey | ✅ SUCCESS |

### Scenario and persisted configuration

Real Backend catalog/schedules provide the actual TourProduct and generated schedule selected through Voice. The journey sets PREMIUM, participant count 2, HOTEL_4_STAR, PRIVATE_LUXURY_CAR_2 and LOCAL_RESTAURANT; adds COFFEE; prevents duplicate Coffee; removes default Champagne; and confirms Champagne stays removed after reload.

“예약해 줘” produces **Reservation POST = 0**. The user enters Review, authenticates/logs in and returns to Review; Reservation POST remains **0** after login return. Explicit GUI **Apply** sends **exactly one Reservation POST**, returns **HTTP 201**, persists the Reservation in real MySQL, clears Draft after success and makes Reservation detail navigable.

Persisted canonical configuration summary (evidence labels, not a new schema):

| Selection | Persisted value |
| --- | --- |
| participantCount | 2 |
| style | PREMIUM |
| hotel | HOTEL_4_STAR |
| transport | PRIVATE_LUXURY_CAR_2 |
| meal | LOCAL_RESTAURANT |
| extras | [COFFEE] |

The existing API fields remain `configuration.hotelOption`, `transportOption`, `mealOption` and `extraOptions`; the table does not rename wire fields.

Review shows actual product name for Journey and actual schedule/date for Schedule, with no “Selected tour” or “Selected schedule” placeholder.

## 8. B9-D1 Review Identity Fix

Initial B9-D verified functionality and persistence but exposed “Journey: Selected tour” and “Schedule: Selected schedule” in Review. Root cause: production Review used a preview-only placeholder resolver.

[Frontend PR #42](https://github.com/WonhoOne/frontend/pull/42):

- Branch: `fix/review-real-selection-identity-b9d`.
- Final head: `bf2e2f53a6fdf90f7bb955bf82f21fad343f7bb6`.
- Merge SHA: `f6a356ef3b2b86f492d56aff0f4ef31ec319671a`.
- CI [38031802893](https://github.com/WonhoOne/frontend/actions/runs/38031802893): **SUCCESS**.

The fix uses existing tour detail and tour schedules data sources to display actual `TourProduct.name` and selected schedule `dateLabel`, avoiding raw IDs/fabricated placeholders. It does not mutate Draft or change submission semantics.

Reported validation:

- Focused Review tests: 24/24 PASS.
- `npm run verify`: PASS; 108 files / 1887 tests.
- `npm run test:e2e -- --workers=2`: 196/196 PASS.

Shared Contract changed: NO  
Voice semantics changed: NO  
ReservationDraft schema changed: NO

## 9. Final Chrome manual smoke

After B9-D merge, project-supplied final smoke used the B9 final Frontend main `1e930fa88767dce065d838a75cc9ee38ad7bceeb`, actual Chrome, actual microphone and real Backend/MySQL.

**Final Chrome Real-Microphone Smoke: ✅ PASS — 10/10**

| # | Check | Result |
| --- | --- | --- |
| 1 | Start Voice | ✅ PASS |
| 2 | Select actual TourProduct through Voice | ✅ PASS |
| 3 | Select Style and actual Schedule through Voice | ✅ PASS |
| 4 | Configure participant, hotel, transport, meal and extra | ✅ PASS |
| 5 | “예약해 줘” booking safety: no automatic Reservation submit | ✅ PASS |
| 6 | Review actual Journey, Schedule and Voice-selected configuration; no placeholders | ✅ PASS |
| 7 | Signup / Login / Review return | ✅ PASS |
| 8 | Explicit GUI Reservation submit | ✅ PASS |
| 9 | Reservation Detail | ✅ PASS |
| 10 | Overall safety/regression | ✅ PASS |

Initial Auth failure was resolved by correcting local Backend startup configuration, as recorded below. The completed smoke has no unresolved product blocker.

## 10. Known local runtime incident and resolution

During local final smoke, `POST /api/v1/auth/signup` returned **502 Bad Gateway** through the Vite proxy. The Backend process was not running and localhost:8080 TCP was closed. The `b9-demo` startup had stopped because the local `JWT_SECRET` environment variable was unset.

Resolution: configure the local `JWT_SECRET`, start Backend successfully and verify localhost:8080 connectivity. Afterwards Signup, Login, Review return, explicit GUI Reservation submission and Reservation Detail all passed.

Classification: **resolved local runtime configuration issue**, not a product code defect. No product code change was required for this incident and no unresolved product blocker remains. No actual secret value, token, credential or secret-setting command is included in this record.

## 11. Final Gate decision

| Gate | Final recorded status |
| --- | --- |
| B9-A Backend Readiness | ✅ PASS |
| B9-B Frontend Live Integration | ✅ PASS |
| B9-C Voice Live Integration | ✅ FINAL PASS |
| B9-D Cross-Repo E2E | ✅ PASS |
| Final Chrome Real-Microphone Smoke | ✅ PASS |
| **Overall B9 Integration Gate** | **✅ FINAL PASS** |

This decision records the completed implementation and supplied final manual evidence. Shared SSOT approval of this closeout still follows the docs/main merge rule; an unmerged PR does not approve a new baseline.

## 12. Remaining work / next phase

Next phase is final submission preparation, presentation/demo preparation, document consistency/freeze and demo scenario freeze. Additional B9 implementation is not required by this closeout.

### Historical Frontend-local QA wording

[Frontend-local V9-QA.md at the final B9 snapshot](https://github.com/WonhoOne/frontend/blob/1e930fa88767dce065d838a75cc9ee38ad7bceeb/src/features/voice-bridge/V9-QA.md) contains historical pre-closeout wording, including “B9-C readiness: MANUAL DEVICE QA REQUIRED” and the next B9-D gate. It describes its earlier execution context; this docs-only branch does not edit it or retroactively change what V9 executed.

Once this closeout is approved on docs/main, implementation evidence and final gate status are superseded by this approved B9 closeout record. Manual device QA is complete, B9-C is FINAL PASS, B9-D is PASS and overall B9 is FINAL PASS. A Frontend owner documentation cleanup can link to the approved closeout while preserving the historical V9 execution record. This is a documentation follow-up, not an unresolved product gate.

## 13. Contract impact statement

Shared Contract changed: NO  
Business Rules changed: NO  
API Contract changed: NO  
ERD changed: NO  
ReservationDraft schema changed: NO  
Voice semantics changed: NO  
Requirements semantics changed: NO  
Product Catalog changed: NO  
VoiceCommand schema/version/canonical 11 commands changed: NO  
Backend public API changed: NO  
Reservation submit semantics changed: NO  
Ownership changed: NO

Only completed evidence and the B9 gate result are recorded. Existing [Baseline v0.2](baseline/BASELINE-v0.2.md), contracts and [repository responsibility](architecture/repository-responsibilities.md) remain the authority for implementation.
