# 노래방 번호 확인 가이드 · 2026-10-14

작업 ID `resume-20261014-2000-content` · [source Issue#6](https://github.com/jpop-today/jpoptoday/issues/6), [통합 Issue#15](https://github.com/jpop-today/jpoptoday/issues/15) · CONTENT-001/DATA-001/SONG-001/SEO-001 · [스펙 v0.4](../product/spec.md), [ADR-006](../adr/006-reviewed-corrections-and-production.md), [편집 기준](editorial-policy.md).

| 작업 계약 | 값 |
| --- | --- |
| 담당 / 통합 | `resume_2000_content` / root 한 명이 Git·공용 파일·CI·배포 담당 |
| source / docs base SHA | `3422391c3f4876cd68e324eeadbb5219fd243959` / `648e5b338b56f292cb652728cdc997860a8db840` |
| source branch | `codex/15-deployment-healthcheck-20261014` |
| 소유 파일 | source `src/content/guides.ts`, docs `content/number-checking-guide-20261014.md` 두 파일만 |
| 발행 경로 / slug | `/guides/checking-song-numbers/` / `checking-song-numbers` |
| 공개 작성·검토 표시 | `jpop.today 편집` · 본문 확인일 `2026-10-14` |
| 실제 본문 대조 완료 | `2026-10-14T20:05:18+09:00`에 로컬 사실·구조 assertion PASS 확인 |
| 상태 | 본문 작성·자체 대조 완료, root의 로컬 19E2E 로그와 360px 화면·오류 기록 확인. 동료 두 담당의 평가와 clean SHA CI·실제 배포 검증은 별도 통합 기록 |

## 사용자에게 추가한 내용

기존 네 가이드를 그대로 보존하고, 잘못된 번호를 피하려는 사람이 따라갈 수 있는 여섯 단계·12문단을 추가했다. 업체/기기 확인 → 제목·가수 대조 → 버전 대조 → 공식 근거·실제 확인일 읽기 → unknown과 미수록 구분 → 근거를 갖춘 제보 순서다.

번호 상태의 정의만 반복하지 않고 실제 정정 세 사례를 설명한다. Butter-Fly는 같은 제목의 서로 다른 가수를 구분하고, 전전전세는 같은 가수의 버전을 구분하며, 튜링 러브는 다른 곡의 번호를 제거한 뒤 미확인 상태로 남기는 이유를 보여준다. 사용자는 기존 메모의 번호를 다시 확인하고, 어느 정보가 다를 때 제보해야 하는지 알 수 있다.

공개 본문에는 Git·Issue·CI·worker 평가나 배포 보고를 넣지 않았다. 현재 `sections[].paragraphs[]` 구조와 기존 편집 표시를 사용한다. 새 가이드에만 optional `sources:[{label,url,checkedAt}]`를 추가해 금영 51714/40554·TJ 27944·금영 57803의 공식 검색을 직접 열 수 있게 했다. 네 URL과 실제 시각은 ledger에 있는 값을 그대로 사용하며 새 조회 결과가 아니다. 이 담당은 페이지 템플릿·카탈로그·ledger·raw·ID·저장키·CSS를 수정하지 않았고, 공통 템플릿의 조건부 근거 링크·날짜 렌더링은 root가 통합한다.

## 주장과 실제 근거

아래는 이미 보존된 [공식 대조 기록](../data/provider-corrections.md)과 [고정 ledger](https://github.com/jpop-today/jpoptoday/blob/3422391c3f4876cd68e324eeadbb5219fd243959/data/reviewed/provider-corrections.json)을 읽은 결과다. 이번 담당이 공식 사이트를 다시 조회한 결과가 아니다. 본문 확인일은 편집한 글을 검토한 날짜이며 제공사 번호의 새 확인일로 사용하지 않는다. 시각은 모두 **2026-10-14 한국 시간**이다.

| 공개 주장 | 공식 근거와 당시 실제 확인 시각 | 적용·경계 |
| --- | --- | --- |
| 전영호 Butter-Fly: 금영 51714, TJ 81866 | [KY 51714](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=51714) `14:18:00.106477`, 가수 전영호; [TJ 81866](https://www.tjmedia.com/song/accompaniment_search?nationType=KOR&strType=16&searchTxt=81866&strWord=Y) `14:17:58.279840`, 가수 전영호 | `jpop-playlist-butter-fly-71e0adeb5b`의 KY `40554→51714`, TJ confirm. 한국어판 선택의 가수·번호 대조 사례 |
| 和田光司 Butter-Fly: 금영 40554, TJ 25589 | [KY 40554](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=40554) `14:18:02.242052`, 가수 和田光司; [TJ 25589](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=25589&strWord=Y) `14:17:59.705309`, 가수 和田光司 | `jpop-playlist-和田光司-butter-fly-106e35d9b9`의 두 업체 confirm. 전영호 항목에 이 번호를 대신 사용하지 않음 |
| 前前前世의 TJ 27944·KY 44067은 movie ver. | [TJ 27944](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y) `14:17:53.032754`; [KY 44067](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44067) `14:17:56.091445` | 두 공식 결과 모두 RADWIMPS·movie ver. `radwimps-前前前世-movie-ver-映画-君の名は-ost`에는 confirm, 별도 `jpop-playlist-radwimps-前前前世-original-ver-1ff3eb38b9`에는 suppress/null. original 공식 미수록을 주장하지 않음 |
| KY 57803은 튜링 러브가 아닌 フォニイ | [KY 57803](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57803) `14:18:08.551976`, 제목 フォニイ·가수 ツミキ feat.可不 | 튜링 러브 ID `jpop-playlist-ナナヲアカリ-チュ-リンク-ラフ-feat-sou-0adf58fbdd`의 KY suppress/null. 대체 KY 번호는 이 기록에서 확보하지 못함 |
| 튜링 러브의 TJ 52536은 공식 대조 기록이 있음 | [TJ 52536](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52536&strWord=Y) `14:18:08.573254`, 제목 チューリングラブ·가수 ナナヲアカリ(Feat.Sou) | 같은 ID의 TJ confirm. TJ 번호를 KY 번호로 사용하지 않음 |
| 공식확인·관측·unknown을 구분하고, 정정 후 기존 곡 주소·저장 ID 보존 | ADR-006, source `contracts/provider-corrections.mjs`, `src/pages/songs/[id].astro`, 기존 호환성·정정 계약 테스트 | 코드 계약의 설명이며 이 담당이 앱 전체·현장 기기·현재 운영을 다시 검증했다는 뜻이 아님 |
| 제보에 곡 주소·업체·번호·제목/가수/버전 차이·근거/날짜 포함 | 기존 상세의 `이 곡 정보 제보` 경로와 `src/pages/contact.astro` | 실제 연락 수신·답변 완료를 주장하지 않음. 불필요한 개인정보·타인의 정보·가사 전문·이미지 전송을 요구하지 않음 |

표의 실제 조회 시각·제목·가수는 ledger의 `evidence.checkedAt/title/artist`와 대응한다. 각 응답의 당시 SHA-256은 기준 문서·ledger에 보존돼 있다. 링크가 나중에 바뀔 수 있으므로 공식 GET 검색 주소의 장기 API 계약이나 실시간 수록 보장을 주장하지 않는다.

## 미확인·권리·심사 경계

튜링 러브의 KY 대체 번호, 전전전세 original ver.의 별도 번호, 모든 현장 기기의 수록·업데이트 상태는 이 글에서 확인하지 않았다. 기존 3,317곡 전체가 공식 대조됐다고 설명하지 않는다. unknown은 번호가 미확보되거나 잘못된 연결·정체성 보류 상태이며 공식 미수록 판단과 다르다.

제공사 메타데이터를 읽었다는 사실은 전체 자료 재배포 허락·저작권 검토 완료·Google AdSense 심사 승인이 아니다. 글은 번호 선택을 설명하는 최소 곡명·가수·번호 비교와 자체 서술로 구성했고, 원문 가사·앨범/영상 이미지·녹음 정보·난이도·음역·권리 허락·심사 결과는 추가하지 않았다. [AdSense 기준](../operations/adsense.md)의 실심사·운영자 단계는 별도다.

## 실제 검증과 통합 수용 기준

| 실제 실행 | 결과 |
| --- | --- |
| 시작 `git status --short`, `git rev-parse HEAD` | 시작 미커밋 변경 없음, source HEAD가 배정 base `3422391…`와 일치. 다른 담당의 이후 변경은 보존 |
| Node24에서 `src/content/guides.ts` 직접 import | guide 5개·slug 5개 고유·새 가이드 6section/12paragraph·모든 문단 비어 있지 않음 PASS |
| 새 가이드 구간을 제외한 소스와 `git show <base>:src/content/guides.ts` 바이트 비교 | 기존 네 가이드·공통 editor/date mapper가 정확히 동일 PASS |
| ledger 사례 assertion | Butter-Fly의 네 번호·KY 이전값, movie confirm와 original 두 suppress/null, 튜링 러브 KY 반증·TJ confirm, 실제 checkedAt 범위 PASS |
| 새 가이드 공식 출처 assertion | source 4개의 HTTPS URL·checkedAt가 각각 실제 ledger entry와 정확히 일치, label 비어 있지 않음, 기존 네 가이드에는 sources 추가 없음 PASS |
| 첫 assertion 시도 | Node `execFileSync('git',…)`의 sandbox `spawnSync git EPERM`으로 비교 단계 실행 실패. Git 읽기를 shell로 분리하고 결과 파일을 Node에서 읽는 재실행은 PASS. 앞선 실패를 통과로 집계하지 않음 |
| 문서 링크 | 아래 파일 존재 검사로 상대 링크의 실제 대상 확인. 공식 사이트 재요청은 미실행 |

통합 수용 기준은 root가 새 가이드의 JS 없는 본문·title/canonical/편집 표시·모바일 읽기·연락 경로와 네 공식 근거 링크·실제 확인일을 확인하고, preview 전체 noindex/광고없음을 유지하며 production sitemap/meta에 이 검토 가이드 한 경로를 포함하는 것이다. 기존 고정 스냅샷·ID·URL·저장 호환과 정정값은 그대로여야 한다. 가이드 수 4→5에 따라 정적 HTML·sitemap 기대값이 변할 수 있으므로, 실제 build의 수치를 검증한 뒤 기록한다. 이 담당은 전체 type/build/artifact·브라우저·CI·Git·배포·복구를 실행하지 않았다.

6개 동료 평가 중 이 산출물에 대한 두 실제 읽기 평가·수정 반영·root의 검증·결과 SHA/PR/릴리스/운영 적용은 통합 기록에 연결한다. 본문 작성 완료를 운영 배포 완료로 승격하지 않는다.

## root 로컬 검증 인계 · 20:15 KST

root가 같은 변경 묶음의 preview와 시스템 Chromium 회귀를 실행했다. 이 담당은 `/tmp/jpop-v031-preview-e2e.log`의 실제 `19 passed (20.7s)` 결과와 새 가이드 JS 없는 렌더링·출처·홈 진입 테스트 두 건을 읽었으며, `/tmp/jpop-v031-qa/evidence.json`과 `guide-mobile.png`를 직접 열어 360×800 화면·가로 넘침 없음·오류 배열 비어 있음을 확인했다. 기록의 실제 시각은 `2026-10-14T11:15:17.369Z`이며 source base `3422391…`, `dirty:true`다. clean SHA CI·preview·운영 성공으로 대신 사용하지 않는다. 임시 이미지·로그의 영속 보존과 최종 SHA 연결은 root 통합 담당이다.

## 읽기 전용 동료 평가 · 콘텐츠 담당 → UI 담당

`2026-10-14T20:20:27+09:00` 실제 읽기 평가다. 자기 산출물에는 점수를 매기지 않았다. 평가 대상은 UI 담당의 source `src/pages/coverage.astro`, `e2e/coverage.spec.ts`, docs `engineering/coverage-ui-20261014.md`이며 이를 수정하지 않았다.

| 평가 항목 | 점수 0..5 | 실제 근거와 판단 |
| --- | ---: | --- |
| 정확성 | 5 | source의 playlist 분모·업체 상태별 합계와 공개 v2 projection을 독립 Node assertion으로 대조했다. TJ 12/3,123/164, KY 10/2,481/808, 분모 3,299, 목록 10ID·18문자열 번호, suppress13·confirmed conflict4가 모두 PASS. 차트 전용 제외·ID/번호 단위 차이·unknown≠미수록·Butter-Fly 51714·튜링 러브 KY null 설명이 코드와 일치한다. |
| 근거/검증 | 5 | 실제 E2E 로그의 coverage 세 테스트 PASS를 직접 읽었다. source/e2e가 모든 공개 근거 URL·원래 checkedAt·제목/가수·기존 상세 링크와 JS-off 키보드/브랜드/360px 넘침을 검증한다. `coverage-desktop.png` 1440×1100, `coverage-mobile.png` 360×800을 직접 열고 evidence JSON의 overflow false/errors[]를 확인했다. dirty base와 CI·배포 pending 경계를 기록해 성공 범위를 부풀리지 않는다. |
| 사용자 가치 | 5 | 관측과 공식 대조를 구분한 표에서 실제 근거가 있는 곡 목록·상세로 바로 이동할 수 있다. 모바일의 합계 카드·목록 이동 링크와 unknown 안내가 번호 선택에 필요한 설명을 제공하며, 공식 검색 새 탭과 확인 시각을 곁에 둬 사용자가 다시 대조할 수 있다. |
| 유지보수성 | 4 | 기존 `indexableSong`, `songHref`, v2 provider 근거와 공용 CSS를 재사용하고 페이지 CSS로 범위를 제한했다. 숫자는 build에서 계산한다. 다만 페이지의 per-provider `safeNumber`와 곡별 publication predicate, E2E의 동일 상태 조건·고정 합계는 향후 상태/ledger 정책 변경 시 함께 검토해야 하는 여러 지점이다. 고정 기대값의 갱신 기준은 UI 문서에 있어 현재 차단 결함은 아니다. |

합계 19/20. 현재 산출물에서 배포를 막는 콘텐츠·집계·호환 결함을 발견하지 않았다. 동료 비교·최저 담당 선정·교체는 이 점수와 다른 다섯 실제 평가 및 root의 최종 검증을 받아 관리자가 수행한다. 코드량·문단 수로 순위를 정하지 않았다.

## 하네스 읽기 평가 중 발견과 보류

20:24:47 KST에 하네스의 source `scripts/verify-deployment.mjs`, `tests/deployment-healthcheck.test.mjs`, docs `engineering/deployment-healthcheck-20261014.md`를 읽고 root의 `/tmp/jpop-v031-unit-final.log`에서 261/261 PASS·실패/skip0을 직접 확인했다. 다만 `/tmp/jpop-v031-healthcheck-baseline-normalized.json`의 실제 v0.3.0 HTTP 검증은 contact/privacy 이메일 변환을 통과한 후 `/songs/jpop-playlist-butter-fly-71e0adeb5b/`에서 BODY_MISMATCH로 6시도/49.006초 뒤 실패했다. 따라서 합성 회귀 성공과 실제 전체 HTTP 수용을 분리하고 최종 하네스 점수를 보류했다.

이 담당은 상세 제보 anchor가 plain 주소 본문과 달리 subject/body query와 `이 곡 정보 제보` label을 갖는 것을 읽고 root에 전달했다. root가 실제 GET 응답에서 그 전체 href의 query·literal `&amp;`를 포함한 CF XOR payload와 기존 label 보존·decoder 하나를 확인해 파일 담당에게 좁은 추가 규칙을 요청했다. 주소만 비교하거나 query를 제거하는 일반 완화는 제안하지 않았다. 이 담당은 실제 HTTP를 새로 요청하거나 하네스 파일을 수정하지 않았다. 최초·후속 실패 receipt는 지우지 않고 보존하며 최종 보완·실제 HTTP 후 읽기 평가를 계속한다.

## 읽기 전용 최종 동료 평가 · 콘텐츠 담당 → 하네스 담당

`2026-10-14T20:31:03+09:00` 재평가다. 최종 source `scripts/verify-deployment.mjs`, `tests/deployment-healthcheck.test.mjs`, docs `engineering/deployment-healthcheck-20261014.md`를 다시 읽었다. plain 주소 span과 query mailto의 전체 payload·label을 함께 expected occurrence budget으로 확인하는 보완을 검토했다. 이 담당의 테스트 중복 실행·외부 HTTP 요청·하네스 파일 변경은 없다.

최종 root 로그 `/tmp/jpop-v031-unit-final-query.log`는 **274/274 PASS·실패/skip0**이다. worker의 최종 하네스 검증은 70개이며 앞선 57개/전체261개 결과와 구분한다. root 실제 production baseline receipt `/tmp/jpop-v031-healthcheck-baseline-complete.json`은 기존 v0.3.0 source `b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8`, `2026-10-14T11:29:56.681Z`, 1시도·35.682초·25 checks 모두 PASS·code null이다. 이는 새 v0.3.1 배포 성공이나 전체 CDN 동시 전환의 증거가 아니다.

이 담당도 root가 보존한 실제 Butter-Fly 상세 변환 HTML과 고정 v0.3.0 dist를 **읽기 전용 로컬** `normalizeCFEmailHTML`로 독립 대조했다. raw SHA `5128030b3b20b62245b941568ed1643236afe44169e6237980601994019eead2`, normalized/expected SHA `55b0786f7d987dd2196e6df58ada5a6104cfc2683dbc32bc761453d03d97140c`가 일치하고 anchor1/mailtoAnchor1/decoder1 PASS였다. 원문 href·동적 payload·HTML을 문서에 복사하지 않았다.

| 평가 항목 | 점수 0..5 | 실제 근거와 판단 |
| --- | ---: | --- |
| 정확성 | 5 | 엄격한 origin/target·clean metadata와 세 digest·catalog count/bytes, 시작·끝 nonce metadata, query 없는 실제 페이지·404·assets를 함께 대조한다. 이메일 보완은 전체 HTML payload의 subject/body/ID/`&amp;`/label·개수를 정확히 비교하고, 같은 decoder 하나만 되돌린 뒤 전체 바이트를 검사한다. query를 버리는 완화가 아니며 두 실제 실패를 숨기지 않고 보완해 현재 실제 baseline 전체 HTTP를 통과했다. |
| 근거/검증 | 5 | 70개 테스트의 stale/mixed, URL/Unicode/실제3317ID, abort 무시·endless body/deadline/oversize, receipt 경합·보존, 원문 secret/HTML/script 주입 거부를 직접 읽고 root274PASS 로그를 확인했다. 두 실패 receipt와 최종 실제25 checks PASS, raw/normalized/expected hash 기록을 분리했으며 실제 상세 정규화를 독립 로컬 대조했다. worker의 합성 테스트를 root 실제 HTTP나 새 운영 배포로 대신 표시하지 않는다. |
| 사용자 가치 | 5 | API pointer·고정 metadata 하나로 운영 전환을 잘못 판정하는 문제를 막는다. 실제 검색/즐겨찾기/상세·문의/개인정보·번호 카탈로그와 crawler 정책의 같은 버전 여부를 검증해 잘못된 운영 반영·복구 보고를 줄이고, 실패 증거와 마지막 정상 결과를 보존한다. |
| 유지보수성 | 4 | 의존성 없는 함수·주입 가능한 fetch/time·안전한 고정 code·비교적 작은 CLI로 테스트와 실제 transport를 분리했다. 다만 정적 원격 표본 경로 목록·home asset 추출 규칙·CF의 정확한 변환 markup/decoder 위치는 서비스나 제공사 변경 시 별도 갱신해야 한다. 문서가 전체 artifact 검증·browser와의 경계를 설명하며 예상치 못한 변화는 실패하므로 현재 차단 결함은 아니다. |

합계 19/20. 하네스의 로컬·실제 기존 baseline 수용은 충족됐고 현재 읽기 평가에서 병합을 막는 결함은 발견하지 않았다. 새 clean SHA CI→preview→운영 배포와 실제 rollback·복원은 root의 다음 게이트이며 아직 이 리뷰의 완료 사실이 아니다. 최초 contact 41.069초 실패와 두 번째 상세 49.006초 실패의 보존 기록을 최종 성공으로 덮어쓰지 않는다. 관리자만 다른 다섯 실제 평가와 최종 검증을 함께 확인하여 순위·교체를 결정한다.
