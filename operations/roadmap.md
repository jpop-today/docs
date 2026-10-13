# 실행 계획과 작업 큐

2026-10-13 · 상태 구분: planned / in_progress / blocked / verified / merged / shipped. AdSense는 별도 unknown / preparing / submitted / approved / rejected로 기록한다. 실제 Issue 생성 전에는 아래 키가 작업 식별자이며 GitHub Issue 번호로 쓰지 않는다.

| 우선 | 키/스펙 | 범위와 산출물 | 의존/완료 기준 |
| --- | --- | --- | --- |
| P0 | FOUNDATION / 전체 | 세 repo 스펙·ADR·협업·하네스 기준 | 문서 링크와 계약 실행, PR/통합은 API 접근 후 증거 |
| P0 | SOURCE / SRC-001 | 개인 repo dev/main·운영 SHA·라이선스·ID baseline | 코드 조사 완료, CF 운영 SHA/프로젝트는 접근 필요 |
| P0 | IDENTITY / DATA-001,FAVORITES-001 | 최소 3,317 legacy ID·query·storage 어댑터 | 운영 SHA 추가 포함, ID 편집 회귀 테스트 |
| P0 | EVIDENCE / DATA-001,CONTENT-001 | 번호 출처·확인일 보강, KY 7개 충돌 검토, 자료 권리 목록 | 실제 대조, 임의 source/date 없음, raw 보존 |
| P0 | MIGRATE / SRC-001 | 기존 org 이력을 유지하는 원본 이력/코드 이관 | SOURCE/IDENTITY 기준 확보, 파일 출처·merge 전략 기록 |
| P1 | APP / SEARCH-001,SONG-001 | Astro/TS 최소 정적 앱과 검색/상세 | baseline·계약, 실제 빌드·타입·브라우저 회귀 |
| P1 | SAVE / FAVORITES-001 | 기존 storage/URL 호환 선곡 목록 | APP/IDENTITY, 미해결/손상/실패 보존 |
| P1 | CONTENT / CONTENT-001,SEO-001 | 검토된 기존 가이드·정적 상세·가수 정보 | 실제 자료 근거, 고유 본문·링크·검색 품질 |
| P1 | RELEASE / RELEASE-001,PRIVACY-001 | CI→preview→동일 SHA 운영·복구 | CF 접근, gates·공개 대상·문의/개인정보 검토 |
| P2 | ADS / ADS-001 | 운영 상태·정책 점검·심사 제출/결과 | RELEASE/CONTENT/EVIDENCE, 계정 본인정보 입력 |

독립적으로 진행 가능한 작업은 파일 범위를 나눠 병행하고, ID/스키마/배포 연결은 한 조정자가 일관되게 통합한다. 다음 실제 구현은 identity baseline과 원본 staging 어댑터다. 디자인·페이지 수를 늘리기 전에 이관 회귀를 고정한다. 기간·승인 시점은 예측값을 사실로 기록하지 않는다.

GitHub Issue가 생성되면 담당 스레드, base SHA, 소유 파일, 검증 방법, 의존 Issue, 상태, 증거/PR 링크를 기입한다. 추후 장애·권한 변경은 operations/status.md에 남기고 독립 작업을 계속한다.

## 생성한 실제 작업 기록

- [docs#2 FOUNDATION](https://github.com/jpop-today/docs/issues/2): 이번 기준 수립과 저장소별 PR·검증·통합 기록.
- [jpoptoday#3 원본 staging·영속 ID/URL/storage](https://github.com/jpop-today/jpoptoday/issues/3): foundation 이후 첫 구현. 현재 열린 작업이며 source 운영 SHA/근거 보강을 포함한다.

그 외 표의 키는 계획 식별자로, 실제 Issue를 생성한 것으로 표시하지 않는다.
