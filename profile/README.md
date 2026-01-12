# LG U+ 안녕하3조 – 대용량 통신 요금 명세서 및 알림 발송 시스템

##  프로젝트 개요
- 대용량 통신 요금 데이터를 기반으로
- 월별 요금 정산, 명세서 생성
- SMS / EMAIL 알림 발송을 수행하는 백엔드 시스템

##  Repository Structure
- **admin-web** : 관리자 웹 서비스 (Spring Boot)
- **billing-batch** : 정산/배치 처리 서비스
- **infra** : Docker, MySQL, Redis 인프라 설정

##  Architecture
- Spring Boot (Java 17)
- MySQL 8.0
- Redis
- Docker / Docker Compose
- Batch & Event-driven 구조

##  Team
- LG U+ 유레카 3기
- Backend Team
