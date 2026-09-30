# Requirements v0.2

## System purpose

허니문, 효도, 골프, 트레킹 등 테마별 맞춤형 여행상품을 고객이 선택하고 호텔, 교통, 식사 등의 세부 옵션을 조정할 수 있는 여행 서비스 시스템을 구현합니다. 고객은 웹 GUI와 제한된 음성 명령을 사용할 수 있고, 직원은 별도의 Console을 통해 여행상품과 관련 물품 재고를 관리합니다.

## Actors

### Customer

- 회원 기능 이용
- 여행상품 및 일정 조회
- Theme에 속한 TourProduct 선택
- Tour Style 및 세부 옵션 선택
- TourSchedule에 Reservation 생성
- 로그인 후 완료된 Travel History 조회
- 정의된 주요 음성 명령 사용

### Employee

- TourProduct 기획 / 조회 / 수정
- 관련 물품 재고 추가 / 조회

## Functional requirements

| ID | Requirement |
| --- | --- |
| FR-01 | Customer와 Employee 계정을 관리한다. 가입 시 최소 `name`, `address`, `contact`를 저장한다. Public signup은 `loginId`, `password`, `name`, `address`, `contact`를 받고 CUSTOMER를 생성한다. EMPLOYEE 계정은 별도 public signup 없이 준비한다. |
| FR-02 | 고객은 TourProduct를 조회할 수 있다. 각 상품은 정확히 하나의 Theme에 속하며, Theme별 상품 수는 0개 이상이다. |
| FR-03 | 고객은 고정 Theme을 선택할 수 있다. Theme 전용 REST endpoint는 제공하지 않는다. |
| FR-04 | 고객은 Tour Style을 선택할 수 있다. `HONEYMOON_ROMANCE`와 `PARENTS_HEALING`은 `GRAND` 또는 `PREMIUM`만 선택할 수 있다. |
| FR-05 | 고객은 TourConfiguration의 Hotel, Transport, Meal 및 추가 옵션을 선택할 수 있다. 선택 값은 canonical ID를 사용한다. |
| FR-06 | 인증된 고객은 특정 TourSchedule에 `participantCount`와 최종 `configuration`으로 예약할 수 있다. 일반 범위는 1..10, Honeymoon 허용 값은 2, 4, 6, 8, 10이다. |
| FR-07 | 시스템은 일반 일정의 participant 합계와 Honeymoon 일정의 파생 couple/team 합계를 제공한다. Backend가 `recruitment` projection과 `reservable`을 판정한다. |
| FR-08 | 일반 일정은 신청 participant 합계가 3 이상이면, Honeymoon 일정은 신청 couple/team 합계가 2 이상이면 확정된다. |
| FR-09 | 고객은 본인의 완료된 Travel History를 최근 순으로 조회할 수 있으며 상품, 기간, 예약 당시 Tour Style, 가격을 확인한다. |
| FR-10 | Employee는 TourProduct를 생성·조회·수정할 수 있다. Theme에서 허용되는 각 Tour Style의 판매 가격을 설정한다. |
| FR-11 | Employee는 고정 품목 종류별 현재 재고를 조회하고 양의 수량을 추가할 수 있다. |
| FR-12 | 고객은 정의된 주요 여행 선택 명령을 음성으로 입력할 수 있다. |
| FR-13 | 음성 명령은 기존 Frontend 기능/API 경계를 통해 Backend 기능과 연동된다. 음성은 Reservation을 자동 제출하지 않는다. |
| FR-14 | TourSchedule이 최초 확정될 때 해당 Schedule의 각 고객에게 SMS를 실제 전송한다. 전송 실패는 예약/확정 결과를 되돌리지 않는다. |
| FR-15 | 새 Reservation 생성 전에 완료된 Travel History가 1건 이상인 고객은 subtotal의 5% Loyalty Discount를 받는다. 다른 할인과 중복 적용하지 않는다. |

## Shared contract summary

- Authentication: JWT Bearer Access Token. Token lifetime은 Backend-local 설정이며 Login의 `expiresIn`은 남은 seconds다.
- Reservation의 소유자는 인증된 Customer이며 request에서 `customerId`, `tourId`, `theme`, 가격 또는 할인 정보를 받지 않는다.
- TourProduct 가격은 TourStyle별 1인 단가(KRW 정수)다. Hotel/Transport/Meal/Extra 변경에 가격 차액을 적용하지 않는다.
- Inventory는 current stock aggregate다. Reservation 생성이나 일정 확정 시 자동 차감하지 않는다.
- Collection 응답은 plain JSON array이며 v0.2 pagination/envelope는 없다.
- `participantCount`의 초기 UI 값, null 상태, 명시적 선택 요구, 조작 방식과 배치는 Frontend-local이다. Shared Contract는 자동 기본값을 요구하지 않는다.

## Out of scope in v0.2

- 온라인 결제, 환불
- 실제 호텔/항공 예약 시스템 연동
- 소셜 로그인, Refresh Token, logout endpoint
- 자유 대화형 AI 여행 추천
- Theme, Option, Price, SMS 전용 REST endpoint
- TourSchedule CRUD, Reservation 취소/변경, Inventory 차감/수정/삭제 endpoint
- Inventory 수량에 따른 상품/예약 차단, 옵션별 가격 차액
