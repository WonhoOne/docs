# Requirements v0.1.2 proposal

## System purpose

허니문, 효도, 골프, 트레킹 등 테마별 맞춤형 여행상품을 고객이 선택하고 호텔, 교통, 식사 등의 세부 옵션을 조정할 수 있는 여행 서비스 시스템을 구현합니다. 고객은 웹 GUI와 제한된 음성 명령을 사용할 수 있고, 직원은 별도의 Console을 통해 여행상품과 관련 물품 재고를 관리합니다.

## Actors

### Customer

- 회원 기능 이용
- 여행상품 조회
- Theme 선택 및 해당 TourProduct 선택
- Tour Style 선택
- 호텔 / 교통 / 식사 등 세부 옵션 변경
- 여행 신청
- 로그인 후 과거 여행 이력 조회
- 정의된 주요 음성 명령 사용

### Employee

- 여행상품 기획 / 조회 / 수정
- 관련 물품 재고 추가 / 조회

## Functional requirements

| ID | Requirement |
| --- | --- |
| FR-01 | 고객과 직원의 회원 정보를 관리한다. Customer와 Employee 가입 시 최소 `name`, `address`, `contact`를 저장한다. |
| FR-02 | 고객은 TourProduct 여행상품을 조회할 수 있다. |
| FR-03 | 고객은 여행 테마를 선택할 수 있다. |
| FR-04 | 고객은 Tour Style을 선택할 수 있다. |
| FR-05 | 고객은 호텔, 교통, 식사 등 세부 옵션을 변경할 수 있다. |
| FR-06 | 고객은 선택한 TourConfiguration으로 특정 TourSchedule에 예약할 수 있다. 일반 Theme Reservation은 `participantCount` 1명 이상이며, `HONEYMOON_ROMANCE` Reservation은 couple/team 단위로 2명 이상의 짝수 `participantCount`를 사용한다. 한 Reservation은 여러 명 또는 여러 couple/team을 포함할 수 있다. |
| FR-07 | 시스템은 일반 일정의 신청 인원을 Reservation `participantCount` 합으로 집계하고, Honeymoon 일정의 모집 상태는 유효한 Reservation별 `participantCount / 2`에서 파생된 couple/team 수 합으로 집계한다. |
| FR-08 | 일반 Theme 일정은 신청 인원이 3명 이상이면 확정하고, Honeymoon 일정은 총 2 couples/teams 이상이면 확정한다. |
| FR-09 | 고객은 로그인 후 Travel History를 최근 여행 순으로 조회하며, 상품, 기간, Tour Style, 가격을 확인할 수 있다. |
| FR-10 | 직원은 여행상품을 기획·조회·수정할 수 있다. |
| FR-11 | 직원은 관련 물품 재고를 추가·조회할 수 있다. |
| FR-12 | 고객은 정의된 주요 여행 선택 명령을 음성으로 입력할 수 있다. |
| FR-13 | 음성 명령은 Backend 기능과 연동된다. |
| FR-14 | TourSchedule이 최초 확정될 경우 신청 고객에게 SMS를 실제로 전송한다. |
| FR-15 | 단골 고객에게 정의된 기준에 따라 할인 혜택을 적용할 수 있다. 단골 판정 기준, 할인율, 적용 시점, 중복 여부는 TBD이다. |

## Unresolved details

- Loyalty Discount의 단골 고객 판정 기준, 할인율, 적용 시점, 중복 여부는 아직 승인되지 않았습니다. Agent가 임의로 결정하지 않습니다.
- TravelHistory를 별도 Table로 저장할지는 TBD입니다.
- SMS Provider는 TBD이지만 최종 production/demo 경로는 실제 SMS 전송을 제공해야 합니다.

## Out of scope in v0.1.1

별도 팀 합의가 없는 한 다음 기능은 요구사항으로 간주하지 않습니다.

- 온라인 결제
- 환불
- 실제 호텔/항공 예약 시스템 연동
- 소셜 로그인
- 관리자 웹 대시보드
- 자유 대화형 AI 여행 추천
- 실시간 외부 여행상품 검색
