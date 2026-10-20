# 차트 두 업체 확인곡 독립 품질 검증

- 실행: `manual-20261020-1325`, [Issue47](https://github.com/jpop-today/jpoptoday/issues/47), 상위 Issue6.
- 담당: `/root/run47_quality`. source base `4d4c021e668d49c39057e76558c21b30735b8d98`, docs base `1291c45fded5f9c22ec28b5bf78fa62d37b1d751`.
- 기준: [제품 스펙의 Issue47](../product/spec.md#차트의-두-업체-확인곡-탐색--issue47-2026-10-20), DATA-001 / CONTENT-001 / SEARCH-001 / DESIGN-001.
- 독점 수정: source `tests/chart-both-evidence.test.mjs`, `e2e/chart-both.spec.ts`, 이 문서. Git·package/lockfile·공용 verifier·CI·공유 서버·배포는 root가 수행한다. 이 담당은 새 worker나 서버를 만들지 않았다.

## 수용 기준과 독립성

검증 흐름은 `/chart/` → 기본 닫힌 네이티브 목록 열기 → TJ 원래 순위 행의 앵커 → 기존 곡 상세 URL이다. 가이드의 `/chart/#chart-both-heading` 링크로도 JavaScript 없이 같은 목록을 연다.

단위 검증은 원본 staging과 reviewed ledger에서 각 업체의 보정 후 번호를 전체 카탈로그 대상으로 센다. 같은 번호의 중복은 미검토·목록 외 곡도 포함한다. current playlist, confirm 항목, 번호별 HTTPS 공식 조회 URL·전체 제목/가수·실제 확인 시각, 중복 없는 번호라는 조건으로 독립 ID 집합을 만든다. `buildPublicCatalog` / `applyProviderCorrections`는 검증 대상이며 결과의 별도 공개 predicate와 이 집합을 비교한다. 앱 count/helper와 root의 artifact verifier는 oracle로 호출하지 않는다.

브라우저 검증도 같은 원본·원장으로 만든 집합과 실제 HTTP `catalog.json`의 별도 predicate를 교차 비교한 후 차트 교집합 수·원 TJ 순위순 ID·문자열 번호·기존100행/업체별 목록/합집합을 대조한다. 기존 스냅샷 날짜·고지·출처와 canonical/target robots·광고 요청 부재, 360px JavaScript off·키보드·44px summary·초점 표시·fragment/reload·localStorage 원문 보존을 요구한다. 번호의 visible text에는 실제 앱의 `· 공식 확인` 문구까지 검사한다.

실제 부정 경계는 `プロポーズ`의 KY null/unknown/evidence null/correction null, KY 명의 확인이 보류된 `ルカルカ★ナイトフィーバー`와 `逆光`, 한 업체 확인곡 때문에 교집합이 합집합보다 작다는 차이다. 이 테스트는 공식 미수록이나 모든 기기의 곡 지원·편곡·권리·Google 심사를 추정하지 않는다. 0곡 fixture나 브라우저별 모든 상태를 확인한 것으로 확대하지 않는다.

## 보존한 기준

CI는 shallow checkout에서도 source만으로 실행된다. 테스트 실행 중 `git show`나 docs checkout·실행 JSON·네트워크는 필요하지 않다. base Git blob에서 실제 계산한 literal hash를 사용한다.

| 기준 | SHA-256 |
| --- | --- |
| staging bytes | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| chart bytes | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| identity baseline bytes | `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8` |
| global CSS bytes | `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889` |
| ledger 원래92개 전체 객체·순서, JSON 정규화 | `73f58019ff2ad06e8a5bcb5db8f5ee254a63da038cc9afe49ce43dec410cc5ad` |

ledger 상위 metadata 전체도 literal 객체와 비교한다. 최소99항목과 index92~98의 정확한7 song/provider 쌍을 검사하면서 후속 append를 허용한다. 나머지5가이드 전체 객체별 base hash, 여섯 slug의 기존 순서, chart guide만 실제 편집일 `2026-10-20`인 조건을 검사한다. 원래92개를 검사하는 이 hash는 상위 metadata까지 합친 다른 verifier의 hash와 입력이 다르다.

## 실제 실행 결과

| 명령/검사 | 관측 결과 | 한계 |
| --- | --- | --- |
| `node --test tests/chart-both-evidence.test.mjs` | exit0, 파일 wrapper 1/1 | leaf 실행4개 통과로 계산하지 않음 |
| `node --test --test-isolation=none tests/chart-both-evidence.test.mjs` | exit0, 실제 leaf4/4, fail0/skip0 | 전체 unit suite가 아님 |
| `JPOP_TEST_BASE_URL=http://127.0.0.1:4323 node_modules/.bin/playwright test e2e/chart-both.spec.ts --list` | exit0, 테스트4개 수집 | 브라우저 실행이 아님 |
| `JPOP_TEST_BASE_URL=http://127.0.0.1:4323 PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium node_modules/.bin/playwright test e2e/chart-both.spec.ts --output=/tmp/jpop47-quality-browser-results` | exit0, 실제4/4, 23.4초, 2026-10-20T05:21:05Z 확인 | root 단일 로컬 preview, 외부 배포 아님 |
| `node /tmp/jpop47-quality-receipt-audit.mjs` | 2026-10-20T05:19:36.010Z, 영수증/후보/별도 편집결정/ledger7건 교차 감사 pass | 새 공식 조회를 실행한 것이 아님 |
| `node /tmp/jpop47-quality-visual-audit.mjs` | 2026-10-20T05:21:50.229Z, 1366×1000 / 360×800 JSoff 실제 감사 pass | 두 viewport와 Chromium만 관측 |

base hash 확보의 첫 Node child-process 호출은 기본 sandbox에서 `EPERM`으로 실패했다. 같은 읽기를 일반 추가 network 권한으로 수행했을 때 성공했다. 이 관측 이상의 원인은 단정하지 않았으며 실패를 검증 성공으로 기록하지 않는다.

실제 요청은 자료 담당의 [조회 영수증](../data/provider-review-20261020-1325-requests.json), [검토 기록](../data/provider-review-20261020-1325.md), [보존 검사](../data/provider-review-20261020-1325-validation.json)를 읽었다. 별도 감사에서 각 후보는 `unreviewed`를 유지하고 HTTP200·final URL·response hash·string 번호·full 제목/가수가 영수증과 일치하며, 별도 confirm 결정과 ledger의 실제 checkedAt/hash까지7건이 일치했다. KY null은 조회하지 않았다는 영수증과 대조했다. raw 공식 HTML을 재조회하거나 본 역할이 최초 공식 응답을 수집했다고 주장하지 않는다.

영수증 bytes hash는 `b562d7284deed95153151f710a22e3726b96c6b9aa6ab80d98f4b3480318cc4d`, 감사 시 ledger bytes hash는 `d4339885092fd050c4f92b5993b7d9a1538e05ac4f0aa06128203d758f348bf3`다. 감사 임시 실행/JSON은 `/tmp/jpop47-quality-receipt-audit.mjs`, `/tmp/jpop47-quality-receipt-audit.json`이며 CI 의존성이 아니다.

## 브라우저 환경과 잔여 단계

frontend-testing-debugging과 연결 Browser 스킬을 읽고 사용 가능한 도구에서 Browser의 `js`/runtime·tab 제어와 CUA 경로를 조회했으나 노출된 도구가 없었다. 따라서 `Browser plugin not available` 상태이며 작업에서 명시적으로 허용한 기존 Playwright/시스템 Chromium으로 검증한다. 새 브라우저 의존성을 설치하지 않는다.

root의 실제 listening 통지 후 단일 `http://127.0.0.1:4323` 서버에서 수행했다. 브라우저 결과는 공유 기본 디렉터리를 피한 `/tmp/jpop47-quality-browser-results`이며 `.last-run.json`은 `status: passed`, `failedTests: []`다. 모든 테스트가 선언된 raw/ledger/public oracle, 원100행·업체별/합집합 보존, full 번호 문자열, 기존 상세, 360px JSoff native details·keyboard·44px/focus, hash reload, storage bytes, guide link, preview robots/noads 조건을 통과했다. URL.hash의 일본어 ID percent-encoding은 `decodeURIComponent`로 비교하고 원문 href/ID exact 검증은 유지한다. 앱 담당의 선행 관측으로 실제 테스트 실행 전에 oracle를 정정한 것이며 UI 회귀나 실패 실행으로 기록하지 않는다.

별도 시각 감사는 page title `TJ JPOP 차트 기록 | jpop.today`·경로 `/chart/`·main 비어 있지 않음·framework overlay 없음·warning/error/pageerror 없음·키보드 Enter 후 native details open·실제27개 링크·가로 넘침 없음까지 확인했다. 최초 화면과 열린 상태 PNG를 읽어 큰 화면의 네 번째 count와 닫힌 목록, 모바일의 2열 count·줄바꿈·눈에 보이는 blue focus와 펼친 번호목록을 확인했다. blue→pink 브랜드선과 pink/mint/blue 배경은 기존 CSS bytes hash도 동일하다. 이미지에는 잘림·중첩·framework 오류를 발견하지 않았다. 단일 로컬 preview 관측이며 외부 실제 preview/운영을 관측한 것으로 확대하지 않는다.

실제 시각 증거는 `/tmp/jpop47-quality-chart-1366-initial.png`, `/tmp/jpop47-quality-chart-1366-opened.png`, `/tmp/jpop47-quality-chart-360-initial.png`, `/tmp/jpop47-quality-chart-360-opened.png`, 관측 JSON은 `/tmp/jpop47-quality-visual-audit.json`이다. 임시 스크린샷·실행 파일은 source artifact에 포함하지 않았다. 이 역할이 전체 suite/type/build/artifact나 같은 SHA CI·외부 preview·실제 복구·운영 배포를 수행한 것은 아니다. root의 전체 검증·배포 결과는 root 기록을 따른다. 다른 브라우저·모든 viewport·실제 기기·광고/Google 계정 심사는 미검증이다.

최종 검증 source hash: `e2e/chart-both.spec.ts` = `c4cde5a1af4a73ed23a22b2a7acfd9fa7f38eb3f18f52a7dea6357aef0a3f294`, `tests/chart-both-evidence.test.mjs` = `7ce6bdc86b89dbbb242f3a3ee31c1626af49c2cc4064ecc76b0da2d691a4502f`.

동료 두 담당의 실제 산출물 읽기 평가는 [별도 peer JSON](chart-both-peer-quality-20261020-1325.json)에 기록한다. 본인 점수나 배포 결과를 만들지 않는다.
