
> 대용량 통신 요금 명세서(정산서) 발행 + 메시지(SMS/Email) 발송 플랫폼  
> 구성: **Admin Web(관리자 UI)** + **Billing Batch(정산 배치)** + **MySQL** + **Redis**

---

## 1) 프로젝트 구성(레포/모듈)

이 프로젝트는 아래 3개 레포(또는 3개 프로젝트)로 구성됩니다.

- **infra**
  - 로컬 개발용 인프라 구성 (Docker Compose)
  - 포함: `mysql`, `redis`, `admin-web`, `billing-batch`
  - 더미데이터(import) 및 개발환경 표준화(Flyway 적용) 담당

- **admin-web**
  - 관리자용 웹 애플리케이션
  - Thymeleaf 기반 관리자 UI + REST API 제공
  - 메시지 템플릿/예약/재시도 관리, 배치 상태 조회 등 관리자 기능 제공

- **billing-batch**
  - 정산 배치 및 스케줄러 실행 주체
  - 정산서 발행, 배치 실행 이력 관리, 메시지 발송 이벤트 발행(Redis Queue), 실패 재시도 트리거

---

## 2) 아키텍처 개요

- Admin Web(관리자) ↔ Billing Batch(정산 배치)
- Billing Batch → Redis 메시지 큐로 발행 → Messaging(발송 모듈 Mock) 처리
- 실패 재시도/상태 업데이트는 Admin Web에서 트리거 가능

> <img width="2130" height="974" alt="image" src="https://github.com/user-attachments/assets/6d74f3a2-bac3-4a8d-9864-b9abe4fbb9cd" />
- billing batch system 구조
  - detail_json : { 모든 청구이력의 상품 타입, 상품명, 청구금액, 할인종류별 할인금액, 청구일자 }
> <img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/2ff263c5-2f7f-44dc-b9e4-c2fadd43a094" />





## .env.example

팀원은 `.env.example`을 복사해서 `.env`를 생성 후 실행합니다.

```
MYSQL_ROOT_PASSWORD=change-me
MYSQL_DATABASE=billing_system
MYSQL_USER=app
MYSQL_PASSWORD=change-me

MYSQL_PORT=13306
REDIS_PORT=16379

SPRING_PROFILES_ACTIVE=docker

SPRING_DATASOURCE_URL=jdbc:mysql://mysql:3306/billing_system?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Seoul&characterEncoding=UTF-8
SPRING_DATASOURCE_USERNAME=app
SPRING_DATASOURCE_PASSWORD=change-me

SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.MySQLDialect
```

## 프로젝트 실행
```bash
# infra 폴더에서 실행
docker compose down -v
docker compose up -d --build
docker compose ps
```  

## 더미데이터 다운로드 링크
https://drive.google.com/file/d/1p9R3s_7uURPHEj7-Fr1QB6tjXsC3asiX/view?usp=sharing

infra폴더에 dump-data.sql 다운받으시면됩니다.

## 더미데이터 import (infra 폴더에서 실행)
```
docker compose exec -T mysql mysql -uapp -papp billing_system < dump-data.sql
```

## 접속  
Admin Web: http://localhost:8080  
Billing Batch API: http://localhost:8090

