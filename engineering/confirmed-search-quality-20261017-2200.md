# 업체별 공식 확인곡 검색 · 품질 검증

작업 `resume-20261017-2200` · [Issue35](https://github.com/jpop-today/jpoptoday/issues/35). SEARCH-001, DATA-001, SONG-001, FAVORITES-001, CONTENT-001, DESIGN-001, HARNESS-001. [제품 계약](../product/spec.md#업체별-공식-확인곡-검색--issue35-2026-10-17).

담당 `/root/resume22_quality`. branch `codex/35-confirmed-search-20261017`, source base `2964f7ea3f8c43f147422107f40629f51c010698`, docs base `d679cf57592b285a30d762784fdb62ae0bf73581`, 조직 base `b7aad18fe2f793453b6505bb150f359d05ae3554`. 세 AGENTS·제품 계약·일일 운영·실행 기록·리뷰 대상 기록과 시작 Git status를 읽었다. 이 담당의 편집 범위는 `tests/confirmed-search.test.mjs`, `e2e/confirmed-search.spec.ts`, 이 문서의 정확히 세 파일이다. Git mutation·공용 계약/타입·package/lockfile·CI/배포·빌드/dist/서버는 root 소유이며 수정하거나 실행하지 않았다.

## 검증 계약

- 합성 자료의 TJ만·금영만·양업체 확인을 구분하고 둘 선택/중복 URL 조건은 AND로 검증한다. 문자열 `00123`/`00517`의 선행 0을 보존한다.
- `observed-unverified`, unknown, registered, unlisted, 억제 correction, conflict true/누락/유사 값, archived/chart-only가 확인 조건을 통과하지 못하게 한다. 근거 누락·비문자열·빈 필드, 잘못된 번호 형식, 다른 번호/업체 URL·비 HTTPS·유사 호스트·credential·fragment·추가/중복 query를 양업체 모두 검사한다.
- 기존 q·has/only·tag·anime/artist와 조합하고, 관측 번호가 있는 `has-ky`와 공식 확인된 `confirmed-ky`를 구분한다. 기존 미인식 number URL 값의 no-op 의미도 보존한다. 날짜/제목·가수의 실제 진위 검증은 ledger/편집 검토의 책임이며 projection의 비어 있지 않은 필드 검사와 혼동하지 않는다.
- 실제 expected ID는 **ledger confirm 항목 + 원 staging current/playlist + 정정 후 전체 자료의 업체별 번호 중복 수**로 독립 계산한다. oracle에서 앱 matcher나 `confirmedNumbers`를 호출하지 않는다. 각 업체의 exact ID Set, 양업체 교집합, 숫자 문자열과 ledger의 공개 근거 필드를 대조하며 미래 검토 자료가 늘어도 고정 건수 때문에 실패하지 않는다.
- 기존 samfree/Ado의 KY 보류는 관측값이 있어도 확인 필터에서 제외한다. `前前前世 (original ver.)`의 양업체 억제 번호는 unknown/null로 남고 저장 ID/기존 상세가 사라지지 않아야 한다. 이번 Issue35의 조회 대상 7개 외의 보류를 승인하는 작업이 아니므로 이 반례를 유지한다.
- frozen 전체 public song의 조회 전후 바이트·안정 ID·번호를 비교하고 legacy `/filter.html` query 바이트/새 number 값과 `/song.html?id=`의 정확한 기존 상세 URL을 검사한다. 즐겨찾기 읽기에서 `{id,note}`와 미해결 문자열 ID의 원 저장 바이트를 변경하지 않아야 한다.
- 브라우저는 exact ledger ID/수, TJ→양업체→금영 선택, old q/tag/artist/has와 공유 URL·새로고침·page reset·뒤로가기·전체(number만 해제)·조건 해제(기존 전체 초기화)를 검증한다. 빈 결과와 즐겨찾기 빈 결과는 공식 미수록을 뜻하지 않는다고 안내해야 한다.
- 실제 SAM/Ado 결과행에서 선택하지 않은 KY에 공식 badge를 붙이지 않고 관측 aria-label을 유지한다. favorites 필터로 잠시 숨긴 항목/미해결 ID는 저장값에서 삭제하지 않는다. 느린 catalog 동안 새 버튼은 비활성이며 JS-off에서 SSR 목록에 조건이 적용되지 않았다고 명시하고 기존 근거/가이드 링크를 제공한다.
- 1440×1100·360×800·800×360에서 가로 넘침, 44px target·키보드 focus/Enter/Space, 원 blue→pink 및 pink/mint/blue 배경을 검증한다. title/본문/overlay·관련 console/pageerror와 실제 사용자 상호작용을 확인한다. 전체 기존 E2E의 404·preview noindex/noads·저장/복사 검사를 대체하지 않는다.

## Browser 경로와 실행 환경

`frontend-testing-debugging`과 `browser:control-browser` skill을 모두 읽었다. 제공된 전체 도구 metadata에서 Browser `js`, 탭 조회/제어 및 CUA runtime을 검색했지만 **호출 가능한 제어 도구는 0개**였다. skill이 목록에 있다는 이유만으로 확장이 연결되었다고 쓰지 않는다. root의 명시적 기존 Playwright/system Chromium 허용 범위에서 실행하며 새 브라우저/의존성을 설치하지 않고 TLS를 우회하지 않는다. Browser 연결 검증은 이 도구 부재로 blocked이며 앱 검증 전체가 blocked인 것은 아니다.

검증 흐름: `/search/` → 업체별 공식 확인 조건 선택 → 실제 근거가 있는 ID/교집합 → 기존 상세·저장 및 조건 복구. root가 제공하는 dirty artifact/공유 서버에서 우선 검사하고 clean CI/실제 preview/운영·복구 결과와 구분한다. 이 담당이 임의 빌드/공유 서버를 만들지 않는다.

## 실제 실행

| 확인 시각 / 검사 | 결과 | 범위와 증거 |
| --- | --- | --- |
| 2026-10-17 22:10 KST / 새 단위 검사 | **7/7 PASS**, fail/skip/cancel0, **214.336067ms** | `node --test --test-isolation=none tests/confirmed-search.test.mjs`, Node24.19.0, 현재 공유 dirty source. root matcher 구현 이후 실행. 이전 직접 실행도7/7 PASS이며 일반 `node --test`는 이 실행 환경에서 파일1개 통과만 출력하므로 내부7그룹 수의 근거로 사용하지 않았다. |
| E2E 수집 | **7개 정상 수집 / exit0** | `npx playwright test e2e/confirmed-search.spec.ts --list`. 이것은 브라우저 실행 PASS가 아니다. JSON은 Node24 ESM 직접 import 대신 `readFileSync`로 읽는다. |
| 22:18 KST / 초기 targeted 브라우저 | **7/7 PASS · 12.8초** | root dirty preview `http://127.0.0.1:4321`, system Chromium `/usr/bin/chromium`, `JPOP_TEST_BASE_URL`/`JPOP_SCREENSHOT_DIR`를 지정한 `npx playwright test e2e/confirmed-search.spec.ts`. 원 로그 `/tmp/jpop-resume22-quality/targeted-first.txt`. 실제 원격 preview/운영이 아니다. |
| 22:18 KST / boolean predicate 보완 후 단위 재확인 | **7/7 PASS · 249.154352ms** | 같은 명시적 isolation-none 명령. 판정 조건·공개 API 보존을 확인했다. |
| 22:18–22:20 KST / 실제 시각·새 rowbbox 회귀 | **시각 결함 발견 / 새8번째 검사 실제 FAIL** | 초기 desktop/mobile/JS-off PNG3개를 직접 열어 아래 충돌을 확인하고 범위 내 E2E에 회귀를 추가했다. `/tmp/jpop-resume22-quality/mobile-before-fix.txt`와 trace는 실패 원본이다. 당시 수정 후 재검증 pending이었으며 아래22:22 결과로 후속 완료했다. |
| 22:22 KST / layout 수정 후 targeted 전체 | **8/8 PASS · 16.7초** | root 재빌드한 동일 dirty preview 서버, 같은 명령/브라우저, 별도 결과 `/tmp/jpop-resume22-quality/targeted-fixed.txt`, PNG는 `/tmp/jpop-resume22-quality/fixed/`. 원7검사와 새rowbbox검사를 전부 수행했다. |
| 전체 unit/type·두 target 재현·clean CI·실제 preview/운영·복구·Release | **pending** | root 통합 게이트이며 단위/수집 성공으로 대신하지 않는다. |

위 관측은 공식 제공사 재조회·Google 심사·rights 확인을 수행한 증거가 아니다. 브라우저 의미 검증 외 WebKit/Firefox·실제 기기·보조기술·필드 성능은 미실행이다. 실제 동료 검토 요청 전 점수나 순위를 만들지 않는다.

root는 초기 dirty preview artifact gate가 browser bundle의 `evidence: {...}` 런타임 복사 객체 때문에 실패한 것을 전달했다. 이것은 로컬 gate 실패이며 공개 데이터 literal이나 실제 배포의 유출을 확인한 것이 아니다. 검증기를 완화하지 않고 browser matcher에서 boolean provider predicate만 사용하며 기존 `confirmedNumbers` 복사 반환/API는 server 측에 유지하는 후속 정리를 root가 수행한다. 판정 조건은 그대로이며 품질 소스 두 파일은 freeze하고 수정 후 같은 단위/실제 브라우저 검사로 수용한다. root의 전체325/type0 보고와 초기 실패들은 원 로그를 읽기 전 이 담당의 직접 실행 결과로 계상하지 않는다.

## 실제 화면 발견과 후속 회귀

초기7개가 통과해도 시각 검증을 끝났다고 쓰지 않았다. `/tmp/jpop-resume22-quality/confirmed-search-desktop.png`, `confirmed-search-mobile-both.png`, `confirmed-search-js-off-mobile.png`를 직접 열었다. desktop은 TJ32곡·정확한 Ado의 한 업체 badge·양업체 필터 설명을 읽을 수 있었고, JS-off는 비활성 조건과 현재 SSR 목록에 조건 미적용 안내·근거/가이드 링크가 표시됐다. 그러나 **360px JS-on 검색 결과행**은 TJ 공식 badge가 금영 라벨과 겹치고 금영 badge가 수직으로 줄바꿈했다. 페이지 전체 overflow PASS로 행 안의 충돌을 가릴 수 없으므로 root/app에 전달하고 운영 수용을 보류했다.

새8번째 회귀는 동적 home/search/favorites의 표시된 모든 결과행과 JS-off search 초기 SSR5행에서 양업체 label·number/unknown·badge 실제 bounding box를 비교한다. 행 밖 이탈·서로의 면적 겹침·badge 여러 줄·번호복사 target44px 미달을 거부한다. current dirty artifact에서 `--grep 'mobile search and saved rows'` 실제 첫 실행은 home의32확인곡 행에서 실패했다. 첫 실패 이후 search/favorites/JS-off 분기는 실행되지 않았으므로 거기까지 재현됐다고 확대하지 않는다. 이 실패 원본과 초기7 PASS/문제 PNG를 보존한다.

app 담당은 원래 chart의 한정된 배치와 유사한 data-list-view 전용 모바일 규칙을 Search 소유 파일에 추가하고 root 재빌드로 넘겼다. `global.css`/브랜드 바이트는 그대로이며 품질 담당이 앱 스타일을 대신 수정하지 않았다. 새로운 artifact가 준비된 뒤8개 전체를 재실행하고 실제 수정 후 화면도 다시 읽어야 최종 수용한다.

### 수정 후 실제 수용 · 22:22 KST

root 재빌드/verify-dist 성공 및 최신 서버 상태 통지를 받은 뒤 새8개 전체를 실행해 **8 PASS/16.7초**를 확인했다. home/search의 TJ확인32행, favorites의 저장3행과 JS-off search의 초기 SSR5행을 실제 비교했다. 양업체 label·number/unknown·badge의 겹침/행 밖 이탈은 없고, badge는 한 줄이며 번호 복사 버튼은44px 이상이다. shared dist/서버·브랜치·CSS를 직접 바꾸지 않았다.

수정 후 desktop·mobile-both·mobile-provider-rows·JS-off mobile PNG **4개를 각각 직접 열었다**. 번호와 업체 badge가 서로 분리되고 기존 blue/pink 및 배경 palette를 유지한다. mobile-provider-rows는 첫3행 Lemon·怪獣の花唄·Pretender의 업체별 번호·한 줄 badge를 보여준다. 기존 첫 PNG와 FAIL 로그를 보존하고 새 `fixed/` 경로로 저장했다. 초기 artifact gate 실패·최초7 PASS가 놓친 시각 결함과 수정 후8 PASS를 서로 다른 사건으로 기록한다.

| 렌더 QA | 결과 / 실제 증거 |
| --- | --- |
| Page identity / nonblank | PASS: `/search/`, jpop.today title·고유 h1·main 본문. |
| Framework overlay | PASS: 관련 overlay selector0. |
| Console / pageerror | PASS: 첫 provider 탐색 loop에서 관련 오류0; 모든 브라우저·전 네트워크 경로를 관측했다고 확대하지 않는다. |
| 사용자 상호작용 | PASS: 업체별/AND exact ID와 수32·27·27, q/old필터/page/history/reset·legacy·storage 원바이트·0미수록안내/회복. |
| 접근성과 responsive | PASS:1440×1100·360×800·800×360의 overflow/새조건44px·키보드 focus/Enter/Space, slowcatalogdisabled·JSoff안내와 대체링크. |
| 결과행 실제 시각/geometry | PASS: 위 home/search/favorites/SSR행 전수 bbox와 수정후 PNG4개 직접 읽기. |
| 실제 원격 배포·계정 심사 | 이 담당 미실행: root 게이트/Google 실제 결과와 구분. |

소스 두 파일은22:22KST 최종 freeze했다. root의 전체 기존 E2E/type/unit·동결 두 target·clean SHA CI·actual preview/복구/운영/Release는 여전히 별도 수용 게이트다. 이 문서의 로컬 검증 완료를 프로젝트 전체 완료나 AdSense 승인으로 쓰지 않는다. 원본 로그와 PNG의 영속 복사/링크는 root가 공유 운영 기록의 파일 범위에서 수행한다. 실제 동료 평가 요청 뒤에만 두 평가를 아래에 추가한다.

### 테스트 유지보수 보완 인계 · 22:28 KST

앱 담당의 실제 읽기 검토에서 `allVisibleIds`의 고정5회 더 보기 상한이 현재32곡에는 충분하지만 이후 확인곡 증가에서 정상 제품을 false FAIL시킬 수 있다는 비차단 지적을 받았다. root가 기존 전체 E2E 종료 후 이 담당의 E2E 파일만 다시 여는 것을 지시해, 상한을 동결 `staging.songs.length`로 바꾸고 **클릭할 때마다 실제 표시행 수가 엄격히 증가하는지** 기다려 검사하도록 보완했다. 제품의30개 pagination step이나 미래 확인곡 수를 hardcode하지 않고 반복 무종료/진행 없음은 계속 거부한다. 단위 소스·제품·빌드·서버는 바꾸지 않았다. 파일 범위 diff 검사는 통과했으며 실제 브라우저 재실행은 root의 전체 검증에 맡겨 동시 실행하지 않았다. 위8/8 PASS는 이 helper 보완 전 실제 결과이고, 보완 후 결과는 root 실행을 확인해야 한다.

같은 인계에서 root는 기존39개 전체 E2E가36 PASS/3 FAIL이며, `deferred-guide.spec.ts`의 ledger77 고정 assertion이 실제84항목을 거부한 결과라고 전달했다. 이 담당이 해당 전체 로그를 직접 읽거나 실행한 것은 아니므로 현재는 **root 통보 결과**로 구분한다. root가 그 기존 파일의 소유 범위를 확장해 첫77의 고정prefix 근거를 검증하면서 후속 append를 허용하고, 기존 SAM/Ado 보류·현재 전체ledger의 build hash 검사는 유지하는 보완을 수행한다. 품질 담당은 해당 공용 기존 E2E를 수정하지 않았으며 최초 실패를 현재 통과로 덮지 않는다.

## 재시작 인계 검토와 assertion 보완 · 2026-10-18 03:09 KST

이 절의 담당은 새 인계팀 `/root/resume35_quality`이며 앞 절의 `/root/resume22_quality`와 다른 실제 작업자다. 위 원본 실행·시각·실패·8/8 결과는 이전 담당의 영속 기록으로 보존하며 이 담당이 직접 실행한 것으로 바꾸지 않는다. root가 같은 Issue35/run을 인계했고, source base `2964f7ea3f8c43f147422107f40629f51c010698` 및 docs base `d679cf57592b285a30d762784fdb62ae0bf73581`와 기존 브랜치·미커밋 변경을 유지한다. 세 저장소 AGENTS와 제품 스펙·status·roadmap·daily/continuous-development, 소유 테스트 두 파일 및 root의 공유 판정/URL·정정 후 conflict 계산을 읽었다. 시작 source Git status는 기존 변경들과 새 테스트 두 파일을 확인했으며 다른 담당의 diff를 되돌리지 않았다.

읽기 검토에서 실제 ledger 기반 기대값이 앱 matcher/`confirmedNumbers`를 재사용하지 않고 전체 정정 후 번호 중복 수를 독립 계산하는지, 합성 거절 반례·AND·URL/history·원 저장 바이트·보류/억제·JS-off 안내를 유지하는지 확인했다. 더보기는 이미 동결 `staging.songs.length` 상한과 클릭마다 엄격한 행 증가를 검사하도록 보완되어 있었으므로 다시 구현하지 않았다.

새 모바일 회귀에서 두 검증 누락을 찾아 root가 범위를 승인한 뒤 같은8번째 검사 안에서 보완했다. home/search는 독립 `tjIds` 전체, favorites는 실제 심은 저장3ID 중 확인집합에 속하는 ID와 **정확한 행 수·ID집합**을 geometry 전에 요구한다. 빈 목록은 geometry 대상이 없어도 PASS하지 못한다. 번호 복사 버튼의44px 요구는 높이와 폭을 모두 검사한다. 새 테스트 개수는 늘리지 않고 총8개를 유지했다. 단위 소스·제품/공용 계약·Git·package/lockfile·CI·dist/서버·배포는 수정하지 않았다.

이 보완 뒤 실제 브라우저 재실행은 root가 단일 실행자로 수행한다. 이전8/8이나 재시작 직전 소실된 `/tmp` 로그로 이번 보완 후39개 전체의 결과를 확정하지 않는다. root는 재시작한 canonical `npm test`325·타입0·preview artifact3347files/3336HTML/0index 성공을 통보했으나, 이 담당의 직접 실행 결과가 아니며 root의 영속 영수증과 연결해 확인해야 한다. 현재 읽기/편집 인계를 완료하고 소유 소스 두 파일을 교차평가용으로 동결했다. 실제 원격 preview/운영·복구·Release/AdSense는 별도 미완료 게이트다.
