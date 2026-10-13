# jpop.today 제품·운영 문서

한국어 사용자에게 정확한 JPOP 노래방 번호와 직접 편집한 선곡 정보를 제공한다. Google AdSense 승인 준비가 목표이며 승인 여부는 Google 심사 결과로 확인한다.

## 문서 지도

- [제품 스펙 v1](specs/product-v1.md): 대상 사용자, 범위, 기능별 완료 기준
- [구조와 저장소 경계](architecture/system.md)
- [ADR 001: 정적 웹과 점진적 이관](decisions/001-static-first.md)
- [데이터 계약과 출처](data/catalog-contract.md)
- [편집 콘텐츠 기준](content/editorial.md)
- [AdSense 준비](quality/adsense-readiness.md)
- [검증 하네스](engineering/harness.md)
- [여러 스레드 협업](engineering/thread-workflow.md)
- [Cloudflare 이관·배포·복구](operations/deployment.md)
- [환경 오류 해결](operations/environment-recovery.md)
- [현재 상태](status/current.md), [로드맵](roadmap.md)
- [2026-10-13 관찰 기록](audits/2026-10-13-baseline.md)

## 저장소 역할

| 저장소 | 책임 |
| --- | --- |
| [jpop-today/.github](https://github.com/jpop-today/.github) | 조직 규칙, 템플릿, 리뷰·협업 방식 |
| [jpop-today/jpoptoday](https://github.com/jpop-today/jpoptoday) | 앱, 데이터, 공개 콘텐츠, 테스트, 배포 설정 |
| jpop-today/docs | 제품 스펙, ADR, 운영 절차, 증거·작업 기록 |

공개 사이트의 글·안내문은 소스 저장소에 둔다. 이 문서 저장소를 런타임에서 읽지 않는다. 설계 변경은 ADR과 코드 PR을 서로 연결한다. 비밀 값은 커밋하거나 작업 기록에 쓰지 않는다.
