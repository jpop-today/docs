# 실행 계획과 작업 큐

갱신 2026-10-16 KST · 상태 구분: planned / in_progress / blocked / verified / merged / shipped. AdSense는 별도 unknown / preparing / submitted / approved / rejected로 기록한다. 실제 Issue 생성 전에는 아래 키가 작업 식별자이며 GitHub Issue 번호로 쓰지 않는다.

직전 완료는 [v0.4.1](releases/v0.4.1-20261015.md)의 안전한 공식번호 조회·실패보존/표기비교가이드·동일M 실제운영/복구·Release다. 아래 초기 범위표는 설계 이력이며 현재 차단 상태는 [실행 상태](status.md)와 문서 하단 v0.4.1 이후 우선 작업을 따른다. 다음은 Issue6 미검토공식근거·자체콘텐츠, Issue15 Actions binding 안전설정, 문의/권리와 운영자 Google 실제계정 단계다.

직전 완료는 [v0.4.2](releases/v0.4.2-20261016.md)의 실제공식8번호/가수별업체확인곡수·동일M운영/복구/Release다. 다음은 미검토공식근거·자체콘텐츠·Actions설정·문의/권리/실제Google계정 단계다.

최신 완료는 [v0.4.3](releases/v0.4.3-20261016.md)의 차트업체별공식확인곡탐색·6번호/2보류·모바일겹침수정과동일M실제운영/복구/Release다. 다음은미검토근거·자체가이드·문의/권리·Actionsbinding·운영자Google실제단계다. 기존URL oracle의wwwKY/port/query차이는이번새E2E에서공식URL계약을공유해해결했다.

## 초기 설계 범위 이력

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

독립적으로 진행 가능한 작업은 파일 범위를 나눠 병행하고, ID/스키마/배포 연결은 한 조정자가 일관되게 통합한다. identity baseline·staging·호환 어댑터와 Astro 앱 미리보기를 구현했다. 초기 운영 전환은 v0.3.0에서 완료했고 v0.3.1·v0.4.0·v0.4.1을 실제 운영에 반영했다. 다음 실제 작업은 아래 미검토 근거·고유 콘텐츠·계정 및 자동배포 후속이다. 기간·승인 시점은 예측값을 사실로 기록하지 않는다.

GitHub Issue가 생성되면 담당 스레드, base SHA, 소유 파일, 검증 방법, 의존 Issue, 상태, 증거/PR 링크를 기입한다. 추후 장애·권한 변경은 operations/status.md에 남기고 독립 작업을 계속한다.

## 생성한 실제 작업 기록

- [docs#2 FOUNDATION](https://github.com/jpop-today/docs/issues/2): 이번 기준 수립과 저장소별 PR·검증·통합 기록.
- [jpoptoday#3 원본 staging·영속 ID/URL/storage](https://github.com/jpop-today/jpoptoday/issues/3): foundation 이후 첫 구현. 현재 열린 작업이며 source 운영 SHA/근거 보강을 포함한다.

그 외 표의 키는 계획 식별자로, 실제 Issue를 생성한 것으로 표시하지 않는다.

## 앱 미리보기 이후

- [jpoptoday#5 Astro 앱](https://github.com/jpop-today/jpoptoday/issues/5), [PR#7](https://github.com/jpop-today/jpoptoday/pull/7): 검색·정적3317 상세·기존 URL/storage·가이드/정책/차트, 로컬122계약/8Chromium·artifact 확인. CI·실제 preview·복구의 최신 상태는 릴리스 기록에서 확인한다.
- [jpoptoday#6 공식 번호 정정](https://github.com/jpop-today/jpoptoday/issues/6): 공식11그룹/23ID 대조에 따른 별도 correction ledger와 reviewed overlay. 현재 관측/staging을 보존하며 전체 검증 완료로 확대하지 않는다.
- 운영 전환: 내용·권리·문의·개인정보·차트 갱신 소유권과 동일 SHA CI/preview/복구 게이트, 기존 todayjpop 도메인/source 연결 확인. 새 앱의 검색·개인화 색인 정책과 reviewed 상세/가이드 sitemap을 별도 결정한다.
- AdSense: 공식 정책 조사 이후 실제 계정 상태·소유권·광고/CMP·Google 심사 제출·결과가 필요하다. noindex 관측 preview를 신청 사이트로 사용하지 않는다.

## 매일09시 실행

[jpoptoday#9](https://github.com/jpop-today/jpoptoday/issues/9): 실제 Codex 일일예약 등록, source Discord 보고·오전cron·버전릴리스, 원본그라데이션 복원, 새3명·총6개동료리뷰·교체 이력을 [일일기록](daily/2026-10-13.md)에 남긴다. 다음날 우선목표는 Issue#6 correction ledger/reviewed overlay와 운영콘텐츠·색인 게이트다. 새로운 실행은 최신원격기록·CI·release·CF환경부터 재확인한다.

## v0.4.3 이후 실제 우선 작업

[jpoptoday#13](https://github.com/jpop-today/jpoptoday/issues/13)의43근거정정·차트100stableID·기존jpop.today sameSHA전환/실제복구·remote14와v0.3.0은[실행증거](releases/v0.3.0-20261014.md)를기준으로완료한다. 전체카탈로그검증이나AdSense승인은아니다.

1. [jpoptoday#15](https://github.com/jpop-today/jpoptoday/issues/15): healthcheck 하네스/70회귀·실제25HTTP·복구/복원·sameM CI/remote19·v0.3.1은 [완료 증거](releases/v0.3.1-20261014.md)를 따른다. 남은 CF Actionsbindings 설정과 실제 자동배포 경로 확인은 안전한 운영자 설정 경로로 인계한다. root managed cloud 배포·독립 개발은 가능하다.
2. Issue#6 자료보강: [v0.4.0](releases/v0.4.0-20261015.md)의공식20번호·가수별확인곡/실제운영·복구·remote22까지완료했다. [Issue20](https://github.com/jpop-today/jpoptoday/issues/20)의안전추출/실패보존·표기비교가이드는[v0.4.1](releases/v0.4.1-20261015.md)로실제운영·복구·remote24/24·Release까지완료했다. 다음은새도구로아직미검토곡의공식제목/가수/버전별근거를대조하고실제자체비교본문을보강한다. 긴JS내Python검사문자열의안전한helper분리는비차단유지보수후속이다. 틀린값/판단보류/미확보를구분한다. 튜링러브KY는참번호미확보unknown 유지. 전체등록/권리허락을임의선언하지않는다.
3. 검토된번호와실제사용흐름을기반으로독창적인곡·가이드/가수비교본문을확대하고정정사례의문서/ledger동기화와모바일사용성을검증한다. 페이지수나변경량을심사보장기준으로쓰지않는다.
4. Google계정실제상태·소유권과Search Console/심사증거는operator입력을확인해기록한다. 광고/CMP/ads.txt/계정본인정보/법적동의는실제상태와provider공식절차에맞춰별도준비한다. 무광고운영전환/기술개선을계정unknown 때문에멈추지않는다.

매일09시와매시간재개점검은같은채팅이며실제첫hourly14:57:43KST를확인했다. 다른관리자active일때중복작업하지않는다. 현재실행이끝났다는것과오늘전체개발/최종승인이완료됐다는것을구분한다.
