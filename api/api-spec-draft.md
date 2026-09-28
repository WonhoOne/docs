# REST API Skeleton v0.1

> Status: **Draft contract skeleton**
>
> Endpoint와 요청/응답 상세 형식은 v0.2에서 구체화합니다. Agent는 이 문서에 없는 endpoint를 공통 계약으로 확정하지 않습니다.

## Authentication / User

```http
POST /api/v1/auth/signup
POST /api/v1/auth/login
```

## Tours

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

## Employee - Tours

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

- JSON 요청/응답 DTO는 v0.2에서 확정합니다.
- 오류 응답 형식도 v0.2에서 확정합니다.
- Frontend와 Voice/Console은 이 API를 통해 Backend와 통신합니다.
- 공개 API 변경은 docs 변경 및 영향 범위 검토 없이 임의로 수행하지 않습니다.
