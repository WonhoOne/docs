# REST API Skeleton v0.1.1

> Status: **Approved skeleton**. Detailed API design remains a draft for v0.2.
>
> v0.1.1에서 아래의 **resource naming, endpoint path, HTTP method는 FIXED**입니다. 기존 endpoint skeleton을 유지합니다. Agent는 이 문서에 없는 endpoint를 만들거나 공통 계약으로 확정하지 않습니다.

`/api/v1/tours`와 `/api/v1/employee/tours`의 `tours`는 Domain의 `TourProduct` 리소스를 가리키는 고정 URL 이름입니다. `{tourId}`는 TourProduct 식별자입니다. `/api/v1/tour-schedules`는 `TourSchedule`, `/api/v1/reservations`는 `Reservation`, `/api/v1/employee/inventory`는 `Inventory`를 가리킵니다. `Theme`은 TourProduct의 분류이며 별도 Theme endpoint는 이 skeleton에 없습니다.

## Authentication / User

```http
POST /api/v1/auth/signup
POST /api/v1/auth/login
```

## TourProduct (`tours` path)

```http
GET /api/v1/tours
GET /api/v1/tours/{tourId}
```

## Tour Schedules

```http
GET /api/v1/tour-schedules
GET /api/v1/tour-schedules/{scheduleId}
```

## Reservations

```http
POST /api/v1/reservations
GET  /api/v1/reservations/{reservationId}
```

## Customer

```http
GET /api/v1/customers/me/travel-history
```

## Employee - TourProduct (`tours` path)

```http
GET  /api/v1/employee/tours
POST /api/v1/employee/tours
PUT  /api/v1/employee/tours/{tourId}
```

## Employee - Inventory

```http
GET  /api/v1/employee/inventory
POST /api/v1/employee/inventory
```

## Contract rules

- Request DTO, Response DTO, error response format, validation details, pagination, authentication token format은 v0.2에서 확정합니다. 인증 방식과 JWT/session 여부도 TBD입니다.
- `POST /api/v1/reservations`는 승인된 BR-07 / BR-13을 따라야 합니다. 일반 Reservation은 `participantCount` 1 이상이며, Honeymoon Reservation은 pair/couple 단위 제약을 충족해야 합니다. Backend가 이를 검증합니다. 구체적인 Request/Response DTO field representation은 v0.2에서 확정합니다. 일반 일정은 `participantCount` 합계로, Honeymoon 일정은 유효 Reservation들의 파생 couple/team 수로 모집 상태를 집계합니다.
- Customer/Employee 가입 시 `name`, `address`, `contact`를 최소 저장합니다. 구체적인 signup DTO 및 사용자 역할 표현은 v0.2에서 확정합니다.
- SMS는 TourSchedule 최초 확정 시 실제 전송되어야 하지만, 새 SMS endpoint를 이 skeleton에 추가하지 않습니다. SMS Provider는 TBD입니다.
- Frontend와 Voice/Console은 이 API를 통해 Backend와 통신합니다.
- 공개 API 변경은 docs 변경 및 영향 범위 검토 없이 임의로 수행하지 않습니다.
