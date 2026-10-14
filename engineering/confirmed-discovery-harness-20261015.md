# 공식 확인곡의 가수 탐색 검증

작업: `resume-20261015-0300` · [소스 Issue 6](https://github.com/jpop-today/jpoptoday/issues/6) · CONTENT-001/SONG-001/DATA-001/SEO-001/DESIGN-001/HARNESS-001/RELEASE-001 · [제품 스펙](../product/spec.md) · [ADR-006](../adr/006-reviewed-corrections-and-production.md).

담당: `/root/resume_0300_quality`. 기준 source `d17b9c8ad555cd97214c67b864a9c263be402186`, docs `78c91f42a01b10639c01463e84a7c2a427d503d1`. 담당 파일은 소스 `tests/confirmed-discovery.test.mjs`, `e2e/artists.spec.ts`, 이 문서뿐이다. 공용 계약·타입·package/lockfile·Git·CI·서버/빌드·배포는 root가 맡는다. 다른 담당자의 미커밋 파일은 수정하지 않았다.

## 경계와 독립 검증

검증 흐름은 `/artists/` → 원문 가수 표기의 fragment → 기존 안정 ID 곡 상세 → 공식 제목·가수·번호·대조시각 → 기존 저장 목록이다. 이 페이지는 검토한 일부 현재 playlist 곡을 탐색하는 디렉터리이며 별칭·공동명의의 실제 인물 정체성이나 제공사 전체 수록률을 추정하지 않는다.

root의 `contracts/confirmed-discovery.mjs`는 이미 엄격하게 검증한 ADR-006 공개 모델을 입력으로 삼는다. `confirmedNumbers`는 current/playlist, provider-confirmed, conflict가 정확히 false인 값, 비어 있지 않은 공개 근거와 업체·번호가 일치하는 공식 numeric URL만 제공한다. 날짜의 달력/미래 검증과 실제 제목·가수·버전 검토는 기존 ledger 검증 경계의 책임이다. 이 helper가 공식 자료를 새로 조회하거나 사람의 검토를 대신하지 않는다.

11개 단위 검증은 다음 위험을 확인한다.

- observed/unknown/registered/unlisted 값을 근거가 있는 공식 번호로 승격하지 않는다. conflict true와 불명확한 conflict 값도 제외한다.
- 문자열 선행 0을 유지하고 숫자형·모든 0·부호·지수·소수·전각·줄바꿈·11자리 값은 거부한다. 실패 fixture에도 같은 번호의 URL을 넣어 단순 URL 불일치로 통과하지 않도록 했다.
- 근거가 없거나 URL/checkedAt/title/artist가 빈 경우에는 페이지 탐색에 포함하지 않는다.
- archived/chart-only는 근거가 있어도 이 디렉터리에서 제외한다.
- 공식 URL은 업체와 번호가 일치해야 하며 http, 다른 호스트, credential, 명시 port, fragment, 추가·중복 query, 다른 번호/검색타입/업체를 제외한다. 기존 공식 numeric URL 규칙을 재사용한다.
- 중복/빈 song ID는 비대상 곡과 중복돼도 거부한다. 포함할 곡의 원문 가수명이 없으면 실패한다.
- 대소문자·별칭·공동명의가 다른 원문 문자열은 각각의 그룹이다. 같은 원문 표기에 저장된 한국어 표기만 중복 없이 모은다.
- 순서 변경과 곡/다른 가수 추가에도 anchor가 유지된다. anchor는 기존 song ID나 새 영구 저장 ID가 아니다.
- deep-freeze 입력에서 안정 ID·Unicode와 literal `%`의 URL encoding·문자열 번호·원래 메타데이터를 유지한다.
- 실제 동결 staging과 ledger에서 유효 번호를 독립 재계산한다. 전체 ID의 raw 번호에 confirm/suppress를 적용해 업체별 중복 수를 산출하고, current/playlist·confirm·유일번호인 ledger 항목을 기대집합으로 사용한다. helper의 출력 자체로 기대집합을 생성하지 않는다. 이 집합과 탐색 ID·번호·공식 URL/확인일/제목/가수 및 기존 href를 대조한다.

## 실제 실행과 실패

2026-10-15 03:08 KST까지 Node 24.19.0에서 `node --test --test-isolation=none tests/confirmed-discovery.test.mjs`를 실행해 **11개 PASS, 0 FAIL**을 확인했다. 기본 `node --test tests/confirmed-discovery.test.mjs`는 이 환경에서 file-level 1개 PASS만 요약했으므로 이를 개별 assertions 11개 통과의 증거로 쓰지 않았다. 직접 실행 및 무격리 실행에서 개별 테스트 이름과 실제 11개 결과를 확인했다.

첫 중복 ID negative 추가 실행은 **10 PASS/1 FAIL**이었다. fixture의 `song(undefined)`가 JavaScript default parameter로 정상 ID를 넣어 버린 테스트 작성 오류였다. `song('invalid', { id })`로 잘못된 ID를 실제 주입하도록 수정했고 이후 개별 11개 PASS를 확인했다. 앱 계약 실패로 기록하지 않으며 처음 실패를 숨기지 않는다.

root가 실행한 `/tmp/jpop-0300-unit.txt`와 `/tmp/jpop-0300-check.txt`를 직접 읽었다. 전체 `node --test --test-isolation=none tests/*.test.mjs`는 **286 PASS/0 FAIL**(7,984.56ms), Astro check는 **26개 파일·0 errors/0 warnings/0 hints**였다. 직접 읽은 root 실행 결과이며 품질 담당자가 별도로 전체 빌드·CI를 실행한 것으로 쓰지 않는다. `e2e/artists.spec.ts`의 TypeScript transpile syntaxErrors 0, 이 문서의 상대 링크 4개 모두 실제 존재도 직접 확인했다.

03:08:13 KST 읽기 시 ledger는 63행, semantic SHA-256 `e38ec78bda5874c5ffb4102b4b6bd7c3c9bedabea34c789055ecd51eb0a0d60e`였다. 당시 직접 projection은 **19개 가수 원문 그룹·20개 song ID·38개 충돌 없는 공식 번호**였다. 이는 미커밋 통합 상태의 관측이며 배포나 최종 릴리스 완료 주장이 아니다. 통합 담당자가 최종 source/입력 hash/브라우저/CI/배포 증거와 연결한다. ledger 조사 근거는 [공식 번호 검토 기록](../data/provider-review-20261015.md), 페이지 행동은 [UI 기록](artist-discovery-20261015.md)에서 확인한다.

## 브라우저 하네스와 현재 상태

Browser plugin과 browser skill이 이 세션에 제공되지 않아 **Browser plugin not available**로 분류했다. frontend-testing-debugging skill을 읽었으며 기존 Playwright/Chromium과 저장소 E2E 흐름을 사용한다. 새 브라우저 dependency는 설치하지 않았다. root가 서버와 preview/production 빌드 실행을 조정한다.

`e2e/artists.spec.ts`의 3개 흐름은 다음과 같다.

| 흐름 | 수용 기준 | 이 기록 작성 시 실행 상태 |
| --- | --- | --- |
| 실제 `/catalog.json`과 가수 디렉터리 대조 | 별도 기대집합과 모든 표시 ID·원문 가수 그룹·번호·근거·확인일이 일치. 광고 요청 없음, title/canonical/meta/sitemap의 실제 target 일치, overlay·console 오류 없음 | root의 최종 로컬 preview 실행 PASS |
| JS-off 360×800에서 키보드 anchor→곡 상세 | 실본문, 가로 넘침 없음, 원문 가수 anchor focus/Enter→기존 곡 href focus/Enter→공식 근거. page/console 오류 없음 | navigation 대기 보완 후 최종 PASS; 첫 실패 보존 |
| JS-on 탐색→즐겨찾기→legacy URL→저장 목록 | 기존 `{ id, note }` 객체와 미해결 ID 보존, 새 곡만 문자열 ID로 추가, `/song.html?id=`가 같은 안정 주소로 연결, 두 저장곡과 미해결 항목 유지 | root의 최종 로컬 preview 실행 PASS |

`JPOP_SCREENSHOT_DIR`가 설정됐을 때 desktop 1440×1100과 mobile 360×800 첫 화면을 외부 증거 경로의 `artists-desktop.png`, `artists-mobile.png`에 저장하도록 했다. 초안 작성 당시에는 브라우저와 스크린샷이 미실행이었다. 이후 root가 첫 preview E2E에서 저장한 `/tmp/jpop-0300-screenshots/artists-desktop.png`와 `artists-mobile.png`를 직접 열어 읽었다. 첫 화면에 실제 제목·19/20/38 범위·파랑/분홍 선과 파스텔 배경이 보이고, 360px에서도 본문·요약·‘가수 목록으로 바로가기’ 링크가 잘리지 않았다. 이는 실패한 navigation 테스트를 PASS로 바꾸는 증거가 아니며 실제 재검증 결과는 아래에 구분한다.

첫 전체 preview E2E는 **21 PASS/1 FAIL**(33.4초)이었다. `/tmp/jpop-0300-preview-e2e-final.txt`의 실제 실패 로그를 직접 읽었다. JS-off 키보드 Enter 후 다음 문서의 navigation이 완료되기 전에 `new URL(page.url()).pathname`을 즉시 읽어 이전 `/artists/`를 기대 경로와 비교했다. UI 담당의 독립 브라우저 관측에서도 곡 제목은 올바른 상세로 전환됐고 즉시 URL 읽기만 이전 값을 잡았다. root가 승인한 source freeze 예외 범위에서 기존 경로 조건을 느슨하게 바꾸지 않고 `await expect(page).toHaveURL(pathFor(song.id))`로 실제 navigation을 기다리도록 수정했다. fragment 변경은 `expect.poll`로 기다리고, JS-on 상세 클릭도 같은 URL 대기 조건을 사용했다. 수정 후 root가 전체 E2E를 다시 실행했으며 `/tmp/jpop-0300-preview-e2e-passed.txt`를 직접 읽어 **22 PASS/0 FAIL**(34.3초)을 확인했다. 위 세 흐름과 기존 19개 회귀 모두 실제 PASS다. 첫 실패 로그와 마지막 성공 로그는 별개다.

최종 로컬 preview는 기준 source SHA의 미커밋 통합 작업본이다. root가 검증한 3,335 HTML·3,345파일·0 index를 clean CI/배포 증거로 바꾸지 않는다. UI 담당의 별도 [3 viewport 실제 receipt](../operations/runs/resume-20261015-0300-ui-qa-verified.json)를 읽고 1440×1100·360×800 JS-off·1280×768 모두 실제 title/본문·19/20/38·광고 없음·콘솔 error/warning 없음·overflow 없음·공동명의 anchor→JANE DOE 안정 URL/공식 근거 PASS를 확인했다. 초기 UI QA navigation/fragment race는 최종 receipt와 분리해 보존됐다.

다음 이미지들을 직접 읽었다. 처음 화면은 가수/곡/번호의 분모와 기존 그라데이션이 보이고, 360px 공동명의 그룹은 개인 `米津玄師` 그룹과 합쳐지지 않으며 기존 한글 표기·JANE DOE·TJ52972/금영57784·공식 제목/가수·실제 대조 시각을 모두 잘림 없이 보여 준다. 번호 정보·원문 제목·가수 외의 약력/난이도/권리를 만들어 넣지 않았다.

- [Desktop 첫 화면](design/artists-20261015-desktop.png)
- [Mobile 첫 화면](design/artists-20261015-mobile.png)
- [Mobile 공동명의 그룹](design/artists-20261015-mobile-joint-anchor.png)

검토 시점의 clean SHA CI·Cloudflare preview·실제 production·복구·Release는 root의 다음 검증 단계이며 이 로컬 성공으로 완료했다고 주장하지 않는다. 이후 root가 제공한 실제 결과만 이 문서의 후속 증거로 연결한다.

남은 한계: Chromium 외 브라우저, 필드 Core Web Vitals, Google 계정·AdSense 실제 심사, 자료 재배포 권한은 이 하네스가 확인하지 않는다. 모든 preview는 noindex·광고 없음이며 어느 테스트 수나 콘텐츠 개수도 승인을 뜻하지 않는다.

## 담당자가 제출한 읽기 전용 동료 평가 원본

아래는 실제 파일과 로컬 최종 검증 증거를 읽은 뒤 root에 제출한 두 평가 JSON의 원본이다. 점수·근거를 이후 운영 단계의 성공 여부에 맞춰 바꾸지 않는다. 각 담당자의 다른 두 평가와 함께 root가 실제 6개 리뷰를 검증하고 roster를 결정한다.

```json
[
  {
    "reviewer": "/root/resume_0300_quality",
    "candidate": "/root/resume_0300_evidence",
    "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 4 },
    "criterionReasons": {
      "correctness": "기존43entries와새20entries를 직접읽어 전자객체/순서불변, 후자current playlist stableID·string number·원래관측값 대응을 확인. numeric1행의 공식제목/가수/부제/공동명의가ledger와일치하고unknown/unlisted·버전합병추정없음.",
      "evidence": "requests20 safe metadata·저장raw20 SHA256·UTF8strict·정확한resultcell재파싱을 독립실행하여URL/checkedAt/title/artist/hash 20/20일치. docs실제20조회시각·hash·검토판정과root286/type0/full22PASS로그를 실제읽음.",
      "value": "동결차트top10곡의TJ/KY20개관측번호가실제공식근거를얻어 안전탐색10→20ID/18→38번호로증가. 제공사별범위/나머지미확인도보존하여현장번호확인에직접가치.",
      "maintainability": "별도ledgerappend와공식URL/checkedAt/reason,기존43보존·lastgood경계는명확. 다만조사parser가/tmp ad hoc이고최초KY원문주변출력에modal가사혼입1회발생; 이후metadata-only추출/미공개/사고기록개선했으나 영구검토도구/정규화safe receipts관리후속이남음."
    },
    "evidence": [
      "source:data/reviewed/provider-corrections.json",
      "docs:data/provider-review-20261015.md",
      "/tmp/jpop-provider-review-20261015/requests.json",
      "/tmp/jpop-provider-review-20261015/validation.json",
      "/tmp/jpop-provider-review-20261015.py",
      "/tmp/jpop-0300-preview-e2e-passed.txt"
    ],
    "reason": "실제파일·20원문hash/metadata재파싱·root실행증거를읽은총19점. 조사처음오류/임시parser한계는유지보수성에반영하고자료량이나말수로평가하지않음. rights/Google심사/cleanCI/실제배포는미확인root운영단계로제외.",
    "acceptance": "accept-local-implementation; clean CI/preview/production/recovery remain root gates"
  },
  {
    "reviewer": "/root/resume_0300_quality",
    "candidate": "/root/resume_0300_ui",
    "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 4 },
    "criterionReasons": {
      "correctness": "artists.astro/계약API/실제catalog를읽음. currentplaylist·안전confirmedNumbers만표시,원문가수exact그룹/공동명의별도·기존songHref·원래checkedAt/공식표기·unsafeprovider번호보류/unknown≠미수록이맞음. full22E2E에서servedcatalog와모든20ID/38번호가일치.",
      "evidence": "rootfinal22PASS34.3s로그,UI3viewportverifiedreceipt와실제Desktop/Mobile/MobilejointPNG3개를직접읽음. 360pxJSoff·키보드anchor→stableURL→officialevidence·consoleerror/warning/광고/overflow없음·gradient실제관측. 최초navigation/fragmentrace실패증거와별도성공receipt보존.",
      "value": "원하는가수부터검토된업체번호/버전/공식근거로이동하는정적경로가생김. 첫viewportjump로긴안내를건너뛰고,부분검토범위를19표기/20ID/38번호로분리하며 공동명의한글표기를완전정체성번역으로오해하지않게label붙임. 기존검색·정정가이드·상세·즐겨찾기연결을확인.",
      "maintainability": "기존전역CSS/저장키변경없이scopedAstro/nativeanchor·공용helper재사용·별도pageJS없음. scopedstyle/상태설명은다른coverage컴포넌트와일부중복되어향후공통화여지가있고3viewportChromium관측범위의한계를문서에명시함."
    },
    "evidence": [
      "source:src/pages/artists.astro",
      "docs:engineering/artist-discovery-20261015.md",
      "docs:operations/runs/resume-20261015-0300-ui-qa-verified.json",
      "docs:engineering/design/artists-20261015-desktop.png",
      "docs:engineering/design/artists-20261015-mobile.png",
      "docs:engineering/design/artists-20261015-mobile-joint-anchor.png",
      "/tmp/jpop-0300-preview-e2e-passed.txt"
    ],
    "reason": "실제source·문서·브라우저receipt·3이미지·최종22회귀를읽은총19점. 최초즉시URL조회race는UI코드오류가아니고대기수정후실제통과함을구분. cleanremoteCI/배포/Google은아직root미완료단계로점수에추가하지않음.",
    "acceptance": "accept-local-implementation; clean CI/preview/production/recovery remain root gates"
  }
]
```
