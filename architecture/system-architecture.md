# System Architecture v0.1

## Components

```text
Customer GUI (React + TypeScript)
             |
             | REST API
             v
Backend (Spring Boot)
Business Rules / Application Logic
             |
             v
           MySQL

Voice Recognition / Employee Console
             |
             | REST API
             v
          Backend
```

## Core principles

1. Customer GUI는 Backend REST API를 통해 기능을 수행합니다.
2. Voice/Employee Console도 Backend REST API를 통해 기능을 수행합니다.
3. Frontend 및 Voice/Console은 MySQL에 직접 접근하지 않습니다.
4. Business Rule의 최종 검증은 Backend에서 수행합니다.
5. Database 접근은 Backend 책임입니다.
6. 공통 계약은 docs 저장소를 SSOT로 사용합니다.

## Technology baseline

- Frontend: React + TypeScript
- Backend: Spring Boot REST API
- Database: MySQL
- Version control: Git / GitHub
- API test: Postman
- Voice: Speech-to-Text 기술(Web Speech API 등 후보)
