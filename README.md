# jpop.today 제품·개발·운영 기준

한국어 사용자가 JPOP 곡을 찾아 TJ·금영 번호를 비교하고 선곡을 준비하는 서비스. 장기 목표는 유용하고 출처가 분명한 서비스와 자체 편집 콘텐츠를 운영하며 Google AdSense 심사에 대응하는 것이다. 승인은 Google의 실제 심사 결과로만 기록한다.

## 처음 읽을 문서

1. [현재 조사 결과](discovery/current-state.md): 원본 코드·데이터·운영 접근의 확인 범위
2. [제품 스펙](product/spec.md): 사용자 흐름, 스펙 ID, 완료 기준
3. [아키텍처](architecture/overview.md)와 [설계 결정](adr/README.md)
4. [데이터 계약](data/catalog-contract.md), [검증 하네스](engineering/harness.md), [원본 UI 검증](engineering/baseline-qa.md)
5. [작업 계획](operations/roadmap.md), [협업 규칙](operations/collaboration.md)
6. [원본 이관](operations/migration.md), [배포·복구](operations/deployment.md)
7. [콘텐츠 기준](content/editorial-policy.md), [AdSense 준비](operations/adsense.md)
8. [실행 상태](operations/status.md), [환경 복구](operations/environment-recovery.md)

## 저장소 역할

| 저장소 | 관리 대상 |
| --- | --- |
| [jpop-today/.github](https://github.com/jpop-today/.github) | 조직 규칙, 기여·보안 안내, 작업·PR 템플릿 |
| [jpop-today/jpoptoday](https://github.com/jpop-today/jpoptoday) | 실행 코드, 계약, 테스트, 빌드·수집·배포 CI |
| 이 저장소 | 제품 스펙, ADR, 데이터·편집 기준, 실행 증거·상태·릴리스 |

운영 사이트: https://jpop.today · 원본: https://github.com/kim75503/todayjpop

현재 조직 소스는 독립 검증 하네스다. 개인 원본은 읽기·로컬 빌드가 가능해졌지만 아직 조직 저장소로 이관·운영 배포한 상태는 아니다. 이 문서는 새 개발 기준이며 기존 운영 상태와 구분한다.
