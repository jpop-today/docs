# /coverage에서 공식 확인곡 찾기

상태: 구현·로컬 통합 검증 완료, 최종 clean SHA/CI/배포는 pending. 작성 기준: 2026-10-14 KST. 미커밋 구현 검증을 운영 반영으로 기록하지 않는다.

## 작업 계약

| 항목 | 값 |
| --- | --- |
| Issue | source [#15](https://github.com/jpop-today/jpoptoday/issues/15), 자료 확인 후속 [#6](https://github.com/jpop-today/jpoptoday/issues/6) |
| Spec / ADR | DESIGN-001, SONG-001, DATA-001, SEO-001, CONTENT-001 / [ADR-006](../adr/006-reviewed-corrections-and-production.md) |
| 담당 | `/root/resume_2000_ui` · `resume-20261014-2000` 앱·디자인 |
| Source base | `3422391c3f4876cd68e324eeadbb5219fd243959` |
| Docs base | `648e5b338b56f292cb652728cdc997860a8db840` |
| 브랜치 | `codex/15-deployment-healthcheck-20261014` · root가 Git 조작 |
| 독점 파일 범위 | source `src/pages/coverage.astro`, `e2e/coverage.spec.ts`; docs `engineering/coverage-ui-20261014.md` |
| 의존성 | 동결 staging에 reviewed ledger를 적용한 공개 v2 모델; 기존 `indexableSong`, `songHref`, 브랜드 CSS |

이전 16시 팀의 usage-limit 실패 기록과 별개 실행이다. 작업자는 Git/package/lockfile/global CSS/CI/배포와 원본 staging/ledger를 수정하지 않는다. 타 작업자 파일·미커밋 변경을 덮어쓰지 않는다.

## 사용자에게 달라지는 점

이전 범위 페이지는 번호가 있는 항목 수만 보여 공식 대조와 관측을 따로 찾기 어려웠다. TJ·금영 각각의 공식 대조/관측/확인 필요 상태를 표로 제공하고, 곡 ID 수와 사업자 번호 건수를 나눠 설명한다. 같은 페이지에서 충돌 없는 공식 확인곡의 이름·가수·번호 문자열, 공식 표기, 실제 대조 시각과 공식 검색 링크를 읽고 기존 상세 주소로 이동할 수 있다.

사용 흐름은 `/coverage/` → ‘공식 확인곡 보기’ → 제목·가수 및 사업자별 근거 읽기 → 안정 ID의 곡 상세다. 페이지 안의 목록 이동과 상세 링크는 정적 HTML이며 JavaScript 없이 동작한다. 공식 근거는 새 탭 링크로 명시한다. 확인일은 원래 ledger의 `checkedAt` 그대로 `<time datetime>`에 보존하고 한국 시간으로 표시한다. 이번 구현 시각을 공식 자료 재확인 시각으로 쓰지 않는다.

범위 설명에는 전체 업체 수록률 또는 중복 제거된 공식 곡 수라는 주장을 하지 않는다. `unknown`은 미수록이 아니며 오류·버전 보류 번호도 이 상태에 포함된다. 번호 충돌은 공식 대조가 있어도 별도 제한이다. 목록에는 current playlist 중 `indexableSong`인 항목만 포함하고, 각 업체의 표시 번호는 `provider-confirmed` + evidence + 충돌 없음일 때만 출력한다. 나머지 업체는 확인 필요/관측/충돌 상태로 안내한다. 기존 ID/URL/localStorage/string 번호에는 변경이 없다.

## 읽은 동결 자료의 실제 집계

2026-10-14 이번 작업에서 source base의 공개 v2 projection을 Node로 읽었다. 등록 목록은 3,299 ID이며 차트 전용 18 ID는 모든 집계·확인곡 목록에서 제외한다.

| 업체 | provider-confirmed | observed-unverified | unknown | 합계 |
| --- | ---: | ---: | ---: | ---: |
| TJ | 12 | 3,123 | 164 | 3,299 |
| 금영 | 10 | 2,481 | 808 | 3,299 |

사업자별 공식 번호 22건은 12개의 곡 ID에 속한다. 그중 4건은 두 곡 ID의 동일 번호 충돌이 있어 확인곡 목록에서 제외된다. 실제 목록은 충돌 없는 10개 ID·18개 번호다. 반증/버전 보류로 unknown인 13건(TJ 4, 금영 9)은 unknown 합계에 이미 포함된다. 수치만으로 전체 목록의 공식 검증 완료 또는 AdSense 승인을 뜻하지 않는다.

번호 정정의 구체적 예로 전영호의 `Butter-Fly` 금영 표시는 51714이며 이전 40554를 노출하지 않는다. 튜링 러브의 금영은 확인 필요이며 57803을 노출하지 않는다. 공식 제목에 있는 기기 제한과 말줄임을 임의로 제거·복원하지 않고 그대로 보여준다.

## 수용 기준과 검증 인계

- 상태 표의 TJ·금영 각 합계가 3,299이며 차트 항목은 제외된다. 공식 번호 22건/12곡 ID/충돌 없는 10곡 ID를 별개로 보여준다.
- 10개 확인곡·18개 번호만 목록에 나오며 번호 문자열·공식 제목/가수·근거 URL·원래 `checkedAt`·안정 상세 링크가 공개 모델과 일치한다. 억제한 번호와 충돌 번호는 목록의 예약 번호로 나오지 않는다.
- 기존 blue `#4f7cff` → pink `#ff7aa8` 브랜드와 pink/mint/blue 배경을 유지한다. 공용 CSS는 수정하지 않고 페이지 안에서 상태 표·카드·링크를 배치한다.
- 360px에서 가로 넘침이 없으며 JavaScript 없이 목록 이동/키보드 Enter/상세 링크를 사용할 수 있다. 페이지 title/h1/본문·오류 overlay 없음·console·스크린샷을 확인한다.
- 모든 preview의 noindex·광고 없음, production의 기존 metadata/sitemap 계약은 root 전체 artifact/브라우저 검증에서 확인한다.

Browser plugin not available. `build-web-apps:frontend-testing-debugging` skill에 따라 기존 Playwright를 사용했다. 중복 서버·빌드가 생기지 않도록 작업자는 브라우저·build/type를 실행하지 않았으며 root가 실행했다. 아래 로컬 실행 결과는 root의 실제 결과 인계이며, 화면과 `evidence.json`은 앱 담당도 직접 열어 읽었다.

`e2e/coverage.spec.ts`는 (1) 분모/세 상태/번호 대 곡 수/unknown 설명, (2) 실제 공개 모델의 공식 근거와 정정 번호·상세 이동, (3) JavaScript 없는 360px에서 팔레트/가로 넘침/키보드 링크를 검증하도록 작성했다. 고정 스냅샷의 숫자는 이 문서의 기준선으로 명시하며 향후 공식 ledger를 갱신할 때 실제 검토로 테스트 기준을 바꾼다.

| 검사 | 상태 | 근거 |
| --- | --- | --- |
| AGENTS/spec/ADR 읽기 | 완료 | 세 저장소 규칙, 제품 v0.4, ADR-006 |
| 공개 v2 실제 상태 집계 | 완료 | 동결 staging + 실제 ledger projection의 Node 읽기; 상기 집계 |
| 작업 범위·소스 diff 확인 | 완료 | coverage 파일만 변경, 새 E2E/doc 파일 |
| Node24 unit | PASS | 초기 root 통합 244개 통과; 이메일 query 하네스 보완 뒤 최종 root 로그 `jpop-v031-unit-final-query.log`를 직접 읽어 274개 통과/실패·skip0/5.854초 확인 |
| Astro type | PASS | root final check 25 files, 0 errors/0 warnings |
| preview build/artifact | PASS | root 3,334 HTML/3,343 files/색인 경로 0 검증 |
| Playwright /coverage + 전체 회귀 | PASS | root 시스템 Chromium 19/19, 20.7초; coverage의 3개 흐름·새 가이드 2개 포함 |
| desktop/360px 화면·console·키보드 | PASS (로컬) | root identity/h1/본문 확인, console errors `[]`, overflow `false`; 앱 담당 PNG 3장과 evidence JSON 직접 읽기; JS 없는 키보드/상세 이동은 coverage E2E 통과 |
| CI / 동일 SHA preview·운영 | pending | root 통합 이후 별도 실행 |
| 두 동료의 실제 파일 평가 | pending | 구현 완료 후 읽기 평가 요청 |

로컬 QA의 source HEAD는 base `3422391c3f4876cd68e324eeadbb5219fd243959`, `dirty: true`다. 즉 2026-10-14 이번 working tree 구현을 확인한 증거이며 clean commit의 동일 SHA CI/preview/운영 증거와 다르다. 이 시점의 managed live 호스트 검증과 하네스 보완은 별도 담당 작업으로 이어진다.

## 화면과 실사용 증거

root 캡처 시각은 `2026-10-14T11:15:17.369Z`(한국 시간 20:15:17)다. 원래 로컬 캡처는 `/tmp/jpop-v031-qa/`에 생성했고, root가 아래 세 이미지를 docs의 검토 증거로 보존했다. 캡처 metadata는 [실행 JSON](../operations/runs/resume-20261014-2000-ui-local.json)에 기록했다. 이미지와 기록은 문서 저장소의 증거이며 앱의 공개 dist에 포함하지 않는다.

| 화면 | 실제 크기/주소 | 앱 담당의 읽기 결과 |
| --- | --- | --- |
| [coverage desktop](design/coverage-20261014-desktop.png) | 1440×1100 · `http://127.0.0.1:4321/coverage/` | 제목·분모·세 요약 수치와 업체별 세 상태 표가 읽히며 겹침/잘림·framework overlay가 없다. blue→pink 로고와 pink/mint 배경이 보인다 |
| [coverage mobile](design/coverage-20261014-mobile.png) | 360×800 · 같은 주소 | 한 열 요약 카드가 폭 안에 들어온다. 제목·본문·목록 이동 링크가 읽히며 가로 넘침이 없다. 첫 viewport 아래의 상태 표/확인곡·상세 이동은 E2E DOM/키보드 검증으로 확인했다 |
| [guide mobile](design/number-checking-guide-20261014-mobile.png) | 360×800 · `/guides/checking-song-numbers/` | 실제 번호 확인 가이드의 제목·본문 확인일·첫 단계가 읽힌다. 날짜가 줄바꿈되지만 잘리지 않는다. 이후 사례·공식 링크는 root 새 가이드 E2E로 확인했다 |

확인 흐름은 `/coverage/`의 ‘공식 확인곡 보기’에 키보드 포커스 → Enter로 `#reviewed-songs` 이동 → 전영호 Butter-Fly 제목에 포커스 → Enter로 기존 안정 상세 주소 이동이다. JavaScript를 끈 360px context에서 실제로 통과했다. 다른 브라우저·현장 반주기·Lighthouse/필드 Core Web Vitals는 이번 QA로 확인하지 않았다.

로컬 검증 완료를 CI 또는 운영 배포 완료로 기록하지 않는다. AdSense는 실제 계정/권리/심사 결과 확인이 남은 별도 목표다.
