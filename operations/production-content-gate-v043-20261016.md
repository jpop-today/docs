# v0.4.3 차트 확인곡 탐색 · 교체 담당 읽기 감사

작업 [Issue27](https://github.com/jpop-today/jpoptoday/issues/27) · 상위 [Issue6](https://github.com/jpop-today/jpoptoday/issues/6) · `resume-20261016-0400` · DATA-001/CONTENT-001/SEO-001/DESIGN-001/HARNESS-001/RELEASE-001. 감사 담당 `/root/replacement04_gate`, 통합·Git·공용 파일·CI·배포는 root다. 이 담당의 유일한 쓰기 파일은 이 문서다.

source base `ce1b49f1ab6cedb64dca7cd2b215e72aafc76e50`, docs base `5d1964dfeacdad5c6b2c81d2522ce3aadb83de05`, 조직 base `b7aad18fe2f793453b6505bb150f359d05ae3554`, branch `codex/27-chart-confirmed-20261016`을 [실행 계약](runs/resume-20261016-0400.json)·[제품 스펙](../product/spec.md) 및 실제 연결된 Issue27 본문에서 읽었다. 세 저장소 AGENTS와 상태·roadmap·daily-development·continuous-development의 운영 경계를 읽었다. 원본 소스·동료 문서·평가·실행 증거를 보존하며 새 점수를 만들지 않는다.

## 교체와 검토 범위

[실제 여섯 평가](runs/resume-20261016-0400-reviews.json)와 각 원문을 읽었다. 원 담당 세 명은 두 동료 합계가 모두 40/40이고 정확성·증거·사용자 가치·유지보수성 합계도 각각 10으로 완전 동률이다. 사전 `run.tieBreakOrder`의 app/evidence/quality 순서로 앱 담당을 현행 명단에서 제외하고, root가 실제 interrupt의 previous state `completed` 및 본 교체 spawn을 기록했다. 이는 품질 우열을 입증하지 않으며 물리 삭제가 아니다. 원 구현과 평가를 지우거나 차이를 만들기 위한 감점을 하지 않았다.

첫 감사 읽기는 `2026-10-15T23:15:34Z`(16일08:15KST)까지의 파일·기존 로그·이미지를 대상으로 한다. 저장된 안전 metadata·ledger·원71 복사본·동결 입력을 읽기 전용 비교했고 영속 PNG12개와 수정 전 모바일 PNG를 각각 직접 열었다. 공식 제공사 추가 요청·raw HTML 재파싱·테스트/빌드/서버/브라우저/CI/배포·Git 조작은 이 담당이 실행하지 않았다. 연결된 GitHub 요청은 공개 Issue27 읽기뿐이다.

## 자료·콘텐츠 정확성

[자료 검토 본문](../data/provider-review-20261016-0400.md), [실제 요청·후보·별도 결정 JSON](../data/provider-review-requests-20261016-0400.json), [root 독립 대조](runs/resume-20261016-0400-evidence-audit.json)와 현재 ledger를 읽었다. 실제 요청8개는 HTTP200·정확한 공식 request/final URL·문자열 번호·실제 checkedAt·응답 bytes/hash·전체 표시 제목/가수를 가지며 후보8개는 `unreviewed`로 보존된다. 별도 reviewer/reviewedAt/reason이 있는 결정8개 중 confirm6개만 ledger에 대응한다. 저장된 candidate/decision/새 ledger evidence의 ID·provider·번호·URL·시각·전체 제목/명의·hash·이유를 직접 대조했다.

| 동결 순위·곡 | TJ 결정 | 금영 결정 | 수용 근거와 한계 |
| --- | --- | --- | --- |
| 15 · ルカルカ★ナイトフィーバー / samfree | 27031 confirm | 43185 defer | TJ 전체 feature 명의가 동결 차트와 일치. 금영 `SAM feat.巡音ルカ`의 SAM=samfree 동일성은 확보하지 않아 관측 유지 |
| 16 · 逆光 / Ado | 68868 confirm | 76509 defer | TJ 영화 OST 부제 전체가 동결 차트와 일치. 금영 표시 제목의 `..` 생략 부분을 재구성하지 않음 |
| 17 · KICK BACK / 米津玄師 | 68677 confirm | 44855 confirm | 양 업체의 전체 OP 부제를 보존하고 명시적 공백·인용부호 차이만 대조. 오디오 동일성은 추정하지 않음 |
| 18 · レオ / 優里 | 52773 confirm | 57826 confirm | 번호·전체 제목/가수 일치. 다른 편곡/기기별 지원까지 확대하지 않음 |

원 ledger71 전체 객체·순서·최상위 metadata와 닫기 전 bytes prefix는 `/tmp/jpop-0400-base-ledger.json`과 직접 비교해 보존됨을 확인했다. 결과는77항목(confirm64/suppress13)이며 새 suppress는 없다. original71 ordered canonical SHA는 `35487404af46c08e2bf0471e0bf05fff088c0545b69639a57d9c928390797689`, 결과 ledger bytes SHA는 `a2d8ea8e9eba737a38a1f2968a0a1687498bf04fb0f8742ea7f42eedaef8f708`이다. staging/chart/identity/helper/global CSS의 실제 파일 bytes hash는 저장된 시작 hash와 모두 일치했다. 기존 수집실패/부분결과/쓰기 실패의 마지막 정상 보존 계약과 helper는 변경하지 않았다.

새4 unit의 전체 소스를 읽었다. 원71·동결 staging/chart·3317 ID·100개 순위/ID, exact6개 근거, 이전/이후 cohort projection의 전체 ID/URL·비대상 provider/identity 전체 객체를 검사한다. 같은 제목의 다른 Ado 곡으로 근거가 전파되지 않고 unknown/null을 유지하며, 보류KY2는 관측 문자열 번호·근거null로 남는다. 보류를 suppress/미수록으로 바꾸지 않으며 후속 독립 검토의 append를 금지하지 않는다.

공식 raw 응답은 저장하지 않았다. 저장된 metadata 사이 대응과 파일 hash는 독립 대조했지만 실제 응답 원문 재파싱/응답 hash 재계산은 불가능하며 수행했다고 주장하지 않는다. 번호 공개검색의 읽기는 재배포 허락·가사/이미지/음원 권리의 근거가 아니다. 자료 권리·SAM 별칭·생략 부제·오디오 동일성·기기별 지원·실제 Google 계정/신청/심사는 미확인이다.

## 앱·사용자 가치·유지보수성

`src/pages/chart.astro`, 기존 `confirmedNumbers`/`SongRow`와 새 E2E2개 전체를 읽었다. 차트의 frozen canonicalId join·100개 고유 ID·원래 순위·요청 기간/fetchedAt·TJ 원문 출처·보관차트/최신 아님/요청기간과 응답 집계기간 구분을 보존한다. `current/playlist`·provider-confirmed·충돌 없음·실제 근거/정확한 공식 numeric URL인 업체만 기존 계약으로 선별한다. 업체별 ID Set은 TJ19/금영17, 전체 합집합19/분모100이며 19+17을36곡으로 표시하지 않는다. 0 또는 목록 부재가 공식 미수록이 아님을 설명한다. 금영 목록도 기존 TJ 순위 순서이며 별도의 금영 차트를 만들지 않는다.

SSR 업체 바로가기→`chart-${stableSongId}` anchor→원100행→기존 곡 상세/공식 근거로 연결된다. fragment는 순위에 종속되지 않고 기존 URL·저장 ID와 복사/저장 구현을 건드리지 않았다. 새 클라이언트 JS·의존성·공용 CSS·계약을 추가/변경하지 않았다. 기존 blue `#4f7cff`→pink `#ff7aa8` 및 pink/mint/blue 배경을 재사용한다.

E2E는 UI의 집계 helper를 import하지 않고 served catalog와 동결 chart의 hash/100행/순위/URL·업체별 자격·합집합을 독립 계산한다. 공식 numeric URL만 기존 `officialLookupMatches` 계약을 재사용해 www금영·port·중복/추가 query와 다른 oracle을 만들지 않는다. 360px JS off에서 마지막 TJ·첫 금영 바로가기를 키보드로 열고 stable hash·viewport·실제 제목 hit target·기존 상세/공식 URL를 확인한다. 기존 storage/legacy URL 회귀를 대체하지 않는다. 범위를 좁힌 소스/회귀와 명시적 DOM 속성·native anchor는 후속 유지보수에 적절하며 현 시점 필수 코드 보완을 발견하지 않았다.

## 실제 모바일 결함·수정·시각 증거

[앱 문서](../engineering/chart-confirmed-navigation-20261016-0400.md), [품질 문서](../engineering/chart-confirmed-quality-20261016-0400.md), [실패 기록](runs/resume-20261016-0400-local-failures.json)을 읽었다. 첫 QA navigation 대기 누락은 임시 script 오류였고 앱 수정 없이 대기를 보완했다. 이어 자동 overflow/동작 PASS였어도 [수정 전 실제 PNG](runs/resume-20261016-0400-ui-before-fix-mobile-anchor.png)에서 공식 확인 badge와 금영 라벨 겹침·badge 세로 줄바꿈이 실제로 보였다. 이를 통과로 숨기지 않았다.

페이지 한정 CSS는 양 업체를 label/button grid와 아래 한 줄 badge로 분리하고 번호/저장 hit target을44px로 키웠다. 최종 [QA JSON](runs/resume-20261016-0400-ui-qa.json)은 root 재빌드 후 `2026-10-15T19:15:18.515Z–19:15:24.787Z`에 실제 실행한 dirty preview다. 1440×1100 JS on·360×800 JS off·1280×768 JS on의 source base/dirty:true 및 catalog·ledger·chart semantic hash가 일치한다. 본 감사의 새 브라우저 실행이나 clean 원격 검증으로 확대하지 않는다.

직접 연 영속12PNG는 `engineering/design/chart-confirmed-20261016-0400-`의 desktop/mobile/short 각각 first·summary·last-shortcut·anchor다. 업체별19/17/전체19·100분모와 설명, 실제 기존81위 마지막 바로가기/원행·상세 연결, 원래 그라데이션을 읽었다. 수정 후 mobile-anchor는 양 업체 라벨·번호·badge가 분리되고 단일행으로 보이며 짧은 desktop도 마지막 link가 가려지지 않는다. 모바일 JSON 전체100행은 번호 버튼181개 최소94×44px·저장100개44×44px·badge44개 최대높이16px를 기록한다. 새 영속 E2E의100행 bbox 검사는 overflow만으로 놓쳤던 provider 내부 겹침/세로 badge를 직접 겨냥한다.

세 viewport의 pageerror/console warning·error/실패요청/광고요청0 및 가로 overflow false를 읽었다. [대비 영수증](runs/resume-20261016-0400-ui-contrast.json)의 실제 선택3표본은6.52/6.23/6.18:1이며 이 감사에서 새 pixel 대비 계산을 실행하지 않았다. 사이트 전체 대비/접근성·WebKit/Firefox·실제기기·스크린리더·필드 성능은 미실행이다.

## 실행 증거와 수용 단계

| 게이트 | 실제 읽은 근거·결과 | 감사 수용 경계 |
| --- | --- | --- |
| 전체 unit | `/tmp/jpop-0400-unit.txt`:314/314, fail/skip0,14,489.143116ms; 새 cohort4개 포함 | 현재 dirty local source의 root 실행 수용 |
| 타입 | `/tmp/jpop-0400-check-final.txt`:0 error/0 warning/0 hint | E2E 파일 수집까지 대신하지 않음 |
| 최초 E2E collection | `/tmp/jpop-0400-preview-e2e-final.txt`:JSON import에 type:json 누락, 브라우저28개 미시작 | 해결 전 실제 실패 보존;28개 브라우저 실패로 세지 않음 |
| 정정 후 preview | `/tmp/jpop-0400-preview-e2e-after-jsonfix.txt`:28 PASS/50.3s, 새2검사 각3.0s | readFileSync/JSON.parse 정정 후 실제 로컬 결과 수용 |
| production local | `/tmp/jpop-0400-production-e2e.txt`:28 PASS/50.5s, 새2검사 각2.9s | 실제 운영 origin 배포 증거와 구분 |
| artifact | run의 local preview3347files/3336HTML/0index, local production3347files/3336HTML/45index, 후보0.4.3 | root 로컬 기록 읽기. clean source M 전체 재현은 후속 |
| clean CI·전체 재현 | 아직 전달된 실제 후속 영수증 없음 | pending |
| 실제 preview HTTP·원격 브라우저 | 아직 전달된 실제 후속 영수증 없음 | pending; 로컬28을 원격 성공으로 복사하지 않음 |
| 실제 운영·이전 복구→새 복원 | 아직 전달된 실제 후속 영수증 없음 | pending; 기존v0.4.2 운영을 새0.4.3으로 표시하지 않음 |
| tag/GitHub Release·Release Discord·완료 Discord | 각각 실제 후속 영수증 없음 | 각각 pending; 시작 Discord sent와 구분 |

현 단계 결론은 **자료·앱·회귀·수정 후 로컬 UI를 통합하는 데 필수 보완 없음**이다. 이는 운영 배포/릴리스 게이트 전체 수용이 아니다. root가 후속 clean SHA/CI·두 target 전체 재현·실제 preview/운영·복구/복원·Release/두 알림의 실제 JSON을 전달하면 이 문서의 후속 항목에 각 증거를 실제 읽고 수용 또는 미확인을 추가한다. 계정 unknown을 기술 승인/Google 승인으로 바꾸지 않는다.

## 후속 수용 · clean M·CI·전체 재현·preview 생성

`2026-10-15T23:20:39Z` 이후 root 인계로 같은 감사에서 [실행 계약](runs/resume-20261016-0400.json)의 sourcePR, [CI 선택 결과](runs/resume-20261016-0400-ci.json), [전체 재현](runs/resume-20261016-0400-reproducibility.json), [preview 배포 전후 API](runs/resume-20261016-0400-preview-deployment.json)를 실제 읽었다. 아래는 첫 감사 이후 확인된 결과이며 위 최초 pending 이력을 덮어 성공으로 바꾸지 않는다. 이 담당의 새 외부 요청·검사·Git 실행은 없다.

- [PR28](https://github.com/jpop-today/jpoptoday/pull/28)의 기록은 H `eb4a70763f9c9c1bc9e0fde6d2cd703aafdb3d3e`에서 M `48c58d57b15aa775676229465fd13c390ccac620`으로 merged다. 네 H push/PR CI와 M app `38822746257`·contracts `38822746265`가 실제 success인 저장 결과를 읽었다. native PR 첨부는5초 timeout/unconfirmed이며 실제 첨부됐다고 해석하지 않는다.
- M main app의 선택 job 로그에는314 tests/pass314/fail0, 타입0 error/warning/hint, preview3347files/3336HTML/0index·source M와28 PASS/36.0s, production3347files/3336HTML/45index·같은 M와28 PASS/33.8s가 각각 실제 시각으로 있다. **CI runner 내부 브라우저 결과**로 수용하며 실제 Cloudflare origin 검사로 해석하지 않는다.
- root는 같은 clean M에서 target별 두 번의 동결 로컬 빌드를 수행했다. preview/production 각각 전체 경로 집합과 모든 파일 SHA가 같고3347files/3336HTML이다. metadata는 dirty:false·3317songs/current3313이며 catalog `742a490b8a6f2f0f2e45ae3663c6f659c959bd112391f0b25fa70d50f6bfe2d3`, ledger `13bfed1c4382365f9dfdd7b548a8874cc680de196772f0767eaec87a5a6743c3`, chart `971bacf9cb65e5b7176e8a5aa59394d2c46ce00d0676aa033178dc11ae7540b9`가 양 target/최종 UI 자료와 같다. 각 target 안의 전체 재현을 수용한다. target끼리 HTML이 동일하다는 뜻이나 전체 CI archive와의 대조 성공이 아니며, CI archive 전체 비교는 이번 run 미실행이다.
- actual 관리 binding 배포 전후 API에는 전용 프로젝트 `jpoptoday-preview`의 새 deployment `c606c798-e003-44af-8fac-4a0a61fe441f`, URL `https://c606c798.jpoptoday-preview.pages.dev`, 생성 `2026-10-15T23:20:31.359693Z`, 단계 success, commit M/dirty:false가 있다. 별도 프로젝트의 Cloudflare `environment:production` label은 jpop.today 운영 전환이 아니다. 새 preview 생성과 같은 M 연결만 수용한다.

이 인계 시 실행 기록의 preview HTTP는 `running`, 실제 원격 브라우저는 `dispatched_pendingactualresult`다. 따라서 **실제 preview HTTP·원격28·새 운영/이전 복구/새 복원·Release·두 알림은 아직 pending**이다. root 인계의 기존 운영은630c/v0.4.2/M3db이며 이 감사가 새 외부 조회로 재확인한 값은 아니다. 다음 실제 영수증을 읽기 전에는 새0.4.3 운영/Release 수용을 선언하지 않는다.

## 후속 수용 · 실제 c606 preview HTTP·원격 브라우저

`2026-10-15T23:22:51Z` 이후 같은 감사에서 [preview 실제 HTTP](runs/resume-20261016-0400-preview-http.json), [CI의 remotePreview 선택 job 로그](runs/resume-20261016-0400-ci.json), 실행 기록의 preview 결과를 실제 읽었다. 앞 단락의 pending은 당시 상태이며 이번에 다음 두 항목을 새로 수용한다.

- 실제 URL `https://c606c798.jpoptoday-preview.pages.dev`의 source M/target preview와 세 입력 hash를 확인하는 root HTTP 검사는 `23:20:57.608Z–23:21:00.795Z`에 **27checks/1attempt PASS**다. 전후 build-info·일반 catalog/home/robots/sitemap/chart·가이드/상세/artists/asset 본문 SHA·비공개 파일/일반 미존재 경로404가 동결 artifact와 일치했다. preview noindex/광고없음 경계와 같은 M의 실제 origin을 수용하며 모든 CDN 위치의 동시 상태로 확대하지 않는다.
- 실제 배포 preview workflow `38823033378`/job `116723906621`는 head M/completed/success다. 선택된 실제 job 로그에 `23:21:25.0740070Z`의 source M/target preview/attempt1/passedtrue HTTP 결과와 `23:22:32.3338636Z`의 **28 PASS/45.7s**가 있다. 이는 앞선 CI runner 내부28과 별개인 **실제 c606 preview origin 브라우저28** 결과로 수용한다. 빈 로그나 workflow 성공 표시만으로 세부 결과를 추정한 것이 아니다.

이 시점 root의 production 배포 명령은 진행 중이며 아직 운영 배포/HTTP 영수증을 받지 않았다. **새 운영 source M 전환·이전 복구→새 복원·운영 원격 브라우저·Release·Release Discord·완료 Discord는 계속 pending**이다. 이 담당은 새로운 HTTP/브라우저/Git/배포를 실행하지 않았다.

## 후속 읽기 · 실제 첫 운영 응답과 복구 실패 보존

같은 감사에서 [운영 배포 전후 API](runs/resume-20261016-0400-production-deployment.json)와 [첫 운영 HTTP](runs/resume-20261016-0400-production-first-http.json)를 실제 읽었다. 관리 binding 배포는 `23:23:07.011602Z` 생성된 `eb9161b3-d43b-45e4-beb8-9b068d456dbe`, commit M/dirty:false·success이며 todayjpop/jpop.today 도메인과 개인 `kim75503/todayjpop/dev` source metadata·자동 production false/preview none을 보존한다. 첫 canonical `https://jpop.today` 검사는 `23:26:46.899Z–23:27:16.333Z`에 source M/production·세 hash와 **엄격27checks/1attempt PASS**다. 새 운영 artifact가 실제 응답한 첫 증거를 수용한다. 이후 제어된 복구/복원 상태와 운영 원격 브라우저/릴리스의 최종 게이트는 별개다.

첫 [복구 요약](runs/resume-20261016-0400-recovery-node-summary.json)·old/new observer·[복원 HTTP 원 결과](runs/resume-20261016-0400-restore-node-http.json)를 읽었다. `23:27:25–23:28:17Z`의 old 복구 observer는12회 안에 exact metadata와 일반 home/sitemap이 동시에 맞지 않아 실패했다. new finally restore 후 observer는3번째에 일치했으나 이어진 엄격 HTTP12attempt는 첫 sitemap BODY_MISMATCH, 중간 coverage BODY_MISMATCH/catalog CATALOG_MISMATCH와 교대 metadata 불일치가 실제 있었고 마지막 METADATA_MISMATCH로 실패했다. 짧은 observer PASS를 full27 PASS로 바꾸지 않는다. 이 실패는 **이번 Issue27 run의 실제 결과**이며 과거v0.4.2 등의 실패를 복사한 것이 아니다.

두 번째 [복구 요약](runs/resume-20261016-0400-recovery2-recovery-node-summary.json)에서는 old/new API 전환과 observer가 모두 일치했지만, [root helper 오류 기록](runs/resume-20261016-0400-recovery2-helper-failure.json) 및 old/new execution 영수증은 각각 returnCode1이다. 임시 verifier의 maxAttempts60이 기존 지원 상한30을 넘어서 **HTTP 검사 시작/영수증 쓰기 전에** 거부됐다. 따라서 이 시도를27check 실패로 세거나 observer를27check 통과로 대체하지 않는다. 제품 소스 결함으로 혼동하지 않으며 원 helper 오류/두 execution 파일을 보존한다.

root 인계의 세 번째 제어된 시도는 지원30attempt·2초 retry·60초 total에서 동일 엄격27·API guard·finally restore를 유지하고 별도 영수증으로 진행 중이다. 이 감사는 아직 그 결과를 읽지 않았으므로 **복구/최종 복원·운영 원격 브라우저·Release·두 알림 수용은 pending**이다. 현재 읽은 자료에서 필수 제품 소스 수정은 발견하지 못했으며 혼합 응답/오류를 숨겨 게이트를 통과시키지 않는다.

## 후속 수용 · 세 번째 실제 복구·최종 복원

완료 `2026-10-15T23:30:32.038640Z`의 [recovery3 요약](runs/resume-20261016-0400-recovery3-recovery-node-summary.json)과 **두 API·두 observer·두 엄격 HTTP 영수증 전부**를 실제 읽었다. 첫/둘째 시도 실패는 위 기록과 원 파일에 그대로 남아 있으며 새 별도 영수증의 통과로 이전 실패를 덮지 않는다.

| 실제 단계 | 읽은 근거 | 수용 결과 |
| --- | --- | --- |
| 이전 버전으로 API 복구 | [rollback API](runs/resume-20261016-0400-recovery3-rollback-node-api.json) | eb916/M48c→630c/M3db; 동일 프로젝트·도메인·source metadata·자동false/none 보존 |
| 이전 버전 상태 관측 | [old observer](runs/resume-20261016-0400-recovery3-old-state-node-observed.json) | 23:29:42.174Z의1관측에서 exact metadata/home/sitemap 일치. 단독으로 full27을 대신하지 않음 |
| 이전 버전 canonical 본문 | [old HTTP](runs/resume-20261016-0400-recovery3-rollback-node-http.json) | 23:29:50.243Z–23:30:00.920Z, source M3db/production·이전 입력 hash와 엄격27checks/1attempt PASS |
| 새 버전 finally 복원 API | [restore API](runs/resume-20261016-0400-recovery3-restore-node-api.json) | 630c/M3db→eb916/M48c, 원래 새 deployment로 복원 |
| 새 버전 상태 관측 | [new observer](runs/resume-20261016-0400-recovery3-new-state-node-observed.json) | 첫 관측 old metadata/sitemap 불일치 보존,23:30:10.037Z의2번째에서 exact metadata/home/sitemap 일치 |
| 새 버전 canonical 본문 | [new HTTP](runs/resume-20261016-0400-recovery3-restore-node-http.json) | 23:30:10.412Z–23:30:32.027Z, source M48c/production·새 입력 hash와 엄격27checks/7attempt 최종 PASS |

새 full HTTP의 첫6attempt는 실제 실패다. 순서대로 coverage BODY_MISMATCH, metadata 불일치, chart BODY_MISMATCH, sitemap BODY_MISMATCH, Ado 역광 상세 BODY_MISMATCH, metadata 불일치가 있고 **7번째23:30:29.047Z 시작 검사에서 전후 metadata를 포함한27개가 모두 통과**했다. 성공 attempt만 남기거나7회 모두 통과했다고 쓰지 않는다.30attempt 상한·2초 retry·60초 total/요청8초·4MiB 상한은 지원 범위의 명시적 실행 예산이며 확인할 본문/정확한 hash/metadata 조건을 약화하지 않았다. 원 helper의 지원 밖60 설정은 그대로 오류 이력이다.

이로써 같은 M의 실제 운영 반영·실제 이전 정상 복구→새 버전 최종 복원을 수용한다. 관측 구간의 혼합/stale 응답 사실과 모든 CDN 위치를 보장하지 않는 한계를 보존한다. 실행 기록의 `production.state`도 `deployed_actualRecoveryPassed_remoteBrowserPending`이며 **운영 원격 브라우저·현재 Actions CFbinding 실제 결과·Release·Release Discord·완료 Discord는 아직 pending**이다. 새 검사/Git/외부 실행 없이 전달된 결과만 읽었다.

## 릴리스 전 최종 수용 · 실제 운영 원격 브라우저

`2026-10-15T23:33:38.836Z` 기록의 [remoteProduction 실제 선택 로그](runs/resume-20261016-0400-ci.json)와 [실행 상태](runs/resume-20261016-0400.json)를 같은 감사에서 읽었다. workflow `38824028665`/job `116727134158`는 같은 M48c·completed/success이며, 선택 로그에서 다음 실행 경계를 확인했다.

1. 314unit/type0와 같은 M의 preview0index·production45index artifact를 검증했다.
2. `23:31:29.7906107Z` 실제 preview strict HTTP는1attempt PASS다.
3. `23:32:17.0815998Z`의28 PASS/26.3s는 runner의 동결 production artifact 검사다.
4. `23:32:17.2779723Z` **Cloudflare Actions bindings missing/deployment skipped**가 실제 기록돼 있다. 관리 cloud binding으로 앞서 수행한 eb916 배포와 구분하며, Actions 자동배포가 성공했다고 쓰지 않는다. 기존 자동화 경로의 미설정 차단은 남아 있다.
5. `23:32:20.6951805Z`의 source M/target production strict HTTP는1attempt PASS이고, 이어 `23:33:00.9383190Z`의 **28 PASS/39.8s는 실제 jpop.today origin 브라우저**다. 앞선 runner local28을 원격 통과로 재사용한 것이 아니다.

이에 **동일 M의 CI·입력·전체 로컬 재현·실제 preview·운영·실제 복구/복원·운영 원격 브라우저 게이트를 수용**한다. 원71/staging/chart/3317 ID·문자열 번호·기존 URL/storage/unknown·브랜드 보존과 실제 근거/콘텐츠 공개 범위, 모바일 결함의 수정·회귀 및 noads/noindex preview 증거를 함께 읽은 범위에서 **릴리스 전에 반드시 고칠 제품 결함은 발견하지 못했다**. 앞선 실제 혼합 응답·helper 오류와 미실행 전체 CI archive/기기·보조기술 검사는 그대로 한계로 남는다.

버전 후보0.4.3의 기능/배포 게이트 수용과 실제 tag/GitHub Release 게시·Release Discord·완료 Discord는 각각 다른 사건이다. **이 세 발행/전송 항목은 아직 실제 영수증 대기**이며, 계정·권리·실제 Google 심사는 계속 unknown이다. 이번 감사는 root 후속 증거를 읽기만 했으며 새 실행·소스/Git 변경을 하지 않았다.

## 후속 수용 · 실제 v0.4.3 발행·Release 알림

같은 감사에서 [실제 Release 영수증](runs/resume-20261016-0400-release.json)을 읽었다. annotated tag `b13266ff60ffe4274c65890d25bdddf157cb043e`의 target type은 commit이며 target은 운영과 같은 M `48c58d57b15aa775676229465fd13c390ccac620`이다. [v0.4.3](https://github.com/jpop-today/jpoptoday/releases/tag/v0.4.3)의 actual API Release ID `414374401`, published `2026-10-15T23:36:29Z`(16일08:36:29KST), draft:false/prerelease:false를 수용한다.

release workflow `38824414759` 선택 로그에는 실제 v0.4.3 identity 검증,28 PASS/30.4s, 게시 URL이 있다. 같은 workflow의 별도 실제 송신 line은 `2026-10-15T23:36:30.4633974Z` **Discord notification sent/1attempt**이며 explicit Release 알림으로 수용한다. 완료 저장 보고의 송신 성공으로 바꾸지 않는다. 원 workflow/알림 경로의 binding만 사용한 결과이며 비밀 값/원문 로그/credential을 감사에 남기지 않는다.

또한 [여섯 평가 및 보충 읽기 원문](runs/resume-20261016-0400-reviews.json)의 `manager.replacementSupplementalAcceptance`를 실제 읽었다. 원 자료·품질 담당 두 명은 이 감사와 실제 배포/복구/CI 증거를 읽어 정확성·근거/검증·사용자 가치·유지보수성의 구체 근거를 각각 제출했고 모두 `accepted_pre_release_audit`/필수보완 없음이다. 새 점수나 순위는 만들지 않았다. 그들의 검토 시각에는 tag/Release/알림이 pending이었으므로 해당 원문 경계를 그대로 보존하고, 본 단락의 새 실제 발행 영수증 수용과 구분한다.

현재 **제품의 동일 SHA 게이트·실제 운영/복구·v0.4.3 tag/Release·Release Discord를 수용**한다. docs/report PR 발행·병합과 저장 완료 보고/Discord는 아직 후속 영수증 대기다. 권리·Google 실제 심사 unknown과 Actions CFbinding 자동배포 미설정은 계속 남아 있으며 릴리스 게시만으로 이를 완료로 만들지 않는다.

## 최종 보충 수용·freeze · 문서/저장 보고 병합·완료 알림

`2026-10-15T23:43:18.552Z`까지의 최종 인계에서 [docs·source 보고 병합 영수증](runs/resume-20261016-0400-source-report.json), [완료 Discord 영수증](runs/resume-20261016-0400-completed-discord.json), [최종 운영 API](runs/resume-20261016-0400-final-production-api.json), [최종 운영 HTTP](runs/resume-20261016-0400-final-production-http.json)를 실제 읽었다. 각각의 수용은 다음과 같다.

- [docs PR16](https://github.com/jpop-today/docs/pull/16)은 `23:41:03Z`에 merge `2d2cce4393031b7e4b6333ef17a1571fc0681600`으로 실제 병합됐다. [source 보고 PR29](https://github.com/jpop-today/jpoptoday/pull/29)는 `23:42:07Z`에 보고 R `312c6b687c504cf82090ad26bdd9d829b99a105f`로 실제 병합됐다. 보고 H의 push/PR app·contracts 네 검사는 모두 completed/success다. 보고 R은 제품/tag/운영 M48c와 `reports/daily/2026-10-16.json`, `reports/runs/resume-20261015-2300.json` **보고 두 파일만 다르며 제품을 새로 배포한 SHA가 아니다**. 두 PR의 native 첨부는 각각5초 timeout/unconfirmed로 남고 실제 GitHub URL/병합 사실과 구분한다.
- 저장 완료 보고 workflow `38825188067`/job `116730895180`는 reportDate `2026-10-16`, mode completed, source 보고 R, product M을 기록한다. 실제 선택 로그의 `23:42:45.5578626Z`는 prepared/delivery not yet confirmed이며, 그 뒤 `23:42:45.8935584Z` **sent/1attempt**가 실제 송신 성공이다. 준비 로그만으로 sent를 추정하지 않았고08:36의 Release 알림과08:42의 저장 완료 보고 알림을 분리해 수용한다. 시크릿·목적지·전체 환경·원문 로그를 보존하지 않는다.
- 최종 API는 `23:41:42.432530Z` todayjpop/jpop.today의 eb916/M48c/dirty:false 및 개인 source metadata·자동false/none을 확인한다. 이어 `23:41:47.923Z–23:42:09.094Z` canonical strict HTTP는 같은 M/production·동일 세 hash/본문의 **27checks/1attempt PASS**다. 앞선 복구 중 실제 혼합 응답 이력은 유지하며, 최종 API와 전체 canonical 검사 수용을 별도 연결한다.

이 읽기 시점 제품 M의 CI·운영 원격28·복구/복원·Release/두 Discord는 실제 증거로 수용한다. root의 인계에서 **보고 R의 main app CI는 아직 in progress이고 contracts는 pass**이므로 main app까지 통과했다고 쓰지 않는다. 이는 제품 M에서 이미 완료한 CI/운영 증거와 별개다. 이번 최종 영수증 및 이 문서 보충을 원격에 보존할 **후속 문서 PR은 root의 남은 작업/pending**이며 이미 원격 병합됐다고 주장하지 않는다.

freeze 직전 root의 추가 인계로 같은 [보고 영수증](runs/resume-20261016-0400-source-report.json)의 `mainCI`를 다시 읽었다. 보고 R312c6b6의 app `38825146435`와 contracts `38825146453`가 **각각 completed/success**로 저장돼 있다. 위 app pending은 이전 읽기 시각의 관측이며 이 후속 실제 결과로 보고 main CI도 수용한다. 제품 운영은 계속 M48c/eb916이다.

감사 담당은 이 문서 단독 편집을 여기서 freeze해 root에 인계한다. 필수 제품 보완 없음이라는 판단과 실제 미확인 한계(자료권리·Google 계정/신청/실제심사 unknown, Actions CFbinding 미설정, 전체 CI archive/실제 기기·보조기술/전체 접근성 감사 미실행)는 유지한다. 새 소스/Git/외부 조회·검사/배포·새 점수/worker는 실행하거나 생성하지 않았다.
