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
8. [원본 staging](data/legacy-staging.md), [호환 어댑터](engineering/legacy-compatibility.md), [Cloudflare 기준선](operations/cloudflare-baseline.md)
9. [실행 상태](operations/status.md), [환경 복구](operations/environment-recovery.md)
10. [지속 개발과 세션 재개](operations/continuous-development.md), [공식 AdSense 정책 조사](operations/adsense-policy-review-20261013.md)
11. [공식 제공사 공유번호 대조](data/provider-evidence-20261013.md), [앱 하네스](engineering/app-harness.md)
12. [조직 앱 미리보기 릴리스](operations/releases/preview-20261013.md): 최종 SHA·CI·원격 검증·복구·인계

## 저장소 역할

| 저장소 | 관리 대상 |
| --- | --- |
| [jpop-today/.github](https://github.com/jpop-today/.github) | 조직 규칙, 기여·보안 안내, 작업·PR 템플릿 |
| [jpop-today/jpoptoday](https://github.com/jpop-today/jpoptoday) | 실행 코드, 계약, 테스트, 빌드·수집·배포 CI |
| 이 저장소 | 제품 스펙, ADR, 데이터·편집 기준, 실행 증거·상태·릴리스 |

운영 사이트: https://jpop.today · 원본: https://github.com/kim75503/todayjpop

현재 조직 소스는 Astro 정적 앱·관측 카탈로그·기존 URL/storage 호환 UI와 검증 하네스를 포함한다. 별도 Cloudflare 미리보기와 기존 개인 저장소의 jpop.today 운영 전환을 구분한다. 실제 SHA·CI·배포 상태는 [실행 상태](operations/status.md)와 릴리스 기록으로 확인한다.
