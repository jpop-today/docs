# v0.4.2 후보 · 독립 콘텐츠·운영 게이트 감사

작업 `resume-20261015-2300` · [Issue24](https://github.com/jpop-today/jpoptoday/issues/24) · 상위 [Issue6](https://github.com/jpop-today/jpoptoday/issues/6) · [PR25](https://github.com/jpop-today/jpoptoday/pull/25). 담당 `/root/replacement23_gate`, Git·공용 파일·서버·CI·배포·발행은 root 단독 담당이다.

## 감사 계약과 관측 시점

| 항목 | 실제 범위 |
| --- | --- |
| 관련 기준 | [제품 스펙 Issue24](../product/spec.md), DATA-001/CONTENT-001/SONG-001/SEO-001/DESIGN-001/HARNESS-001/RELEASE-001/ADS-001, [ADR-006](../adr/006-reviewed-corrections-and-production.md), [ADR-007](../adr/007-deployment-response-verification.md) |
| source / docs / 조직 base | `dffa93bc6982c9f186a269da577e3dbc108ad189` / `daa79c0828ad6906461f88a6dc5edef36842fd73` / `b7aad18fe2f793453b6505bb150f359d05ae3554` |
| branch / 읽은 PR head | `codex/24-provider-counts-20261015` / H `4e1ec00d5ca9d3505cc36b505d42478c9e140255` |
| 단독 편집 | 이 문서 한 파일. 나머지 소스·증거는 읽기 전용 |
| 최초 감사 시각 | 2026-10-15 23:22 KST까지 읽은 자료·로그·화면 기준 |
| 도구 관측 | 환경 revision231/current/running·connected·unrestricted/enforced, managed CF binding 준비 관측. 값·전체 환경·credential 파일은 읽거나 기록하지 않음 |
| GitHub 읽기 | Issue24 open, PR25 open/미병합, head H·base 위 source base를 직접 API로 확인. API의 미병합 `merge_commit_sha`를 실제 main M으로 해석하지 않음 |
| 첫 판정 | 자료·앱·로컬 검증 범위 수용. 이 범위에서 배포 전에 고쳐야 할 코드·콘텐츠 결함은 발견하지 않음. clean M·동일 SHA CI/원격·복구·발행 게이트는 아직 수용하지 않음 |

세 저장소 `AGENTS.md`, spec/status/roadmap/daily-development/continuous-development, ADR-006/007, [AdSense 기준](adsense.md), 실제 Issue24/PR25, [실행 계약](runs/resume-20261015-2300.json), [원본 6개 동료 평가](runs/resume-20261015-2300-reviews.json), 자료·앱·품질 담당 문서를 읽었다. 그 뒤 source ledger/staging/chart·관련 계약·앱 페이지·cohort unit/독립 E2E/기존 전체 E2E·artifact 검사 코드를 읽고 저장된 metadata와 현재 파일을 읽기 전용으로 비교했다.

새 하위 에이전트·Git 조작·테스트 재실행·빌드·서버·CI·제공사 조회·배포·Discord 전송은 이 감사에서 하지 않았다. 기존 raw HTML은 저장되지 않았으므로 응답 원문을 독립 재파싱하거나 response hash를 재계산했다고 주장하지 않는다. 현재 파일의 SHA-256 대조와 저장된 provenance의 일치 검사는 원문 재확인과 다르다.

## 실제 자료·콘텐츠 수용

[자료 담당 기록](../data/provider-review-20261015-2300.md)과 [metadata 전용 요청·별도 편집 결정](../data/provider-review-requests-20261015-2300.json)을 직접 읽었다. 동결 차트 수집일 `2026-10-13T10:26:45.525Z`, 요청 기간 `2026-09-29–2026-10-13`을 새 차트나 새 순위로 바꾸지 않았다. rank11–14 canonicalId가 staging의 정확한 current/playlist ID와 연결된다.

| 곡 / 원문 가수 | TJ / 금영 | 읽기 대조 |
| --- | --- | --- |
| Walking with you / Novelbright | `68419` / `44686` | 2개 전체 제목·가수 셀, 문자열 번호, 안정 ID 일치 |
| ドライフラワー / 優里 | `68333` / `44652` | 2개 전체 제목·가수 셀 일치. 같은 제목 Ado의 다른 ID는 대상이 아님 |
| 水平線 / back number | `68446` / `44701` | 2개 전체 제목·가수 셀의 소문자·공백 포함 일치 |
| 踊り子 / Vaundy | `68807` / `76508` | 2개 전체 제목·가수 셀 일치 |

저장된 요청8개 각각의 HTTP200·요청 URL=최종 URL·원 `verification: unreviewed` 후보와 별도 `/root/resume23_evidence` reviewer/판단시각/confirm 결정을 확인했다. 이 감사의 읽기 전용 비교에서 candidate↔decision↔실제 ledger append8개의 ID/provider/번호·전체 제목/가수·원 checkedAt·공식 URL·response hash가 8/8 일치했다. checkedAt은 실제 23:03:23–39 KST 조회 시각이고 별도 검토 시각은 23:04:46 KST다. parser 성공을 사람의 정체성 대조로 바꾸거나 이 감사 시각으로 갱신하지 않았다.

`/tmp/jpop-2300-base-ledger.json`과 현재 ledger를 직접 읽어 기존63개 전체 객체·순서와 최상위 metadata가 같음을 확인했다. 끝 confirm8만 추가돼 총71개(confirm58/suppress13)다. 마지막 닫기 전 bytes prefix 보존은 자료 담당의 저장된 관측이며, 이 감사는 기존 객체·순서와 결과 bytes hash를 독립 확인했다. staging·chart·identity baseline·안전 helper의 현재 bytes SHA-256 4개가 요청 JSON의 보존 hash와 같았고, `src/styles/global.css` bytes도 기존 `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889`와 같았다. 기존3,317 ID를 확인했다. 원자료·차트·수집 시각·문자열 번호·ID/기존 URL/localStorage를 변경하는 코드가 이번 범위에 없으며 기존 회귀 로그와 이어 확인한다. [root 독립 자료 감사](runs/resume-20261015-2300-evidence-audit.json)도 같은 경계를 명시한다.

확인 대상은 이 4ID·8번호의 공개 metadata 정체성이다. 원문 HTML·가사·이미지·주변 문맥은 저장하지 않았고 공개 projection은 내부 response hash를 제거한다. 실제 오디오·반주기 모델별 제공 범위·메타데이터 재배포 허락·가사/음원 권리·Google 계정/심사는 미확인이다. 공식 검색을 읽었다는 이유로 허락이나 승인을 선언하지 않는다.

## 앱·의미·호환·브랜드 수용

`src/pages/artists.astro`는 기존 `confirmedNumbers`/`buildConfirmedArtistGroups`를 그대로 사용한다. current/playlist·provider-confirmed·충돌 없음·검증된 공식 근거 판정에서 업체별 ID Set을 만들고, 전역 전체는 ID 합집합으로 센다. 실제 안전 탐색은 원문20가수 표기·24곡 ID·46번호(TJ24/금영22)다. 이는 전체 provider-confirmed TJ26/금영24에서 충돌을 제외한 범위이며, 46을 전체곡 수로 설명하지 않는다.

상단·가수 바로가기·각 그룹의 수치가 SSR 본문에 있고, 0은 ‘현재 관리 목록에 해당 업체의 확인 근거 없음’이며 미수록이 아니라고 명시한다. 모든 count 의미 연결은 `provider-count-note`와 `aria-describedby`, 그룹은 실제 `div[role="group"]`다. 기존 exact 원문 그룹·공동명의 분리·해시 anchor·상세 URL·전체 공식 제목/가수/원 대조 시각을 유지한다. 새 별칭·가수 약력·난이도·음역·권리 상태·승인 주장을 만들지 않는다.

[앱 담당 QA](../engineering/artist-provider-counts-20261015-2300.md), [최종 로컬 QA JSON](runs/resume-20261015-2300-ui-qa.json)과 영속 PNG6장을 이 감사도 전부 실제 열어 읽었다. 1440×1100/360×800 JS-off/1280×768의 상단 요약·두 열 바로가기·실제 금영0 그룹·마지막 가수 도달에서 숫자/원문명의/번호/근거·줄바꿈이 일치했다. body pink/mint/blue palette와 blue `#4f7cff`→pink `#ff7aa8` 로고·소개 상단선을 보존했다. QA의 82수치/20그룹 독립 집계·키보드 흐름·넘침/clip/overlay/콘솔·광고 요청0을 확인했다. 이 PNG/QA는 base SHA+`dirty:true` 최종 로컬 preview이며 clean M·원격 화면 증거가 아니다. 실제 스크린리더·WebKit/Firefox·필드 성능·실제 광고/CMP 검사는 미실행이다.

## 실제 검증과 실패 경계

이 감사는 아래 명령을 재실행하지 않고 실제 저장 로그·코드·영속 증거를 읽었다. [품질 담당 문서](../engineering/artist-provider-counts-quality-20261015-2300.md)의 담당 실행과 root 전체 결과를 중복 합산하지 않는다.

| 증거 | 수용 결과 / 한계 |
| --- | --- |
| `/tmp/jpop-2300-unit.txt` | root unit310 PASS·실패/skip0·11,481.144122ms. 새3개 cohort 검사에서 원63 객체/순서 hash·동결 입력·8개 exact tuple·모든 비대상 객체/ID/URL과 같은 제목 Ado의 observed/unknown 보존 확인 |
| `/tmp/jpop-2300-check-final.txt` | 최종 `div role=group` 포함 Astro check 26files, 0 errors/warnings/hints |
| `/tmp/jpop-2300-preview-e2e-final.txt` | 최종 로컬 dirty preview 전체26 PASS·44.6초. 독립 served catalog ID Set·global/index/group·0 의미·키보드/기존 URL/storage·무광고/preview noindex 회귀 포함 |
| `/tmp/jpop-2300-production-e2e.txt` | 최종 로컬 dirty production 전체26 PASS·45.9초. 실제 jpop.today 브라우저 검사는 아님 |
| 로컬 artifact / 실행 JSON | 양 target 각3346files/3336HTML, preview0 / production41 index. clean M 재현/원격 산출물 수용은 아직 별도 pending |
| [실제 초기 실패](runs/resume-20261015-2300-local-failures.json) | root whole preview 처음24PASS/2FAIL은 기존 coverage E2E의 과거 수치 고정 문제. 실제 served catalog 독립 집계로 정정 후26PASS. 숫자/근거 조건을 완화하지 않음 |
| 품질 초기 실패 | 별도 신규2E2E는 `24곡` 대신 실제 `24개 ID` 단위 가정 오류로 처음2FAIL, 정정 후2PASS. 초기 `p` artifact에는 role 검사가 없었고 최종 div/role 추가 뒤 root 재빌드/전체26 통과. 앞선 PASS를 최종 source로 올리지 않음 |
| 앱 초기 진단 실패 | 공식 URL 가정2회·brand 부모 선택자1회 오류를 QA 진단에서 정정. 제품 코드를 잘못된 기대값에 맞춰 바꾸지 않음 |

공개 artifact 검사는 private data/scripts/docs/reports·raw 가사/외부 이미지·내부 response hash·광고 코드와 target별 색인/정확한 sitemap·build-info 입력 hash를 검사하는 기존 게이트다. 단순히 root의 dirty 로컬 결과나 이 코드의 존재만으로 clean SHA/실제 배포 게이트를 통과했다고 쓰지 않는다. 기존 수집/파싱/쓰기 실패의 마지막 정상 자료 보존 계약도 그대로 유지하며, 이번 실제8조회 실패0을 전체 수집 무결성 보장으로 확대하지 않는다.

## 기존 6평가와 교체 후 보완

원본 6평가의 네 항목 근거를 모두 읽었다. 실제 합계는 자료40/앱40/품질39이고 유일 최저는 `/root/resume23_quality`다. root가 active roster 제외·interrupt(previous completed) 후 이 교체 감사를 생성했으며 물리 삭제는 하지 않았다. 원본 파일·평가·실패 증거는 보존한다. 이 감사는 새 점수·투표·순위를 만들지 않는다.

앱 reviewer의 품질 유지보수4 사유를 실제 E2E/계약과 읽어 확인했다. `officialLookupMatches`는 `www.kygabang.com`을 허용하고 명시적 port·여분/중복 query 키를 거부하지만, E2E의 독립 URL oracle은 KY canonical hostname만 허용하고 port·query의 정확한 키 개수/중복 조건을 그대로 검사하지 않는다. 현재 모든 실제 공식 근거는 정상 canonical URL여서 현재24/22/24·로컬26 결과에 영향이 없으며, 계약 validator가 공개 projection 전에 잘못된 근거를 거부한다. **현재 후보의 필수 수정은 아니다.** 향후 유효한 www 표기에서 잘못된 회귀 실패 또는 port/query 차이를 놓치는 일을 막도록 독립 oracle의 허용/거부 사례 정합성을 별도 유지보수 후속으로 남긴다. 독립 검증을 제거하거나 현재 숫자 조건을 완화하라는 결론이 아니며, 이 감사에서 코드 patch를 추가하지 않았다.

## 남은 배포·발행 게이트

최초 감사에서 직접 읽은 현재 운영 기준은 [run 시작 API](runs/resume-20261015-2300-baseline-api.json)와 [새 HTTP](runs/resume-20261015-2300-baseline-http.json)의 v0.4.1 M `d7cf8cb522da936450c9a08864abc99722f2f544` / `ebd045fa-e409-4306-9621-0858f391dee5`다. 첫 실패 attempt와 최종27 PASS를 구분한다. source Git metadata 개인 repo/dev·자동false/none 보존은 이 시작 관측이지 향후 고정 사실이 아니다.

| 게이트 | 23:22 KST 감사 판정 |
| --- | --- |
| 자료·콘텐츠·앱·로컬 unit/type/브라우저 | 위에 한정해 수용, 현재 필수 코드 보완 없음 |
| clean M PR/CI·입력 hash 연결 | pending: 아직 open H의 CI 진행 단계. 실제 main M/결과 증거 필요 |
| 두 target 고정 snapshot 재현 | pending: clean M의 전체 파일 집합·각 bytes hash 재현 증거 필요 |
| 동일 M 실제 preview/noindex·원격26 | pending: 실제 preview API/27HTTP·브라우저 결과 필요 |
| 실제 운영·복구/재복원 | pending: 현재/이전 deployment 확인 뒤 ADR-007의 실제 old27→new27 본문/metadata 검사와 현재 API·브라우저26 결과 필요. API pointer 성공만으로 수용하지 않음 |
| 버전/tag·GitHub Release·docs/report 병합 | pending: 후보0.4.2와 실제 게시를 구분, root 후속 증거 필요 |
| Discord | 시작 보고는 [기존 actual sent 영수증](runs/resume-20261015-2300-start-discord.json)이 있으며 최종 완료/Release 보고는 pending. workflow 성공만으로 실제 전송을 가정하지 않음 |
| Actions CF 자동배포 | 이전 missing 상태는 이 실행의 새 결과를 기다림. managed cloud 배포와 verify-only/배포 skipped·실제 자동배포를 구분 |
| AdSense | 기술·콘텐츠 preparing / 실제 계정·소유권·신청·Google 심사 unknown. 자료 권리·문의 실제 수신 처리·지역 광고/CMP·본인정보/법적동의는 별도 확인 |

root가 실제 후속 증거를 인계하면 같은 문서에 시각·M·검증 범위를 순차 추가한다. 아직 실행하지 않은 clean CI/복구·Release/Discord·Google 심사를 미리 수용하지 않는다. 공식8번호와 유용한 업체별 탐색의 개선은 전체 데이터 확인이나 AdSense 승인을 뜻하지 않는다.

## 23:26 KST 후속 · clean M·CI·로컬 재현·미리보기 API

2026-10-15 23:26:49 KST에 실제 PR25 API를 다시 읽어 `merged:true`, 병합 시각 `2026-10-15T14:21:28Z`, main M **`3db781ba1fab978995e8edddc0626c0e703bbfbf`**를 확인했다. 앞의 open H/게이트 표는 최초 감사 시점 기록으로 보존하고 아래 실제 후속 결과로 갱신한다. root가 초기 계약·자료·6평가를 docs branch `91f5202`에 push했다는 인계와 이 감사 문서의 발행/병합은 별개다.

[CI 영수증](runs/resume-20261015-2300-ci.json)의 actual M `app` [38750666866](https://github.com/jpop-today/jpoptoday/actions/runs/38750666866)·`contracts` [38750666745](https://github.com/jpop-today/jpoptoday/actions/runs/38750666745)의 success를 읽었다. PR H/push H와 main M 실행을 구분하며 `mainApp.safeResults`에 저장한 실제 step별 결과를 다음 범위로 수용한다.

| clean M CI 검사 | 저장된 실제 결과 |
| --- | --- |
| 단위·타입 | unit310 PASS·fail0, Astro check 26files·0 errors/warnings/hints |
| preview artifact·브라우저 | 3346files/3336HTML·0index, source M, 별도 브라우저26 PASS·27.9초 |
| production artifact·브라우저 | 3346files/3336HTML·41index, source M, 별도 브라우저26 PASS·29.3초 |

이 CI의 production artifact 브라우저 검사는 실제 jpop.today 원격 브라우저 결과가 아니다. 선택된 safe step 결과만 읽었으며 전체 Actions 로그·환경·자격 증명을 취득하지 않았다.

[재현 영수증](runs/resume-20261015-2300-reproducibility.json)은 root가 23:22:29–58 KST에 **target마다 clean local build 두 번**을 실행해 전체3346개 경로 집합과 모든 파일의 SHA-256이 같았음을 기록한다. preview/prod 둘 다3336HTML·3317ID/3313current·`dirty:false`/source M이며 index는0/41다. catalog bytes hash `e01e4fe87769c251b209f5fb82bfb3f46b094ec5fee0bfe0768277f6ecd01ff0`, semantic ledger `3c51501c77400f62fc43aed5b99ab87a82933546f89f597fe02449b117b433c8`, semantic chart `971bacf9cb65e5b7176e8a5aa59394d2c46ce00d0676aa033178dc11ae7540b9`가 두 target에서 같다. 전체 manifest hash는 preview `32d9f538b19c72ebbd55e3c1526ccf6ac62b5dc00e625a095aa356c6a39effe2`, production `044a711afe304889bfd14b08f4643dbe9b1c7084f8791aa76d8372e72904de83`다. 이는 **전체 clean 로컬 재현 수용**이며 전체 CI archive bytes를 다운로드·대조한 결과는 아니다. 이전 production37→41 index 증가는 실제 근거를 얻은 네 stable-ID 상세의 검토 경계이며 대량 관측 페이지의 일괄 색인으로 확대하지 않는다.

[실제 preview 배포 전후 API](runs/resume-20261015-2300-preview-deployment.json)에서 root managed cloud CLI exit0, `jpoptoday-preview`의 새 deployment **`a73d8e0a-93c3-4649-aa6d-298db8903d1b`**, 생성 `2026-10-15T14:25:19.431782Z`, deploy success·commit M·`commit_dirty:false`를 읽었다. 이 전용 preview 프로젝트의 Cloudflare `environment: production` 표시는 jpop.today 운영 전환이 아니다. 실제 preview HTTP/noindex·원격26 브라우저 성공은 이 API만으로 수용하지 않는다.

후속 판정: **clean M 병합·동일 M CI·전체 로컬 재현·preview API 배포까지 수용.** 실제 preview HTTP27·원격26, jpop.today 운영/브라우저26·old27→new27 복구/재복원, Actions CF bindings의 새 결과, tag/GitHub Release·docs/report 병합·최종/Release Discord는 이 시점에서 아직 pending이다. 새 증거가 생성되기 전 결과를 미리 채우지 않는다.

## 10월16일 03:03 KST · 같은 교체 감사 재개와 preview HTTP 수용

실제 사용량 한도로 후속 turn이 중단된 뒤 root가 같은 `/root/replacement23_gate`에 인계해 재개했다. 이 감사가 다시 읽은 clock은 `2026-10-15T18:03:24Z`(10월16일03:03:24 KST)다. 새 팀·평가·제공사 조회를 생성하지 않았고 기존 문서·소스·6리뷰·교체를 유지한다. root의 재개 관측은 환경256/current/running/enforced·CF ready, main 여전히 M·source clean/기존 docs diff 보존이다. 이는 root의 인계이고 이 감사의 이전 직접 환경231 관측과 시점을 구분한다. runId/시작일은 `resume-20261015-2300`/10월15일을 유지하되 향후 완료/Release 시각은 실제 10월16일 KST가 되면 그 날짜로 기록한다. 지난 공식 checkedAt을 재개 시각으로 바꾸지 않는다.

[실제 preview HTTP 영수증](runs/resume-20261015-2300-preview-http.json)을 읽어 source M·target preview·같은 catalog/ledger/chart hash와 **27checks/1attempt PASS**, 시작 `2026-10-15T14:26:08.726Z`·완료 `14:26:12.526Z`를 확인했다. metadata 전후200·동일 hash, query 없는 `/catalog.json`, 일반 홈·robots·sitemap·artists/가이드/대표상세·JS/CSS·private 경로404·실제없는 경로404가 frozen artifact 기대값에 모두 일치한다. `/artists/`의101,489bytes hash `d765db6a162c789be64bb28b41195bd302ef872deeec8e073674660fe9500248`와 preview robots/sitemap의 정확한 bytes 일치도 포함한다. 기존 상한(max12attempts/전체60초/요청8초/응답4MiB)을 유지했으며 이 결과를 모든 CDN 위치의 동시 일치로 확대하지 않는다.

같이 읽은 [CI JSON](runs/resume-20261015-2300-ci.json)의 remotePreview [38751360483](https://github.com/jpop-today/jpoptoday/actions/runs/38751360483)는 source M·completed/success·생성14:26:08Z/업데이트14:27:31Z이지만, 이 감사 시점 `safeResults` 배열이 비어 있다. 따라서 workflow success와 **실제 원격26 브라우저 결과줄 확인을 분리**하고, 후자는 root의 선택 로그 영수증이 채워질 때까지 pending으로 둔다. 현재 읽은 코드·자료·preview HTTP에는 배포 전 필수 보완이 없으며, 운영 배포·old27→new27 복구/재복원·운영26·Release·최종 Discord는 아직 수용하지 않았다.

### 03:09 KST · 원격 preview 브라우저26 수용

2026-10-16 03:09:11 KST에 위 CI JSON을 다시 읽었다. root가 connected GitHub `fetch_workflow_job_logs`로 실제 job `116478304418`을 읽고 선택 보존한 `Verify actual Pages behavior` 결과줄은 **`2026-10-15T14:27:28.8420768Z` / `26 passed (34.8s)`**다. source M·실제 Pages preview `a73d8e0a`의 동일 SHA workflow이며 앞에서 읽은 로컬/CI artifact 브라우저26과 별도의 원격 결과로 수용한다. 기록을 읽은 시각(16일03:08:50 KST)과 실행/검사 완료 시각(15일23:27:28 KST)을 구분하며 새 검사로 중복 가산하지 않는다.

처음 `gh --log` whole/job 읽기는 exit0·0bytes여서 결과줄 근거가 없었다. root가 기존 실행의 다른 연결 읽기로 실제 결과줄을 확보했고 추가 테스트 재실행 없이 `safeResults`에 보존했다. 앞의 빈 배열/pending 관측은 숨기지 않으며, **동일 M 실제 preview API·27HTTP·원격26 브라우저까지 수용**으로 갱신한다. 이 시점 운영 API는 root 재확인 기준 이전 `ebd045fa`/v0.4.1 M이고, root의 새 운영 배포·strict HTTP·old/new 복구/재복원·운영 브라우저·Release/최종 Discord는 실제 후속 영수증을 기다린다.

## 03:13 KST · 실제 운영 API·본문과 old/new 복구 수용

[운영 배포 전후 API](runs/resume-20261015-2300-production-deployment.json)를 읽어 root managed cloud CLI exit0과 실제 `todayjpop`/`jpop.today` 새 deployment **`630c12d8-341a-4ad0-b406-43e36eefd4b2`**·source M·dirty false·deploy success를 확인했다. 생성 시각은 `2026-10-15T18:09:22.241747Z`, **10월16일03:09:22 KST**다. 새 Git source metadata를 만들거나 개인 이력을 지우지 않았으며 owner `kim75503`/repo `todayjpop`/production branch `dev`는 남고 자동 production false·preview none을 유지한 실제 API 결과다. 이전 23시 시작 기준 v0.4.1 상태는 이 시점 새 운영 결과로 갱신한다.

[이전 동결 artifact 무결성](runs/resume-20261015-2300-fallback-integrity.json)은 old v0.4.1/source `d7cf8cb522da936450c9a08864abc99722f2f544`·3346files를 과거 전체 per-file manifest와 대조해 일치함을 기록한다. old manifest hash는 `b17ddc9823f359b39885785bee37db2333a06a451f66f0c31e13df366c371efc`다. 이번 새 ledger로 old artifact를 재빌드한 결과가 아니다.

| 실제 canonical `https://jpop.today` 검사 | 읽은 결과 |
| --- | --- |
| [최초 새 운영 HTTP](runs/resume-20261015-2300-production-first-http.json) | 03:09:39–03:10:15 KST, **최종27 PASS/2attempts**. 첫 attempt는 `/sitemap.xml` REQUEST_TIMEOUT이며5개 도중 종료, 두 번째 전체27개가 새 M artifact와 일치 |
| [이전 v0.4.1로 실제 rollback API](runs/resume-20261015-2300-rollback-node-api.json) | 실제 pointer `630c12d8`→`ebd045fa`·old source d7cf8cb로 변경, 개인 Git metadata/자동설정 보존 |
| [old 상태 관측](runs/resume-20261015-2300-old-state-node-observed.json) | 03:10:33–03:11:02 KST, bounded8회. 초기 metadata/sitemap 잔류·혼합을 보존하고 마지막 complete metadata+일반 홈/sitemap hash 일치. 이 상태 관측은 full verifier의 대체가 아님 |
| [old full HTTP](runs/resume-20261015-2300-rollback-node-http.json) | 03:11:10–17 KST, **27 PASS/1attempt**, old source·catalog/ledger/chart hash와 metadata 전후·일반 본문·자산·404 전체 일치 |
| [새 M으로 실제 restore API](runs/resume-20261015-2300-restore-node-api.json) | 실제 pointer `ebd045fa`→`630c12d8`·새 source M으로 재복원, 개인 Git metadata/자동설정 보존 |
| [new 상태 관측](runs/resume-20261015-2300-new-state-node-observed.json) | 03:11:21–38 KST, bounded8회. old metadata/sitemap 잔류·중간 혼합을 보존하고 마지막 metadata+일반 홈/sitemap 일치 |
| [new full HTTP](runs/resume-20261015-2300-restore-node-http.json) | 03:11:38–43 KST, **최종27 PASS/2attempts**. 첫 attempt의 metadata는 새 M이지만 catalog가 old hash `9c05e5dc…`/1,782,149bytes로 CATALOG_MISMATCH. 다음 attempt는 새 catalog `e01e4fe8…`/1,786,437bytes와 전체27개 strict PASS |

세 full HTTP 영수증의 마지막 attempt는 각27개 모두 `passed:true`, metadata 앞뒤 동일 hash임을 이 감사도 읽기 전용으로 집계했다. [복구 요약](runs/resume-20261015-2300-recovery-node-summary.json)의 old/new HTTP·상태 관측 true와 위 실제 개별 영수증이 일치한다. 전체 recovery는 03:10:30–03:11:43 KST다. 기존 Node24 inherited proxy/CA·정상 TLS와 strict metadata/일반 본문 비교를 유지하고 명시적60초/max12attempt/요청8초/응답4MiB 상한을 적용했다. timeout·잔류응답이 없었다고 쓰거나 상태 관측만으로 전체 복구 성공을 채우지 않는다. 잔류 응답의 구체적인 캐시 계층·전체 CDN 동시전환은 미확인이다.

후속 판정은 **동일 M 실제 운영 API·27HTTP·실제 old27→new27 복구/재복원까지 수용**이다. 현재 진행 중인 verify-only 운영 workflow `38783643157`의 실제 원격26 브라우저/HTTP·Actions binding 결과는 아직 읽지 않았고, 버전/tag·GitHub Release·docs/report 병합·완료/Release Discord 역시 실제 후속 증거 전까지 pending이다. 콘텐츠·자료 권리와 Google 실제 상태의 unknown 경계는 그대로다.

## 03:17 KST · 실제 운영 원격26·자동배포 차단·예약 관측

[CI 영수증](runs/resume-20261015-2300-ci.json)의 `remoteProduction`을 10월16일03:17:29 KST에 읽었다. verify-only [38783643157](https://github.com/jpop-today/jpoptoday/actions/runs/38783643157)는 source M·completed/success, 실제 connected GitHub job `116589984430`의 선택 로그를 다음과 같이 보존한다. 소스 `production.yml`의 해당 단계 순서와 로컬/원격 환경 변수도 읽어 서로 다른26개 결과를 구분했다.

| 실제 단계 / UTC 결과 시각 | 수용 결과 |
| --- | --- |
| same-source preview frozen bytes / `2026-10-15T18:12:43.9027323Z` | source M·preview·1attempt PASS. 앞선 전용 preview 검증의 독립 재확인 |
| 빌드된 production artifact의 로컬 E2E / `18:13:18.8630858Z` | **26 PASS·16.9초**. 실제 jpop.today 검사로 잘못 가산하지 않음 |
| Actions deployment bindings / `18:13:18.9215244Z` | 실제 `Deployment skipped: Cloudflare Actions bindings missing; managed cloud deployment remains available`. 이번 실행에서도 자동배포 자격 미설정이며 앞선 root managed 배포와 구분 |
| 실제 `https://jpop.today` production frozen bytes / `18:13:27.2756080Z` | source M·production·1attempt PASS. verify-only로 이미 반영된 실제 운영을 검증 |
| 실제 jpop.today 원격 E2E / `18:14:20.9179758Z` | **26 PASS·53.3초**. 기존 URL·저장·번호/근거·업체별 count·JS-off/모바일/키보드·crawler/private/광고 경계 전체 회귀 |

따라서 **현재 코드·콘텐츠·동일 M CI/preview·실제 운영 HTTP/브라우저·old/new 복구 게이트를 수용**한다. Actions의 verify-only success를 credentials 기반 자동배포 성공으로 쓰지 않으며, root managed CF binding 값을 Actions secret으로 복사하지 않는다. 자동배포 설정은 안전한 별도 운영 후속이고 이번 실제 운영 검증의 실패가 아니다.

[실제 예약 관측](runs/resume-20261015-2300-automation-observation.json)의 03:16:45 KST 조회에서 원래 daily09 `6acdccd99030819180ea1c9d907d9f55`와 hourly `6acf109ec1808191b09a2414e2de80af`가 모두 enabled·Asia/Seoul·일정 불변임을 읽었다. hourly 마지막 관측은 `2026-10-15T17:59:20.019138Z`, daily는 `2026-10-14T23:58:49.673277Z`이고 nextRunTime은 둘 다 null이다. 활성 설정/관측을 정확한 정시 실행·24시간 무한 세션·토큰 초기화 감지나 매시 성공 보장으로 바꾸지 않는다.

root는 annotated `v0.4.2` tag를 M에 push하고 release workflow `38784313891`이 진행 중이라고 인계했다. **진행 중 tag/워크플로와 GitHub Release 실제 게시를 구분하여 Release 수용은 아직 pending**으로 유지한다. docs/report 최종 병합·완료/Release Discord도 실제 영수증을 기다린다. 새 점수/투표·제공사 조회·빌드/Git/배포는 이 감사에서 추가하지 않았다.

## 03:20 KST · 실제 v0.4.2 Release·명시적 Release Discord 수용

2026-10-16 03:20:06 KST에 [Release 영수증](runs/resume-20261015-2300-release.json)과 [CI JSON](runs/resume-20261015-2300-ci.json)의 release 결과를 직접 읽었다. annotated tag object **`d271f6ae0c48bf05162ecdcec3e40e2ad110e183`**가 commit M **`3db781ba1fab978995e8edddc0626c0e703bbfbf`**를 가리키며 package/tag/notes 일치 검사를 통과했다. [v0.4.2](https://github.com/jpop-today/jpoptoday/releases/tag/v0.4.2)는 ReleaseID `414128773`, draft false·prerelease false, 실제 게시 시각 **`2026-10-15T18:18:49Z` = 10월16일03:18:49 KST**다. run의 10월15일 시작일이나 공식23:03 대조일과 구분한다.

[release workflow38784313891](https://github.com/jpop-today/jpoptoday/actions/runs/38784313891)은 source M·completed/success다. 실제 verify job `116592248938`의 `Reviewed release identity verified v0.4.2` 및 `2026-10-15T18:18:35.4197021Z`의 **26 PASS·29.9초** 선택 결과줄을 읽었다. release.yml의 기본 artifact E2E이며 앞선 실제 jpop.today 원격26(53.3초)과 별도다. publish job `116593096653`의 실제 Release URL과 **`18:18:50.5837730Z` `Discord notification sent (1 attempt).`**를 확인했다. 이 명시적 Release 알림은 GITHUB_TOKEN 이벤트가 다른 workflow를 시작했다고 가정한 것이 아니며, 최종 관리자 완료 보고 전송을 대신하지 않는다. secret 값·webhook URL·전체 로그·환경은 이 감사에 취득/기록하지 않았다.

### 제품 게이트 최종 판정과 네 기준 근거

| 기준 | 실제 수용 근거 / 경계 |
| --- | --- |
| 정확성 | 실제8개 metadata↔별도decision↔append 일치, 원63·3317ID/원본입력 보존, 동일제목 다른가수 분리. 정확 ID Set으로 TJ24/KY22·합집합24를 표시하고0≠미수록·원문그룹/URL/storage·브랜드를 유지. 현 후보 필수 코드·콘텐츠 보완 없음 |
| 근거/검증 | 직접 읽은310/type0/dirty26·26·6PNG/QA, clean M mainCI·두 target 전파일 재현, 실제 preview27/원격26, 운영27·old27/new27복구·운영원격26, tag→M·실제Release·명시적ReleaseDiscord. 초기실패·혼합응답·한도중단·조회 원문 비저장/독립재파싱 불가·전체 CI archive bytes 비교 미실행을 각각 보존 |
| 사용자 가치 | 방문할 업체의 확인곡 수를 가수 선택 전후로 비교하고 동결차트4곡8번호의 전체 공식 표기/대조시각·기존 상세로 이동. 중복업체합46을24곡 ID와 구분하고 실제0그룹/360JS-off/키보드·마지막가수 접근을 검증. 전체 업체 수록·반주기 실기기·권리/AdSense 승인으로 과장하지 않음 |
| 유지보수성 | 기존 ledger 끝append·validated projection·SSR/native anchor·고정 snapshot을 사용하고 새 client dependency/공용CSS·ID/storage를 변경하지 않음. 실제6리뷰40/40/39·교체 이력/원본 보존, 비차단 독립 URL oracle의 wwwKY/port/query 정합성 후속 명시. 재개/시각/M/실패/증거를 연결해 root 단독 통합 경계를 유지 |

**제품 변경·실제 운영/복구·버전 Release·명시적 Release Discord까지 수용한다.** 문서/최종 report의 원격 병합과 저장 완료 보고 기반 관리자 Discord의 실제 전송은 이 시점 아직 pending이다. 이전 섹션의 pending들은 각 관측 시점 이력이며 위 실제 결과가 있는 게이트만 순차 수용했다. 이후 두 기존 담당의 읽기 전용 보충 수용은 새 투표/순위로 만들지 않고 root가 별도 기록한다.

최종 남은 범위는 Actions CF 자동배포 credentials 설정/실제 자동배포 확인, 미검토 번호·유용한 자체 콘텐츠 후속, 실제 문의 처리·자료 권리/광고/CMP 검토와 운영자 Google 계정·소유권/신청/심사다. 기술/콘텐츠 준비와 실제 Google 승인을 분리하며 이 감사도 승인·허락·실기기 제공 결과를 만들어내지 않았다. 새 external 요청·Git/worker·테스트/서버/빌드·CI/배포·알림을 이 감사에서 수행하지 않았다.
