# Threadly

> 헥사고날 아키텍처 기반 MSA 소셜 미디어 플랫폼
> 신입~3년차 백엔드 개발자 포트폴리오 프로젝트

[![Java](https://img.shields.io/badge/Java-17-007396?logo=java)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.3.3-6DB33F?logo=spring-boot)](https://spring.io/projects/spring-boot)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql)](https://www.postgresql.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-7-47A248?logo=mongodb)](https://www.mongodb.com/)
[![Kafka](https://img.shields.io/badge/Apache%20Kafka-3.7-231F20?logo=apache-kafka)](https://kafka.apache.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker)](https://www.docker.com/)

---

## 📋 목차

- [프로젝트 소개](#-프로젝트-소개)
- [주요 기능](#-주요-기능)
- [기술 스택](#-기술-스택)
- [시스템 아키텍처](#-시스템-아키텍처)
- [프로젝트 구조](#-프로젝트-구조)
- [주요 구현 사항](#-주요-구현-사항)
- [인프라 & 운영](#-인프라--운영)
- [테스트 전략](#-테스트-전략)
- [트러블슈팅](#-트러블슈팅)
- [성과 및 개선점](#-성과-및-개선점)
- [시작하기](#-시작하기)
- [문서](#-문서)
- [연락처](#-연락처)

---

## 🎯 프로젝트 소개

**Threadly**는 게시물·댓글·팔로우를 중심으로 한 소셜 미디어 플랫폼으로, **헥사고날 아키텍처**와 **MSA(Microservice Architecture)** 를 기반으로 설계된 백엔드 시스템입니다.

### 개발 동기

신입~3년차 백엔드 개발자로서 실무에서 요구되는 **아키텍처 설계 역량**과 **운영 경험**을 갖추기 위해 시작했습니다. 단순한 CRUD API를 넘어, **서비스 분리, 이벤트 기반 통신, 무중단 배포, 모니터링**까지 실제 운영 환경에 필요한 전체 사이클을 구현했습니다.

### 핵심 목표

- ✅ **헥사고날 아키텍처**로 비즈니스 로직과 인프라 의존성 분리
- ✅ **MSA**로 서비스 간 독립성 확보 및 장애 격리
- ✅ **이벤트 기반 아키텍처**(Kafka)로 서비스 간 느슨한 결합
- ✅ **DDD & CQRS** 패턴으로 도메인 중심 설계
- ✅ **CI/CD 파이프라인**으로 자동화된 빌드/배포
- ✅ **무중단 배포**(Blue-Green)로 서비스 가용성 보장
- ✅ **모니터링 시스템**(Prometheus, Grafana, Loki)으로 운영 가시성 확보

### 프로젝트 구성

Threadly는 **2개의 독립적인 마이크로서비스**로 구성됩니다:

| 서비스 | 역할 | 데이터베이스 | 테스트 |
|--------|------|--------------|--------|
| **threadly-service** | 사용자, 게시글, 댓글, 팔로우 등 핵심 비즈니스 로직 | PostgreSQL, Redis | 111개 클래스, 531개 메서드 |
| **notification-service** | 실시간 알림, 이메일 발송, WebSocket 통신 | MongoDB, Redis | 31개 클래스, 164개 메서드 |

---

## ✨ 주요 기능

### 사용자 관리
- 회원가입 / 로그인 (이메일 인증)
- JWT 기반 인증/인가 (Access Token + Refresh Token)
- 프로필 관리 (이미지 업로드, 소개글)
- 팔로우 / 언팔로우

### 게시글 & 소셜 활동
- 게시글 CRUD (이미지 업로드 지원)
- 댓글 / 대댓글
- 좋아요 (게시글, 댓글)
- 피드 조회 (팔로잉 사용자 게시글)

### 실시간 알림
- WebSocket 기반 실시간 알림 푸시
- 알림 타입: 팔로우, 좋아요, 댓글, 대댓글
- 읽지 않은 알림 카운트
- 이메일 알림 발송 (SMTP)

### 배치 작업
- 소프트 삭제된 데이터 하드 삭제 (Spring Batch)
- 배치 작업 모니터링 (Prometheus Pushgateway)

---

## 🛠 기술 스택

### Backend
- **Java 17** - LTS 버전, Record/Sealed Class 활용
- **Spring Boot 3.3.3** - 최신 Spring 생태계
- **Spring Security 6** - JWT 인증/인가
- **Spring Data JPA** - ORM, 도메인 모델링
- **Spring Data MongoDB** - 문서형 데이터 저장
- **Spring Batch** - 대량 데이터 처리
- **Spring WebSocket** - 실시간 양방향 통신

### Database & Cache
- **PostgreSQL** - 관계형 데이터 (사용자, 게시글, 팔로우)
- **MongoDB** - 문서형 데이터 (알림, 로그)
- **Redis** - 캐시, 세션, 토큰 블랙리스트
- **Flyway** - 스키마 버전 관리

### Messaging & Event
- **Apache Kafka** - 이벤트 기반 서비스 간 통신
- **Spring Cloud Stream** - Kafka 추상화

### DevOps & Infra
- **Docker & Docker Compose** - 컨테이너화
- **GitHub Actions** - CI/CD 파이프라인
- **Nginx** - 리버스 프록시, 로드 밸런서
- **AWS EC2** - 애플리케이션 서버
- **AWS S3 + CloudFront** - 프론트엔드 정적 호스팅

### Monitoring & Observability
- **Prometheus** - 메트릭 수집
- **Grafana** - 대시보드 시각화
- **Loki + Promtail** - 로그 수집/검색
- **Spring Actuator** - 애플리케이션 헬스체크

### Testing
- **JUnit 5** - 단위 테스트
- **Mockito** - Mock 객체
- **AssertJ** - 가독성 높은 검증
- **Fixture Monkey** - 테스트 데이터 자동 생성
- **TestContainers** - 통합 테스트 환경
- **Jacoco** - 코드 커버리지 측정
- **K6** - 부하 테스트

> 📖 상세한 기술 선정 이유는 [기술 스택 및 선정 이유](https://github.com/KimGyuBek/Threadly/wiki/기술-스택-및-선정-이유) 참고

---

## 🏗 시스템 아키텍처

### 전체 시스템 구성도

<!-- 이미지 위치: threadly_docs/images/system-architecture.png -->
![시스템 아키텍처](images/system-architecture.png)

### 헥사고날 아키텍처 구조

Threadly는 **포트와 어댑터 패턴**을 기반으로 비즈니스 로직과 인프라를 분리했습니다.

```
threadly-service/
├── threadly-apps/              # 진입점 (Application Layer)
│   ├── app-api/               # REST API
│   └── app-batch/             # Spring Batch
├── threadly-core/              # 비즈니스 로직 (Core Layer)
│   ├── core-domain/           # 순수 도메인 모델
│   ├── core-service/          # Use Case 구현
│   └── core-port/             # 포트 인터페이스
├── threadly-adapters/          # 인프라 (Adapter Layer)
│   ├── adapter-persistence/   # JPA, PostgreSQL
│   ├── adapter-redis/         # Redis 캐시
│   ├── adapter-storage/       # 파일 저장소
│   └── adapter-kafka/         # Kafka Producer
└── threadly-commons/           # 공통 유틸리티
```

> 📖 아키텍처 상세 설명: [헥사고날 아키텍처](https://github.com/KimGyuBek/Threadly/wiki/헥사고날-아키텍쳐), [MSA](https://github.com/KimGyuBek/Threadly/wiki/MSA)

### 서비스 간 통신 흐름

<!-- 이미지 위치: threadly_docs/images/service-communication.png -->
![서비스 통신](images/service-communication.png)

1. **threadly-service**가 게시글 좋아요 이벤트를 Kafka에 발행
2. **notification-service**가 Kafka에서 이벤트를 수신
3. MongoDB에 알림 저장
4. WebSocket으로 실시간 알림 전송
5. SMTP로 이메일 알림 발송

> 📖 상세 플로우: [인증/인가 흐름](https://github.com/KimGyuBek/Threadly/wiki/인증-인가-흐름), [CQRS 설계](https://github.com/KimGyuBek/Threadly/wiki/CQRS-설계)

---

## 📦 프로젝트 구조

### Repository 구성

```
ecommerce/
├── threadly-service/          # 메인 비즈니스 서비스
├── notification-service/      # 알림 전담 서비스
├── threadly-front/            # React 프론트엔드 (S3 + CloudFront)
├── threadly_docs/             # 📍 메인 문서 (현재 위치)
└── Threadly.wiki/             # 📚 프로젝트 Wiki 문서
```

### 서비스별 역할

| 서비스 | 포트 | 데이터베이스 | 주요 책임 |
|--------|------|--------------|-----------|
| **threadly-service** | 8080/8081 (Blue/Green) | PostgreSQL, Redis | 사용자, 게시글, 댓글, 팔로우, 좋아요 |
| **notification-service** | 9080/9081 (Blue/Green) | MongoDB, Redis | 실시간 알림, 이메일 발송, WebSocket |

---

## 🎨 주요 구현 사항

### 1. 도메인 중심 설계 (DDD)

- **Aggregate Root**: User, Post, Comment, Follow 등 도메인 모델 정의
- **Value Object**: Email, Password, Nickname 등 불변 객체
- **도메인 이벤트**: PostCreatedEvent, PostLikedEvent 등

```java
// 도메인 모델 예시 (순수 비즈니스 로직)
public class Post {
    public void like(User user) {
        validateNotDeleted();
        validateNotOwnPost(user);
        // 도메인 이벤트 발행
        registerEvent(new PostLikedEvent(this.id, user.getId()));
    }
}
```

> 📖 상세 내용: [DDD 설계](https://github.com/KimGyuBek/Threadly/wiki/DDD-설계), [도메인 모델과 JPA 엔티티 분리 설계](https://github.com/KimGyuBek/Threadly/wiki/도메인-모델과-JPA-엔티티-분리-설계)

### 2. CQRS 패턴

- **Command**: 상태 변경 (CreatePostUseCase, LikePostUseCase)
- **Query**: 조회 (GetPostUseCase, GetFeedUseCase)
- 읽기/쓰기 모델 분리로 성능 최적화

> 📖 상세 내용: [CQRS 설계](https://github.com/KimGyuBek/Threadly/wiki/CQRS-설계)

### 3. 이벤트 기반 아키텍처

- Kafka를 통한 **비동기 이벤트 처리**
- 서비스 간 **느슨한 결합** 유지
- **최종적 일관성** 보장

> 📖 상세 내용: [MSA](https://github.com/KimGyuBek/Threadly/wiki/MSA)

### 4. 데이터베이스 분리 전략

- **PostgreSQL**: 관계형 데이터 (ACID 보장 필요)
- **MongoDB**: 문서형 데이터 (유연한 스키마, 읽기 성능 중시)
- **Redis**: 캐시 (세션, 토큰 블랙리스트, 조회수)

> 📖 상세 내용: [DB 분리 전략](https://github.com/KimGyuBek/Threadly/wiki/DB-분리-전략), [ERD](https://github.com/KimGyuBek/Threadly/wiki/ERD)

### 5. 보안 구현

- **JWT 인증/인가**: Access Token (15분) + Refresh Token (7일)
- **Refresh Token Rotation**: 토큰 재발급 시 갱신
- **Redis 블랙리스트**: 로그아웃한 토큰 무효화
- **Spring Security**: 필터 체인 기반 인증 처리

> 📖 상세 내용: [인증/인가 흐름](https://github.com/KimGyuBek/Threadly/wiki/인증-인가-흐름), [보안 정책](https://github.com/KimGyuBek/Threadly/wiki/보안-정책)

### 6. 스키마 관리

- **Flyway**로 스키마 버전 관리
- 환경별 마이그레이션 스크립트 분리 (dev, test, prod)
- 롤백 전략 수립

> 📖 상세 내용: [Flyway로 스키마 관리](https://github.com/KimGyuBek/Threadly/wiki/flyway로-스키마-관리)

---

## 🚀 인프라 & 운영

### CI/CD 파이프라인

<!-- 이미지 위치: threadly_docs/images/cicd-pipeline.png -->
![CI/CD 파이프라인](images/cicd-pipeline.png)

#### CI (Continuous Integration)
- Pull Request 시 자동 실행
- 단위 테스트 + 통합 테스트 실행
- Jacoco 커버리지 리포트 생성
- GitHub Actions Artifact 업로드

#### CD (Continuous Deployment)
- Master 브랜치 푸시 시 자동 배포
- Docker 이미지 빌드 → Docker Hub 푸시
- EC2 SSH 접속 → Blue-Green 배포 스크립트 실행
- Slack 알림 (성공/실패)

> 📖 상세 내용: [CI/CD 동작 프로세스](https://github.com/KimGyuBek/Threadly/wiki/CI-CD-동작-프로세스), [CI 파이프라인 구축](https://github.com/KimGyuBek/Threadly/wiki/CI-파이프라인-구축), [CD 파이프라인 구축](https://github.com/KimGyuBek/Threadly/wiki/CD-파이프라인-구축)

### 무중단 배포 (Blue-Green Deployment)

<!-- 이미지 위치: threadly_docs/images/blue-green-deployment.png -->
![Blue-Green 배포](images/blue-green-deployment.png)

1. **Green 환경**에 새 버전 배포
2. **헬스체크** 10회 재시도 (각 3초 대기)
3. 헬스체크 성공 시 **Nginx 트래픽 전환**
4. 기존 **Blue 환경** 종료
5. 배포 실패 시 **자동 롤백**

**배포 시간**: 평균 2분 이내
**다운타임**: 0초

> 📖 상세 내용: [무중단 배포](https://github.com/KimGyuBek/Threadly/wiki/무중단-배포), [배포 스크립트 가이드](https://github.com/KimGyuBek/Threadly/wiki/배포-스크립트-가이드)

### 운영 서버 구성

- **AWS EC2** (t3.medium)
  - Nginx: 리버스 프록시, 로드 밸런서
  - Docker Compose: 애플리케이션 컨테이너 관리
  - PostgreSQL, MongoDB, Redis, Kafka
  - Prometheus, Grafana, Loki 모니터링 스택

- **AWS S3 + CloudFront**
  - React 프론트엔드 정적 호스팅
  - CloudFront 캐시 무효화 자동화

> 📖 상세 내용: [운영 서버 구성](https://github.com/KimGyuBek/Threadly/wiki/운영-서버-구성), [Systemd 서비스 설정](https://github.com/KimGyuBek/Threadly/wiki/Systemd-서비스-설정)

### 모니터링 시스템

<!-- 이미지 위치: threadly_docs/images/monitoring-dashboard.png -->
![Grafana 대시보드](images/monitoring-dashboard.png)

- **Prometheus**: 메트릭 수집 (CPU, 메모리, 요청 수, 응답 시간)
- **Grafana**: 대시보드 시각화 (시스템, 애플리케이션, 비즈니스 메트릭)
- **Loki + Promtail**: 구조화된 로그 수집 및 검색
- **Pushgateway**: Spring Batch 작업 메트릭 수집

> 📖 상세 내용: [운영 환경 모니터링](https://github.com/KimGyuBek/Threadly/wiki/운영-환경-모니터링)

---

## 🧪 테스트 전략

### 테스트 구조

```
threadly-service/
├── 단위 테스트 (Unit Test)
│   ├── 도메인 로직 테스트 (Pure Java)
│   └── Use Case 테스트 (Mockito)
├── 통합 테스트 (Integration Test)
│   ├── API 테스트 (MockMvc)
│   ├── Repository 테스트 (TestContainers)
│   └── Kafka 테스트 (Embedded Kafka)
└── 부하 테스트 (Performance Test)
    └── K6 스크립트
```

### 테스트 커버리지

| 서비스 | 클래스 커버리지 | 라인 커버리지 | 테스트 수 |
|--------|-----------------|---------------|-----------|
| **threadly-service** | 85% | 78% | 531개 테스트 메서드 |
| **notification-service** | 72% | 65% | 164개 테스트 메서드 |

<!-- 이미지 위치: threadly_docs/images/jacoco-coverage.png -->
![Jacoco 커버리지](images/jacoco-coverage.png)

> 📖 상세 내용: [테스트 구조 및 전략](https://github.com/KimGyuBek/Threadly/wiki/테스트-구조-및-전략), [Jacoco 커버리지 측정](https://github.com/KimGyuBek/Threadly/wiki/Jacoco-커버리지-측정), [Fixture 기반 테스트 데이터 전략](https://github.com/KimGyuBek/Threadly/wiki/Fixture-기반-테스트-데이터-전략), [K6를 이용한 부하 테스트](https://github.com/KimGyuBek/Threadly/wiki/k6를-이용한-부하-테스트)

---

## 🔧 트러블슈팅

프로젝트 개발 및 운영 과정에서 마주한 기술적 도전과 해결 방법을 정리했습니다.

### 주요 트러블슈팅 사례

1. **[서비스 추가로 인한 운영 서버 다운](https://github.com/KimGyuBek/Threadly/wiki/서비스-추가로-인한-운영-서버-다운-트러블-슈팅)**
   - 문제: notification-service 추가 후 EC2 t2.micro 메모리 부족으로 서버 다운
   - 해결: 인스턴스 업그레이드 (t3.medium), Docker 메모리 제한 설정

2. **[알림 발행 실패 시 전체 트랜잭션 롤백](https://github.com/KimGyuBek/Threadly/wiki/알림-발행-실패-시-전체-트랜잭션-롤백-트러블-슈팅)**
   - 문제: Kafka 전송 실패 시 게시글 좋아요 트랜잭션까지 롤백
   - 해결: `@TransactionalEventListener` + `AFTER_COMMIT`으로 트랜잭션 분리

3. **[하드 딜리트 배치 성능 저하](https://github.com/KimGyuBek/Threadly/wiki/하드-딜리트-배치-성능-저하-트러블-슈팅)**
   - 문제: 대량 데이터 삭제 시 Spring Batch 성능 저하
   - 해결: Chunk 사이즈 조정, 인덱스 최적화, 병렬 처리

4. **[Spring Batch 메타 테이블과 Flyway 충돌](https://github.com/KimGyuBek/Threadly/wiki/Spring-Batch-메타-테이블과-Flyway-충돌-트러블-슈팅)**
   - 문제: Flyway가 Batch 메타 테이블을 관리하지 못함
   - 해결: `spring.batch.jdbc.initialize-schema=always` 설정 추가

5. **[Filter 단계에서 예외 발생 시 오류 응답 누락](https://github.com/KimGyuBek/Threadly/wiki/Filter-단계에서-예외-발생-시-오류-응답-누락-트러블슈팅)**
   - 문제: JWT 필터에서 예외 발생 시 `@RestControllerAdvice`가 처리하지 못함
   - 해결: 커스텀 `AuthenticationEntryPoint` 구현

6. **[Blue-Green 배포 시 업로드 파일 손실](https://github.com/KimGyuBek/Threadly/wiki/blue-green-배포-시-업로드-파일-손실-트러블-슈팅)**
   - 문제: 컨테이너 재시작 시 업로드된 이미지 파일 손실
   - 해결: Docker Volume 마운트로 영구 저장소 사용

> 📖 전체 트러블슈팅 목록: [Threadly Wiki - 트러블슈팅](https://github.com/KimGyuBek/Threadly/wiki/_Sidebar#트러블-슈팅)

---

## 📊 성과 및 개선점

### 프로젝트 성과

- ✅ **헥사고날 아키텍처**로 테스트 가능한 코드 설계
- ✅ **MSA 구축** 경험 (서비스 분리, 이벤트 기반 통신)
- ✅ **무중단 배포** 구현으로 서비스 가용성 100% 달성
- ✅ **CI/CD 파이프라인** 구축으로 배포 자동화
- ✅ **모니터링 시스템** 구축으로 운영 가시성 확보
- ✅ **테스트 커버리지 75% 이상** 달성
- ✅ **10개 이상의 트러블슈팅** 사례 문서화

### 통계

- **코드 라인 수**: 약 30,000 LOC
- **테스트 수**: 695개 (threadly-service: 531, notification-service: 164)
- **API 엔드포인트**: 50+ 개
- **Wiki 문서**: 50+ 개
- **커밋 수**: 500+ 개
- **개발 기간**: 6개월 (2024.05 ~ 2024.11)

### 향후 개선 계획

1. **성능 최적화**
   - 쿼리 성능 개선 (N+1 문제 해결, 인덱스 튜닝)
   - Redis 캐싱 전략 고도화

2. **테스트 보강**
   - notification-service 커버리지 80% 이상 달성
   - E2E 테스트 추가

3. **인프라 확장**
   - Kubernetes 마이그레이션
   - 멀티 리전 배포

4. **기능 추가**
   - 게시글 검색 (Elasticsearch)
   - 실시간 채팅
   - 추천 알고리즘

---

## 🚀 시작하기

각 서비스별 상세한 설치 및 실행 가이드는 아래 README를 참고하세요.

### 서비스별 README

- **[threadly-service README](../../threadly-service/README.md)** - 메인 비즈니스 서비스
- **[notification-service README](../notification-service/README.md)** - 알림 서비스
- **[threadly-front README](../../threadly-front/README.md)** - React 프론트엔드

### 빠른 시작 (Docker Compose)

```bash
# 1. 레포지토리 클론
git clone https://github.com/KimGyuBek/threadly-service.git
cd threadly-service

# 2. 환경 변수 설정
cp .env.example .env
# .env 파일 수정 (DB 정보, JWT Secret 등)

# 3. Docker Compose 실행
docker-compose up -d

# 4. 헬스체크
curl http://localhost:8080/actuator/health
curl http://localhost:9080/actuator/health
```

### API 문서

- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **API 명세**: [Threadly Wiki - API 명세](https://github.com/KimGyuBek/Threadly/wiki/API-명세)

---

## 📚 문서

프로젝트의 모든 설계, 구현, 운영 내용은 **Threadly Wiki**에 정리되어 있습니다.

### Wiki 주요 문서

#### 소개
- [프로젝트 개요](https://github.com/KimGyuBek/Threadly/wiki/프로젝트-개요)
- [기술 스택 및 선정 이유](https://github.com/KimGyuBek/Threadly/wiki/기술-스택-및-선정-이유)
- [API 명세](https://github.com/KimGyuBek/Threadly/wiki/API-명세)

#### 아키텍처
- [헥사고날 아키텍처](https://github.com/KimGyuBek/Threadly/wiki/헥사고날-아키텍쳐)
- [MSA](https://github.com/KimGyuBek/Threadly/wiki/MSA)
- [CQRS 설계](https://github.com/KimGyuBek/Threadly/wiki/CQRS-설계)
- [DDD 설계](https://github.com/KimGyuBek/Threadly/wiki/DDD-설계)

#### 데이터 설계
- [ERD](https://github.com/KimGyuBek/Threadly/wiki/ERD)
- [DB 분리 전략](https://github.com/KimGyuBek/Threadly/wiki/DB-분리-전략)
- [Flyway로 스키마 관리](https://github.com/KimGyuBek/Threadly/wiki/flyway로-스키마-관리)

#### 인프라 & 운영
- [운영 서버 구성](https://github.com/KimGyuBek/Threadly/wiki/운영-서버-구성)
- [무중단 배포](https://github.com/KimGyuBek/Threadly/wiki/무중단-배포)
- [CI/CD 파이프라인](https://github.com/KimGyuBek/Threadly/wiki/CI-CD-동작-프로세스)
- [운영 환경 모니터링](https://github.com/KimGyuBek/Threadly/wiki/운영-환경-모니터링)

#### 테스트
- [테스트 구조 및 전략](https://github.com/KimGyuBek/Threadly/wiki/테스트-구조-및-전략)
- [Jacoco 커버리지 측정](https://github.com/KimGyuBek/Threadly/wiki/Jacoco-커버리지-측정)
- [K6 부하 테스트](https://github.com/KimGyuBek/Threadly/wiki/k6를-이용한-부하-테스트)

#### 트러블슈팅
- [전체 트러블슈팅 목록](https://github.com/KimGyuBek/Threadly/wiki/_Sidebar#트러블-슈팅)

> 📖 전체 문서 목록: [Threadly Wiki 사이드바](https://github.com/KimGyuBek/Threadly/wiki/_Sidebar)

---
