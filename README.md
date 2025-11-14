# Threadly

> 일상을 가볍게 기록하고, 가까운 사람들과 자연스럽게 연결되는 사진 기반 소셜 서비스

## 서비스 소개

문득 떠오르는 일상의 순간, 친구에게 조용히 들려주고 싶은 이야기,
기록해두고 싶은 작음 감정들.

그럴 떄 자연스럽게 꺼내 쓸 수 있는 공간이 있다면 어떨까요?

사진과 짧은 글 한 줄로 편하게 일상을 남길 수 있고

친구들의 소식을 살펴볼 수 있습니다.

누군가와 가볍게 연결되고 싶은 순간에 팔로우로 관계를 이어가고,

특정 친구에게만 보여주고 싶은 이야기는 따로 선택해서 공유할 수도 있습니다.

기록하고 싶은 날도, 사람들과 소통하고 싶은 날도

그 모든 순간을 자연스럽게 담아둘 수 있는 공간.

Threadly는 그런 순간을 위해 만들어졌습니다.

**링크:** [Threadly](https://threadly.kr)

### 특징

> Threadly는 단순한 SNS 구현을 넘어,
>
> 실무 서비스 수준의 구조와 운영 환경을 목표로 설계되었습니다.

- 헥사고날 아키텍처 기반의 명확한 계층 분리 및 설계로 유연한 확장성 확보
- `Kafka` 이벤트 기반의 MSA 구조 구현
- AWS `EC2` + `Docker Compose` 기반의 운영 환경 구성 및 실제 서비스 운영
- `S3`, `CloudFront`를 이용한 프론트엔드 배포 및 운영
- `Blue-Green` 전략을 이용한 무중단 배포 환경 구축
- `Prometheus`, `Grafana`, `Loki`를 활용한 모니터링 / 로그 수집 / 운영 가시성 확보
- `Github Actions` 기반이 완전 자동화 `CI`/`CD` 파이프라인 구축
- 테스트 전략, `Fixure` 기반 테스트, 부하 테스트 등 입증 가능한 테스트 환경

---

## 사용 기술 스택

### Backend
- Java 17
- Spring Boot (Security, JPA, WebSocket, Batch)
- Kafka 기반 비동기 이벤트 처리

### Database & Storage
- PostgreSQL (주 데이터베이스)
- MongoDB (알림 서비스)
- Redis (캐싱)
- Flyway (DB 마이그레이션)

### DevOps & Infrastructure
- Docker / Docker Compose
- GitHub Actions (CI/CD)
- Nginx
- AWS EC2, S3, CloudFront

### 모니터링
- Prometheus, Grafana
- PushGateway
- Loki + Promtail (로그 수집)
- Spring Actuator

### 테스트
- JUnit 5, Mockito, AssertJ
- Jacoco (커버리지)
- K6 (부하 테스트)
---

## 개발 범위 및 역할

### 백엔드

- 백엔드 전반 설계 및 구현
- MSA, 헥사고날 아키텍쳐, DDD, CQRS, Kafka 기반의 비동기 이벤트 처리 설계
- 인증/인가, 예외 처리, 로깅, CI/CD, 모니터링, AWS 배포 환경 구성 등 **백엔드 전 영역 직접 구현**

### 프론트엔드

- **`Codex`를 활용해 프론트엔드 구현**
- `Threadly` 백엔드 API와 연동될 수 있도록 구조 조정 및 일부 수정
- 배포 환경(`S3` + `CloudFront`) 구성 및 연결

---

## 서비스 구성도

![<img src="images/system-structure.png">](images/system_structure.png)

---

## Threadly 구성 서비스

## Backend(MSA 구조)
Threadly는 **MSA**기반으로 2개의 서비스로 구성되며,

각 서비스는 독립적으로 배포되고 `Kafka` 이벤트로 연결됩니다.

모든 백엔드 서비스는 **Docker** 기반으로 컨테이너화**되어

**AWS `EC2` 환경에서 무중단 배포 됩니다.**

> [백엔드 운영 환경 구성 위키 문서 보기](https://github.com/KimGyuBek/Threadly/wiki/%EC%9A%B4%EC%98%81-%EC%84%9C%EB%B2%84-%EA%B5%AC%EC%84%B1)

### threadly-service (메인 서비스)
  - 사용자, 게시글, 댓글, 검색 , 팔로우 등 **핵심 도메인을 담당하는 메인 서비스**
  - https://github.com/KimGyuBek/threadly-service
   
### notification-service (알림 서비스)
  - `Kafka` 메세지를 기반으로 **알림 처리를 담당하는 알림 서비스**
  - https://github.com/KimGyuBek/notification-service

## Frontend
웹 프론트엔드는 **Codex로 구현** 되었으며,

**AWS `S3` + `CloudFront`를 통해 배포됩니다.**

https://github.com/KimGyuBek/threadly-frontend

> [프론트엔드 운영 환경 구성 위키 문서 보기]()

---

## CI/CD 파이프라인 구축
Threadly는 **`Github Actions` 기반으로 테스트/빌드/배포까지 자동화된 파이프라인**을 구성했습니다.

[CI/CD 상세 위키 문서 보기](https://github.com/KimGyuBek/Threadly/wiki/CI-CD-%EB%8F%99%EC%9E%91-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)

---

## 트러블 슈팅
- [서비스 추가로 인한 운영 서버 다운](서비스-추가로-인한-운영-서버-다운-트러블-슈팅)
- [알림 발행 실패 시 전체 트랜잭션 롤백](알림-발행-실패-시-전체-트랜잭션-롤백-트러블-슈팅)
- [하드 딜리트 배치 성능 저하](하드-딜리트-배치-성능-저하-트러블-슈팅)
- [Swagger UI와 ResponseBodyAdvice 충돌 문제](Swagger-UI와-ResponseBodyAdvice-충돌-문제-트러블-슈팅)
- [게시글 삭제 시 연관 데이터 삭제 동기 처리로 인한 성능 문제](https://github.com/KimGyuBek/threadly-service/issues/75)
- [Spring Batch 메타 테이블 Flyway 충돌 문제](Spring-Batch-메타-테이블과-Flyway-충돌-트러블-슈팅)
- [Filter 단계에서 예외 발생 시 오류 응답 누락 문제](Filter-단계에서-예외-발생-시-오류-응답-누락-트러블슈팅)
- [Flyway 마이그레이션 체크섬 불일치 문제 트러블슈팅](Flyway-마이그레이션-체크섬-불일치-문제-트러블슈팅)
- [DB 환경 차이로인한 쿼리 오류 트러블 슈팅](DB-환경-차이로인한-쿼리-오류-트러블슈팅)
- [blue-green 배포 시 업로드 파일 손실 트러블 슈팅](blue-green-배포-시-업로드-파일-손실-트러블-슈팅)


---

## 위키 목록(https://github.com/KimGyuBek/Threadly/wiki)

- [소개](소개)
- [아키텍쳐](아키텍처)
- [데이터 설계](데이터-설계)
- [기술적 의사결정](기술적-의사결정)
- [개발 규칙 및 정책](개발-규칙-및-정책)
- [핵심 기능 구현](핵심-기능-구현)
- [인프라 및 운영](인프라-및-운영)
- [CI/CD](CI-CD)
- [테스트](테스트)
- [프론트엔드](프론트-엔드)
- [트러블 슈팅](트러블-슈팅)
