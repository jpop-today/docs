# 업체별 공식 확인곡 검색 UI · Issue35

- 작업: [Issue35](https://github.com/jpop-today/jpoptoday/issues/35), `resume-20261017-2200`.
- 스펙: [SEARCH-001/DATA-001/SONG-001/FAVORITES-001/CONTENT-001/DESIGN-001](../product/spec.md#업체별-공식-확인곡-검색--issue35-2026-10-17).
- 담당: `/root/resume22_app`. 기준 source SHA `2964f7ea3f8c43f147422107f40629f51c010698`, docs SHA `d679cf57592b285a30d762784fdb62ae0bf73581`.
- 편집 범위: 소스 `src/components/Search.astro`, `src/scripts/app.ts`, 이 문서. 공용 계약·Git·package/lockfile·CI·배포는 root 담당이다.

## 구현

홈·검색·즐겨찾기가 공유하는 Search에 `TJ 공식 확인`/`금영 공식 확인` 버튼을 추가했다. 기존 `number` 토글과 `aria-pressed`를 사용하며 각각 `confirmed-tj`/`confirmed-ky`를 URL에 저장한다. 두 조건은 root의 공용 matcher에서 AND로 판정한다. 기존 전체 버튼은 번호 조건만 해제하며 검색어와 나머지 필터를 유지한다. 조건 해제 버튼의 기존 전체 query 초기화, 새로고침·뒤로가기, 저장·복사·recent 저장 형식은 바꾸지 않았다. 선택 조건과 최근 번호 필터에는 한국어 라벨을 표시한다.

공식 확인 안내는 선택한 업체의 제목·가수 대조 근거 범위만 설명한다. 두 공식 확인 조건을 함께 선택하면 양쪽 근거가 필요하고, 선택하지 않은 업체의 번호는 관측값일 수 있음을 명시한다. 0건 문구는 공식 미수록을 뜻하지 않는다고 설명하며, 실제 결과 행의 업체별 공식 확인 배지·관측 번호·확인 필요 상태는 기존 코드를 그대로 사용한다. 새 번호·확인일·심사 결과를 생성하지 않는다.

번호 조건 버튼은 `role=group`, 설명 연결, 개별 pressed 상태를 유지한다. Search 컴포넌트에 포함한 CSS로 버튼을 줄바꿈하고 최소 44px 높이·폭을 지정했다. 원본 글로벌 CSS의 파랑 `#4f7cff`→분홍 `#ff7aa8` 그라데이션과 pink/mint/blue 배경 팔레트는 수정하지 않았다.

첫 실제 화면 검사에서 360px의 기존 전체 버튼 폭이 `43.921875px`로 44px 목표에 못 미쳤다. 같은 시점 품질 담당의 실제 모바일 캡처를 읽어 결과행의 TJ 배지와 금영 라벨 겹침·금영 배지 세로 줄바꿈도 확인했다. 최초 실패와 원 PNG를 보존하고, 버튼 최소 폭을 44px로 명시했다. 홈·검색·저장 영역 `[data-list-view]`에만 적용하는 모바일 provider grid를 Search 스타일에 추가해 업체 라벨/번호와 그 아래 한 줄 배지를 분리했다. 번호·저장 버튼은 44px를 확보하며, 기존 chart 전용 방식과 같은 배치를 사용한다. 번호·상태·복사·저장 데이터와 글로벌 CSS 파일은 변경하지 않았다. root 재빌드 전 기존 서버 결과를 수정본 성공으로 쓰지 않는다.

JavaScript가 없거나 목록 로딩이 끝나기 전에는 조건 버튼이 비활성이다. JavaScript 없는 화면에는 현재 SSR 목록에 URL 조건이 적용되지 않았음을 명시하고, 기존 [공식 확인 번호 목록](https://jpop.today/coverage/)과 [번호 확인 가이드](https://jpop.today/guides/tj-ky-numbers/)로 연결한다. 즐겨찾기 개인 목록도 JavaScript가 필요하다는 기존 안내를 유지한다.

## 검증 계약과 현재 결과

검증 흐름은 `홈·검색·즐겨찾기 → 업체별 공식 확인 버튼 선택 → URL/pressed/요약/실제 결과 확인 → 두 조건 AND·전체·새로고침·뒤로가기·0건 확인`이다.

- 실제 확인: AGENTS 3개, Issue35 스펙과 run 기록, 기존 Search/app/global CSS/E2E 및 package scripts를 읽었다. 편집한 소스 2파일의 `git diff --check`는 통과했다.
- 브라우저 경로: frontend-testing-debugging과 control-browser skill을 읽고 실제 tool 목록을 검색했다. Browser용 `js` 및 CUA 런타임 도구가 제공되지 않아 연결 브라우저를 사용할 수 없다. 작업 계약에서 허용한 기존 Playwright/system Chromium fallback을 사용한다. 브라우저가 연결됐다고 주장하지 않는다.
- 실행 경계: root가 제공한 artifact/공유 서버만 사용했다. 담당자가 별도 build/server/dist를 만들거나 수정하지 않았다. 아래의 local rendered QA를 clean CI/원격 preview/운영 결과로 대신하지 않는다.

### 최초 실제 화면 검사와 보완

2026-10-17T13:19:01.269Z–13:19:04.672Z(한국 시간 22:19)에 root가 제공한 `http://127.0.0.1:4321` dirty preview artifact를 기존 Playwright/system Chromium으로 검사했다. 1440×900과 768×1024의 page identity/의미 있는 본문/overlay 없음/console health/키보드 Space·Enter/pressed·URL·요약/팔레트/모든 조건 버튼 44px 검사는 통과했다. 세 번째 360×800 검사에서 전체 버튼 폭 `43.921875px`로 실패하여 중단했다. 9개 흐름 전체가 통과한 것으로 기록하지 않는다. 실제 실행 결과 `/tmp/jpop35-app-qa/report.json`, 원 캡처 `search-desktop.png`, `search-tablet.png`를 보존했다. 품질 담당의 별도 원 캡처 `/tmp/jpop-resume22-quality/confirmed-search-mobile-both.png`에서 결과행 겹침을 함께 확인했다. 수정 후 새 root artifact에서 같은 검사와 결과행 bbox/배지 한 줄 검사를 재실행한다.

### 수정 후 실제 rendered QA · PASS

2026-10-17T13:21:59.075Z–13:22:09.197Z(한국 시간 22:21:59–22:22:09)에 root 재빌드 후 새 Chromium context로 **9개 흐름 통과**, 오류 0개, PNG 12개를 생성했다. `build-info`는 source base `2964f7ea3f8c43f147422107f40629f51c010698`/target `preview`인 dirty artifact다. 아직 clean commit SHA의 CI나 원격 배포를 검증한 결과가 아니다.

| 검사 | 실제 결과와 범위 |
| --- | --- |
| 페이지 identity·의미 있는 본문·framework overlay | JavaScript 사용 6개 화면 모두 URL/title·본문·overlay 없음 통과. 홈·검색·저장의 JS-off 3개 화면도 실제 title/h1·설명·대체 링크를 확인했다. |
| console·pageerror | JavaScript 사용 6개 화면의 error/warning/pageerror 0개. JS-off 3개는 별도 console 수집을 하지 않았다. |
| 실제 상호작용 | 검색 1440×900/768×1024/360×800/360×320, 홈 360×800, 즐겨찾기 360×800에서 Space로 TJ 해제→Enter로 재선택, KY 유지·실제 URL·pressed·요약을 확인했다. 빈 즐겨찾기는 공식 미수록을 뜻하지 않는다는 안내를 보였다. |
| responsive·keyboard·44px | 6개 화면 가로 overflow 없음. 모든 번호 조건 버튼 최소 44×44px, 실제 keyboard focus outline 0보다 큼. 모바일 전체 버튼은 실제 44×44px로 보완됐다. |
| 결과행 겹침 회귀 | 검색 360×800/360×320·홈 360×800에서 첫 결과행 업체 cell은 각각 138px, x=16/162로 분리됐다. 업체 번호/저장 버튼은 44px, 배지는 해당 cell 안의 아래 줄에 `nowrap`으로 표시됐으며 겹침 없이 읽힌다. |
| JavaScript 없음 | 홈·검색·저장 360×800의 5개 버튼 비활성, SSR 미적용 안내·44px 대체 링크 확인. coverage 링크에 keyboard Enter로 이동해 실제 `/coverage/` 본문이 열렸다. |
| 브랜드 | JavaScript 사용 6개 화면의 computed style에서 blue `rgb(79,124,255)`→pink `rgb(255,122,168)` 그라데이션 및 pink/mint/blue 배경을 확인했다. |
| 화면 증거 | desktop/tablet/mobile/short/favorites/JS-off를 포함한 12개 실제 PNG. 데스크톱·태블릿·모바일 결과행·짧은 화면 결과행·즐겨찾기·JS-off 검색/즐겨찾기를 직접 열어 읽었다. |

실행은 기존 `@playwright/test`의 Chromium API, `/usr/bin/chromium`, temporary script `/tmp/jpop35-app-qa-fixed.mjs`, root의 동일 `http://127.0.0.1:4321` 서버로 수행했다. 전체 결과는 `/tmp/jpop35-app-qa-fixed/report.json`이며 PNG는 같은 디렉터리에 있다. 최초 실패 디렉터리는 덮지 않았다. 임시 파일의 증거 저장소 통합은 root가 담당한다.

새 Chrome/의존성 설치, 운영 광고·계정·개인정보 입력, Git/배포를 실행하지 않았다. 실제 기기·다른 브라우저 엔진·스크린리더·접속 성능, 이 UI 검사와 별개인 전체 단위/type/정적 경계·재현/clean CI/원격 preview·운영·복구는 root 통합 결과를 확인해야 한다. 현재 앱 편집 범위를 동결하고 동료 읽기 평가를 기다린다.

수용 기준은 기존 q/has/only/tag/anime/artist/page와 storage 원본 보존, 업체별 근거 조건과 두 조건 AND, URL reload/back/clear, 0건 안내, 세 화면의 키보드·360px 가독성/overflow·44px hit area, JS-off 비활성·대체 링크, console 오류/blank/framework overlay 없음이다. 브라우저 출력은 실제 실행한 target과 SHA의 결과로만 기록하며 로컬 성공을 원격 배포 성공으로 쓰지 않는다.

## 환경 재시작 후 앱 담당 인계 · 2026-10-18 KST

2026-10-17T18:10:06Z(한국 시간 10월18일03:10:06) 인계 확인에서 `/root/resume35_app`은 위 소스 두 파일의 현재 내용·diff, 세 저장소 AGENTS와 필수 운영/제품 문서를 읽었다. 위 브라우저 실행의 실제 담당은 `/root/resume22_app`이며, 인계 담당이 과거 QA를 직접 실행한 것으로 바꾸지 않는다. 기존 미커밋 앱 변경은 그대로 보존했고 새 소스 변경이나 중복 build/server/전체 테스트를 실행하지 않았다. 공유 matcher의 AND 판정과 Search의 업체별 설명·0건/JS-off 안내, URL/전체/조건 해제·history·저장 경로를 읽기 대조했으며 추가 앱 수정이 필요한 결함은 확인하지 못했다.

root가 저장한 [최초 QA JSON](../operations/runs/resume-20261017-2200-app-initial-qa.json), [수정 후 QA JSON](../operations/runs/resume-20261017-2200-app-fixed-qa.json), [UI 증거 색인](../operations/runs/resume-20261017-2200-ui-qa.json)을 직접 읽었다. 색인에는 최초 실패와 수정 후의 실제 시각·dirty preview 범위 및 PNG 21개의 영속 경로가 있다. 인계 담당은 [최초 모바일 겹침](design/confirmed-search-20261017-2200/quality-initial-confirmed-search-mobile-both.png), [수정 후 업체별 행](design/confirmed-search-20261017-2200/quality-fixed-confirmed-search-mobile-provider-rows.png), [JS-off 검색 안내](design/confirmed-search-20261017-2200/app-fixed-js-off-search.png)도 직접 열어 기록과 대조했다.

위 `/tmp` 경로들은 당시 실행 경로이며 환경 재시작 후 존재하거나 다시 실행할 수 있다는 보장이 아니다. 영속 JSON/PNG는 이전 관측의 근거이고 현재 환경에서 새 브라우저를 실행한 결과가 아니다. 새 전체 검증·clean SHA CI·실제 preview/운영·복구의 최종 판정은 root의 후속 실행 기록을 따른다. 앱 소스와 이 인계 문서는 동결하고 두 동료의 읽기 평가를 기다린다.
