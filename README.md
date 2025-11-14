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

## Threadly 구성 서비스

Threadly는 다음과 같은 서비스로 구성되어 있습니다.

- 메인 서비스: https://github.com/KimGyuBek/threadly-service
- 알림 서비스: https://github.com/KimGyuBek/notification-service
- 프론트 엔드: https://github.com/KimGyuBek/threadly-frontend

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
