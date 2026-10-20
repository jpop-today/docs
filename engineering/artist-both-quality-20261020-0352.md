# 두 업체 확인곡 탐색 독립 회귀 · 2026-10-20

- 실행: `manual-20261020-0352`, [Issue44](https://github.com/jpop-today/jpoptoday/issues/44).
- 담당: `/root/run44_quality`, 초기 세 번째 작업자. source base `49d6806bb00e813bb3dcda15a22f2cf8d2d21da8`, docs base `4ddfa9d20b2d8aee5c538d25388c499a83c8675f`, source branch `codex/issue44-provider-both`.
- 기준: [제품 스펙](../product/spec.md)의 Issue44·SEARCH-001·CONTENT-001·DATA-001·DESIGN-001, 세 저장소 `AGENTS.md`, [실행 계약](../operations/runs/manual-20261020-0352.json).
- 쓰기 범위: `jpoptoday/e2e/artist-both.spec.ts`와 이 문서만. Git·공용 package/lock·CI·빌드·서버·배포·공용 검증기는 root 소유다. 기존 미커밋 ledger·guide·artists 변경은 다른 담당의 것으로 보존했다.

## 수용 기준과 독립성

검증 동선은 가수별 정적 확인 수 → 키보드로 TJ·금영 모두 확인 검색 → 두 조건의 AND 결과와 주소 복원 → 조건 해제·저장 원본 보존이다. JavaScript가 꺼진 경우 가수별 목록·확인 수·근거는 읽고, 검색의 비활성·조건 미적용 안내와 가이드의 정적 대안을 확인한다.

새 네 회귀는 앱의 count/group/matcher/projection/official URL helper를 import하지 않는다. `Song`은 type-only import다. 원본 staging과 append된 ledger를 직접 읽어 업체별 정정 후 번호의 전체 중복 횟수를 별도 계산하고, current/playlist·confirm·중복 없음인 ID 집합을 구한다. 제공된 `catalog.json`은 별도로 status `provider-confirmed`, `conflict:false`, 번호 문자열·비어 있지 않은 실제 evidence 필드, 날짜 파싱, HTTPS 공식 numeric URL과 정확 번호 대응을 검사한다. 이 두 집합이 업체마다 같아야 다음 브라우저 기대값으로 사용한다. 구조 검증을 사람의 제목·가수·버전 대조로 바꾸어 주장하지 않는다.

교집합과 합집합을 구분하는 실제 데이터 상태도 요구한다. 두 업체 교집합은 0보다 크고 합집합보다 작아야 하며, both=0인 실제 가수 그룹이 있어야 한다. 모든 전역·index·group의 data-count를 비교하고 정확 원문별 row ID·중복 없음·SHA256 원문 anchor·기존 곡 URL을 검사한다. 조회되지 않은 관측값·unknown·suppress·conflict·chart alias가 AND 검색에 섞이면 정확 ID 집합 비교가 실패한다.

## 실제 초기 확인 · 03:58 KST

2026-10-19T18:58:12Z(2026-10-20 03:58:12 KST)에 로컬 파일을 읽었다. 독립 Node 스크립트의 실제 결과는 다음과 같다.

| 원자료/집계 | 실제 값 |
| --- | --- |
| 원본 안정 ID | 3317 |
| ledger 전체 항목 | 92 |
| TJ / 금영 안전 current playlist ID | 36 / 31 |
| 두 업체 교집합 / 어느 한 업체 합집합 | 31 / 36 |
| 한 업체만 공식 확인된 ID | 5 |
| 정확 가수 원문 그룹 / both=0 그룹 | 28 / 5 |

원본 staging SHA256은 `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3`, 현재 ledger SHA256은 `a0d23d04cc705d444325f1ea8662cc00044fcba3f86074c66800569840a6eae9`다. 이 계산은 로컬 자료 검사이며 실조회·브라우저 실행·배포 검증을 대신하지 않는다.

`node node_modules/@playwright/test/cli.js test e2e/artist-both.spec.ts --list`는 실제 성공해 네 테스트가 발견됐다. 이 단계의 테스트 소스 SHA256은 `e5cce630dda0f7b6c9283043148cf570473267bc33fbe385d6a52c9e57e36b19`다. 목록 발견은 TypeScript 타입 검사나 브라우저 PASS가 아니다.

## 브라우저·통합 상태

초기 인계 시점에는 실제 브라우저 실행·스크린샷·전체 unit/type/artifact·clean CI·remote preview·운영·복구·Release 결과를 아직 이 담당이 실행하거나 읽지 않았다. 아래 후속 결과는 root가 한 서버·브라우저를 제공한 뒤 실제 로그를 읽고 추가한 것이다. `frontend-testing-debugging` skill을 읽었고 Browser capability의 실제 상태 및 Playwright 사용 사유는 root와 앱의 실행 기록과 연결한다. 자체 서버·설치·공유 빌드를 중복 실행하지 않는다.

스크린리더·Firefox/WebKit·실제 모바일 기기·음원/권리·Google 신청/심사 결과는 검사 범위 밖이다. 동료 평가는 두 담당의 최종 파일과 실제 검증 증거를 읽은 후 별도로 기록한다.

## 실제 첫 브라우저 결과와 재검증 대기

초기 목록 발견 이후 index/group의 모든 업체·both **가시 텍스트**도 data-count와 함께 정확히 일치하도록 보강했다. 이 최종 검사 소스 SHA256은 `efebc52a29fd3da8d026ba10766df86a46ddf2c275da704723b54a8251a4477e`다. root가 이미 시작한 초기 전체 회귀가 이 보강을 포함했다고 주장하지 않는다.

root의 persistent `http://127.0.0.1:4322` 서버를 사용한 최종 네 검사 로그 `/tmp/jpop44-working-final4.log`를 실제 읽었다. 결과는 **3 PASS / 1 FAIL (36.4초)**다. 모바일 키보드 AND·저장/URL, JS-off 정적 수·anchor·검색 비활성, 첫 가이드·정적 대안은 통과했다. 첫 전역/전체 가수 검사 실패의 실제 오류는 수치/본문 assertion이 아니라 `browserContext.close`의 trace 파일 ENOENT와 zip ENOENT였다. `test-results/.../error-context.md`도 같은 오류를 담았다.

초기 전체 회귀와 이 네 검사 실행이 기본 `test-results`를 공유하던 시점이므로 동시 runner의 산출물 정리 충돌 가능성을 root에 알렸다. 원인을 단정하지 않고, assertion을 바꾸지 않은 채 순차 실행·고유 `--output` 디렉터리로 같은 네 검사를 재실행하도록 인계했다. 이 실패를 PASS로 지우거나 앱 count 결함으로 바꾸지 않는다. 재실행 결과를 읽기 전 전체 네 검사 성공으로 기록하지 않는다. Node의 NO_COLOR/FORCE_COLOR 경고와 npm 업데이트 알림은 브라우저 콘솔·앱 오류와 별개다.

이어 root가 두 Playwright runner를 기본 output에서 병렬 실행한 것이 root의 실행 실수였음을 확인했다. 초기 전체 53개는 50 PASS/3 FAIL이며 coverage/guide trace 파일 오류와 기존 `title-artist-guide.spec.ts`의 첫 글 본문일자 `2026-10-14` 고정 assertion이 포함됐다고 인계받았다. 실제 20일 편집된 첫 글만 20일을 기대하고 다른 다섯 글의 날짜·근거 보존 검사는 유지하는 수정은 해당 기존 파일을 추가 소유한 root가 수행한다. 이 실행 실수와 기존 검사 갱신은 worker의 산출물 품질을 낮춘 근거로 삼지 않는다. 실패 로그·선택된 영수증은 남았지만 ENOENT로 없는 trace/zip이 보존됐다고 쓰지 않는다. root는 최종 frozen 검사 소스를 포함한 전체 53개를 순차·고유 `/tmp/jpop44-isolated-browser-results` 출력 경로로 재실행한다.

## 후속 실제 읽기 평가 · 04:06 KST

root의 `manual-20261020-0352-working-allowed.json`을 실제 읽어 unit **377 PASS**, Astro **26파일·오류/경고/hint 모두0**, preview artifact **3347파일/3336 HTML/색인0**, dirty base source `49d6806bb00e813bb3dcda15a22f2cf8d2d21da8`를 확인했다. 실패한 초기 restricted unit(-15)나 이후 trace 충돌을 이 결과로 지우지 않는다.

`/tmp/jpop44-isolated-browser.log`의 순차 실행 15–18번에서 최종 frozen SHA `efebc52a29fd3da8d026ba10766df86a46ddf2c275da704723b54a8251a4477e`의 신규 **4개 모두 PASS**를 실제 읽었다. 19:06:21Z 시점 전체53은 34번까지 진행 중이었다. 따라서 그 시점에서 전체53 PASS/clean CI/원격 성공을 주장하지 않는다.

| 검증 동선/화면 | 실제 읽은 결과 |
| --- | --- |
| 의도한 URL/title/h1·비어 있지 않은 본문·오류 overlay 없음 | 신규1번 PASS |
| 전역·모든28그룹/index의 수와 가시 텍스트·원문 anchor·정확 ID | 신규1/3번 PASS, 독립 TJ36/KY31/both31/union36 |
| 360px 키보드 44px/focus→AND 검색→31 ID→reload→0곡 복구→storage byte | 신규2번 PASS |
| JS off360 정적 수·마지막 anchor·기존 상세 URL·검색 비활성/미적용 안내 | 신규3번 PASS |
| 첫 가이드 AND/전체 의미/JavaScript/정적 artists 링크 | 신규4번 PASS |
| console warning/error·pageerror·광고 요청·모바일 가로 넘침 | 위4개의 해당 검사 PASS |
| screenshot | 앱 담당 영수증 `/tmp/run44-app-browser.json`을 읽고 영속 PNG4개를 실제 `view_image`로 열람. 이 담당이 새로 캡처했다고 주장하지 않음 |

Browser 제어용 js/runtime/CUA 도구가 없다는 앱 담당의 실제 조회와 root 제공 경로를 확인했다. 소스 AGENTS와 배정된 기존 Playwright 회귀를 따라 시스템 Chromium을 사용했고, 앱 영수증의 `http://127.0.0.1:4322`, 1440×1000 및 360×800 JS off, noerrors/noads·원래 gradient/palette·44px/3px focus와 실제 네 화면이 대응했다. 눈으로 읽은 화면은 desktop36/31/31, mobile 같은 수와 안내, Ado TJ1/KY0/both0의 상태 분리, 두 검색 활성 조건과31곡이다. dirty preview이며 다른 브라우저/실기기/운영/최종 SHA 검증은 별도다.

정확히 두 동료의 실제 파일/metadata/이미지/검증을 읽은 평가는 [품질 담당의 두 리뷰 JSON](../operations/runs/manual-20261020-0352-reviews-quality.json)에 남겼다. 자료20/20·앱20/20으로 필수 수정 결함을 발견하지 않은 동률이다. 각 기준의 이유·실제 파일 SHA·독립 대조 수·미완료 게이트를 구분하고 변경량/말수로 차등하지 않았다.

### 순차 전체 회귀 최종 결과

2026-10-19T19:07:39.573145Z에 기록된 [순차 브라우저 영수증](../operations/runs/manual-20261020-0352-isolated-browser.json)과 원 로그 `/tmp/jpop44-isolated-browser.log`의 마지막 결과를 실제 읽었다. root가 고유 `--output=/tmp/jpop44-isolated-browser-results`로 실행한 전체 **53 PASS (2.4분)**이며 신규4개와 root가 갱신한 첫 가이드 편집일 검사를 포함한다. E2E 파일 전체 SHA256 `efebc52a29fd3da8d026ba10766df86a46ddf2c275da704723b54a8251a4477e`는 재확인했고 바뀌지 않았다.

이는 dirty49d 작업본의 실제 회귀 통과다. 04:06의 두 리뷰 원문은 당시 진행 중 상태를 그대로 보존하며 이후 성공을 과거의 완료로 소급하지 않는다. 같은 결과 SHA의 clean CI·두target 동결 재현·원격 preview·운영/복구·Release는 여전히 root의 후속 증거가 필요하다. root가 여섯 리뷰의 40/40/40 동률과 사전 순서에 따라 evidence 담당을 논리적 active roster에서 제외하고 실제 replacement를 영입했다는 통보를 받았다. 이 동률을 실질 품질 차이로 설명하거나 물리 삭제됐다고 주장하지 않는다. 후속 공용검증기 감사 산출물은 읽기 요청과 실제 결과를 받은 뒤 별도 평가한다.

### 실제 교체 감사 평가 · 04:13 KST

교체 담당의 `tests/both-provider-artifact.test.mjs` SHA256 `6794eb13a45658e1b2df93f7db330a3597e529314e376bf87a32702a3384fc81`, [감사 문서](artist-both-artifact-audit-20261020-0352.md) SHA256 `0ab2ef5d4735fe96c2901a3867649bbcce16abedc36d6bb484233f7587c7e6ff`, root의 최종 verifier SHA256 `b31b13f31ebdb40784bbb1171eec62062b2c6575ba1b794fc1afa7eb1c426d74`를 실제 읽었다. 초기 허용 실행 **37 PASS/10 FAIL**은 실제 leaf 우회8개와 parent실패2개이며, 같은 test hash의 교체담당 독립 재실행과 root 별도 재실행이 각각 **47 PASS/0 FAIL**임을 두 원로그·해시에서 확인했다.

다른 가수 이름/원문 속성·숨은 수/검색 동작이라는 실제 결함8개를 찾고 root가 정적 비노출 하위 트리 및 가수명을 검증하도록 보완했다. 본인은 공유 verifier나 합성 테스트를 수정하지 않았다. 합성 문자열 ID/선행0·원문 가수 차이·union/intersection·single-provider/unknown/conflict/archived/chart-only를 명시 기대값으로 구분하는 감사다. 실제 제공사 조회나 CSS/일반 HTML parser와 완전 동등성·브라우저/운영 결과를 이 테스트로 주장하지 않는다.

[교체 결과 읽기 평가](../operations/runs/manual-20261020-0352-replacement-review-quality.json)는 19:13:03Z 실제 근거로 **20/20·필수수정/차단0**을 기록했다. 최초 두 동료 리뷰와 root의 전체6평가·동률/논리적 교체 기록은 변경하지 않았다. 이 후속 평가도 clean CI/원격 배포·복구·릴리스를 대신하지 않는다.
