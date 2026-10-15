# 차트 업체별 공식 확인곡 탐색 · 품질 검증

작업 `resume-20261016-0400` · [Issue27](https://github.com/jpop-today/jpoptoday/issues/27), 상위 [Issue6](https://github.com/jpop-today/jpoptoday/issues/6). DATA-001, CONTENT-001, SEO-001, DESIGN-001, HARNESS-001. [제품 계약](../product/spec.md#차트의-업체별-공식-확인곡-탐색--issue27-2026-10-16).

담당 `/root/resume04_quality`. 소스 base `ce1b49f1ab6cedb64dca7cd2b215e72aafc76e50`, docs base `5d1964dfeacdad5c6b2c81d2522ce3aadb83de05`, 조직 base `b7aad18fe2f793453b6505bb150f359d05ae3554`; branch `codex/27-chart-confirmed-20261016`. 세 저장소 AGENTS·Issue27 스펙·실행 기록을 읽고 시작 시 source Git status가 clean인 것을 읽었다. Git·공용 파일·빌드·공유 서버·CI·배포는 총관리자만 수행한다.

소유 파일은 소스 `tests/reviewed-chart-cohort0400.test.mjs`, `e2e/chart-confirmed-navigation.spec.ts`와 이 문서의 정확히 3개다. 다른 담당 소스·자료는 읽기만 했다. 기존 공용 테스트·계약·package/lockfile은 수정하지 않았다.

## 검증 계약

- 원 ledger 71개 전체 객체·순서를 canonical JSON SHA256 `35487404af46c08e2bf0471e0bf05fff088c0545b69639a57d9c928390797689`로 고정한다. staging 원본 `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3`, 동결 chart 원본 `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97`, 3317 ID와 chart100 ID/순위도 보존한다.
- 자료 담당의 실제 8요청·별도 편집 8판정을 읽고 **confirm6/defer2**로 회귀 범위를 정했다. HTTP200만으로 8개 모두 확인되었다고 쓰지 않는다. samfree의 TJ feature credit과 Ado OST, KICK BACK OP 부제를 공식 원문 전체로 보존하며, 불명 KY43185의 SAM/samfree 동일성과 KY76509의 생략 부제는 확인하지 않는다.
- 새 6항목의 정확 ID/provider·문자열 번호·공식 URL·실제 checkedAt·전체 제목/가수·response hash를 고정한다. slice0..77의 cohort projection을 원71 projection과 대조해 모든 ID/기존 URL·메타데이터·비대상 provider 전체 객체를 보존한다. 이전 같은 제목 `ドライフラワー`의 다른 Ado ID와 unknown도 유지하며 `unlisted`를 생성하지 않는다. 이후 독립 검토 cohort append는 허용한다.
- E2E 집계는 served catalog와 repository의 동결 chart를 사용한다. `build-info.chartSha256`와 동결 chart semantic hash가 같아야 하고 원100행의 ID·순위·stable anchor·기존 상세 URL 전체가 순서까지 같아야 한다. UI의 `confirmedNumbers`/집계 helper를 import하지 않으며 current playlist·provider-confirmed·충돌 없음·근거 텍스트 자격과 업체별/합집합 ID Set을 독립 계산한다.
- 공식 numeric URL 자격에는 ledger의 **`officialLookupMatches` 계약만 재사용**한다. 이는 UI 집계 구현과 분리된 보안/근거 계약이며 www 금영 허용·port/중복/추가 query 거절 규칙을 별도 좁은 oracle로 바꾸지 않는다. 이전 가수별 E2E의 URL oracle 정합성 비차단 후속을 이번 새 검사에 반영했다. 계약 자체의 edge cases는 기존 단위 검사를 유지한다.
- 업체별 summary/전체합집합/100분모, 순위순 nav ID·순위·실제 확인 번호·원문제목·공식 확인 설명과 aria 연결을 대조한다. TJ+KY 합이 union보다 큰 실제 overlap을 요구해 중복합 오표기를 잡는다. 0은 공식 미수록이 아니며 관측만 있는 provider는 바로가기에 포함하지 않는다. 확인 보류 2개가 후일 실제 검토로 승격되는 것을 영구 금지하는 hardcode는 넣지 않았다.
- `/chart/` → 업체 바로가기 → exact stable chart행 → 기존 곡 상세를 360×800·JavaScript off·키보드로 이동한다. 마지막 TJ와 첫 금영 바로가기를 실제 focus/Enter로 열고 행 viewport·제목의 실제 hit target을 확인해 긴 nav/overlay 가림을 검사한다. canonical·target별 robots·광고 요청/markup 없음·console/overlay·가로 넘침과 blue#4f7cff→pink#ff7aa8/pink·mint·blue 배경도 확인한다. 기존 전체 E2E의 저장/legacy URL 회귀를 대체하지 않는다.
- 모바일 차트100행의 업체 라벨·번호 버튼/unknown·확인 badge 실제 bounding box를 모두 비교한다. 페이지 overflow가 없어도 행 안에서 TJ/금영의 라벨·버튼·badge가 겹치거나 provider 내용이 행 밖으로 나가고 badge가 세로 여러 줄로 찌그러지면 실패한다.

렌더 검증은 `frontend-testing-debugging` skill을 적용한다. **Browser plugin not available**: 이번 제공 목록에는 browser skill·browser 제어 도구가 없으므로 기존 Playwright를 사용한다. 브라우저·의존성 설치는 하지 않는다. 검증 flow는 `/chart/` → 방문 업체의 순위순 바로가기 focus/Enter → 원 chart행 → 기존 곡 상세·공식 근거 URL이다. 총관리자의 최종 artifact/공유 서버가 준비되기 전 자체 빌드·공유 서버·전체 브라우저는 실행하지 않았다.

## 실제 실행과 자료 읽기

| 검사 | 실제 결과 | 명령·범위 |
| --- | --- | --- |
| 새 cohort unit | **4 PASS**, fail/skip/cancel0, **197.164329ms** | `node --test --test-isolation=none tests/reviewed-chart-cohort0400.test.mjs`, 현재 공유 dirty source, Node24 |
| 요청/편집/ledger 독립 대조 | **8/8 PASS**; confirm6/defer2 | 읽기 전용 Python으로 [요청 JSON](../data/provider-review-requests-20261016-0400.json)의 각 frozen rank/ID/문자열 번호·원 unreviewed candidate HTTP200/final URL·checkedAt/hash와 별도 decision/reviewer/reviewedAt·이유 및 실제 append6개 전체 evidence 일치 확인 |
| root 전체 unit / type 로그 읽기 | **314/314 PASS**, fail/skip0, **14,489.143116ms** / **0 errors·warnings·hints** | `/tmp/jpop-0400-unit.txt`, `/tmp/jpop-0400-check-final.txt` 실제 결과를 읽음. 이 담당의 별도 전체 실행 아님 |
| root 정정 후 전체 preview E2E 로그 읽기 | **28 PASS / 50.3초**, 새2검사 각각3.0초 | `/tmp/jpop-0400-preview-e2e-after-jsonfix.txt`. 실제 첫 collection 오류는 아래에 별도 보존. 새 mobile100행 bbox 회귀도 포함 |
| root 전체 production E2E 로그 읽기 | **28 PASS / 50.5초**, 새2검사 각각2.9초 | `/tmp/jpop-0400-production-e2e.txt`. target별 robots/sitemap·기존 storage/legacy URL·guides·100행/새 탐색 회귀 포함. 실제 운영 배포가 아닌 로컬 production artifact 검사 |
| 앱 최종 시각 QA 읽기 | **실제 3viewport PASS**, 확인용 영속 PNG6개 직접 열람 | [QA JSON](../operations/runs/resume-20261016-0400-ui-qa.json)의 전체 bytes와 final2 원본 동일, 영속 PNG12개 각각 원본과 동일. 아래 환경/증거 경계 참조 |
| clean CI·두 target 재현·실제 preview/운영·복구·Release | **미확인** | 총관리자 후속 게이트. 문서/개별 unit 통과와 구분한다 |

자료 담당의 [검토 본문](../data/provider-review-20261016-0400.md), 원 metadata JSON과 현재 ledger를 읽었다. 독립 대조는 저장된 안전 metadata와 편집 판정의 일대일 대응을 검증했다. 품질 담당의 추가 공식 요청은 **0회**이며 저장하지 않은 raw 응답의 재파싱·response hash 독립 재계산을 했다고 주장하지 않는다. 가사/원문 HTML·시크릿·전체 환경은 출력하거나 저장하지 않았다. 권리·오디오 동일성·기기별 지원·Google 실제 심사는 미확인이다.

초기 작성 중 nav의 설명 연결은 provider section 자체가 아닌 실제 enclosing navigation과 각 anchor에 있다는 소스를 읽고 검사 selector를 실행 전에 맞췄다. 테스트가 기대한 markup을 만들려고 앱을 바꾸지 않았다. 아직 실행하지 않은 E2E 실패/통과를 만들어 기록하지 않는다.

앱 담당은 초기 실제 viewport 자동 검사 후 screenshot 직접 읽기에서 360px 원 chart행의 공식 확인 badge가 업체 라벨과 겹치는 문제를 발견했다. 품질 담당도 `/tmp/jpop-resume04-ui/final/mobile-anchor.png`를 직접 열어 TJ badge/금영 라벨 충돌과 금영 badge의 수직 줄바꿈을 확인했다. 이 시각 문제를 페이지 overflow PASS로 가리지 않고 총관리자에게 전달하고, 위의 모든100행 bbox 회귀를 새 E2E에 추가했다. 품질 담당이 브라우저를 별도 실행한 것은 아니며, 앱 담당의 scoped CSS 보완·root 재빌드 후 새 검사까지 통과하는지 기다린다. 기존 공용 SongRow/global CSS는 품질 담당 범위 밖이다.

root의 첫 전체 E2E 수집은 새 파일의 직접 JSON import 때문에 Node24 ESM의 `ERR_IMPORT_ATTRIBUTE_MISSING`으로 실패했다. 실제 오류는 `/tmp/jpop-0400-preview-e2e-final.txt`에 보존되었고, 이 실행에서 28개 브라우저 검사는 시작되지 않았다. 담당이 `readFileSync(new URL(..., import.meta.url), 'utf8')`와 `JSON.parse`로 동결 chart를 읽도록 이 소유 파일만 수정했다. package/config·앱 CSS·수용조건은 바꾸지 않았다. 정정 뒤 `npx playwright test e2e/chart-confirmed-navigation.spec.ts --list`를 직접 실행해 **2검사 정상 수집/exit0**을 확인했다. 이것은 브라우저 실행 PASS가 아니며, Astro type 검사는 e2e를 exclude하므로 type0을 이 수집 오류의 사전검증으로 사용하지 않는다. 전체 새 브라우저 결과는 root 재실행을 기다린다.

파일의 실제 mtime으로 JSON 정정은 `2026-10-15T19:16:24.184050Z`(16일 **04:16:24 KST**)에 반영됐다. 당시 도구 결과에는 직후 `--list`의 두 실제 검사명이 출력됐으며 별도 브라우저 실행은 없었다. 16일08:04KST 총관리자의 **동일 run/동일 담당 재개**를 전달받고, `2026-10-15T23:05:32Z`(08:05:32KST) 실제 시각에 이 파일과 mtime을 다시 읽었다. unit 파일 mtime은 `19:10:06.316068Z`, E2E 파일은 위 정정 이후 그대로다. source 두 파일을 freeze하며 새 team/Git/공식 조회/build/server/browser를 만들거나 실행하지 않는다. root가 `/tmp/jpop-0400-preview-e2e-after-jsonfix.txt`로 수행하는 최종28검사를 기다린다. 시간 경과나 재개를 검사 성공으로 해석하지 않는다.

이후 위 root 최종 preview 로그의 **28 PASS/50.3초**를 직접 읽었다. 원 JSON 수집 실패를 지우거나 브라우저 fail28개로 잘못 세지 않았으며, 이 정정 후 실제 실행에서 기존26개와 새2개가 모두 통과했다. 새2개에는 SSR 수/순위/분모/기존URL/robots/noads·360px JSoff/키보드·모든100행 bbox가 포함된다.

앱 최종 QA는 `2026-10-15T19:15:18.515Z–19:15:24.787Z`(16일04:15KST), root dirty preview `http://127.0.0.1:4321`, system Chromium `/usr/bin/chromium`에서 수행됐다. 1440×1100 JSon·360×800 JSoff·1280×768 JSon의 JSON을 실제 읽었다. final2 원본과 root 영속 JSON 전체 및 PNG12개 각각이 byte 동일함을 별도 읽기 스크립트로 확인했다. 실제 화면은 영속 `desktop-first`, `desktop-summary`, `mobile-first`, `mobile-summary`, `mobile-anchor`, `short-last-shortcut` 6개를 직접 열었다. 모바일 anchor의 양업체 label/button/badge가 명확히 분리되고 badge가 한 줄인 것을 확인했다. 순위81의 마지막 확인곡 바로가기와 원100행 복귀를 낮은 viewport에서도 확인할 수 있다. 화면의 TJ19/KY17/전체19·100분모·0/중복 안내와 기존 gradient/palette가 JSON 및 독립 계산과 일치했다.

QA JSON에서 실제 mobile100행의 번호 버튼181개는 최소94×44px, 저장 버튼100개는44×44px, 확인 badge44개는 최대높이16px 단일행이다. 세 viewport의 pageerror/console/failedRequest/adRequest 배열과 overflow는 모두 비어 있음/false다. title·본문·overlay·robots/canonical과 focus/Enter→stable anchor→Tab/Enter→기존 상세 경로가 기록되어 있다. 별도 [선택 대비 영수증](../operations/runs/resume-20261016-0400-ui-contrast.json)을 읽고 Python/Pillow로 영속 screenshot의 지정 pixel RGB와 기록을 실제 대조한 뒤 WCAG sRGB 식을 독립 계산했다. summary6.52:1·scope6.23:1·badge6.18:1/각기준4.5를 재확인했으며 선택3표본만의 결과다. 이미지 편집·새 브라우저 실행은 하지 않았다. 이 시각 QA는 앱 담당이 직접 실행한 근거를 본 담당이 읽고 확인한 것이며, 본 담당의 새 브라우저 실행·clean artifact/CI·원격 preview/운영 증거로 확대하지 않는다. WebKit/Firefox·실제 기기·보조기술/필드 성능은 미실행이다.

## 동료 평가

아래 두 평가는 16일08:10KST 이후 root의 실제 최종 로컬 게이트와 두 담당 소유 파일/안전 metadata/영속 화면을 읽어 작성했다. 두 담당 모두 할당된 성분별 수용 기준을 충족해 동일 점수를 주었고, 차이를 만들기 위해 결함이나 감점을 생성하지 않았다. root의 사전 동률 규칙·전체6개 평가 확인을 대신하지 않는다. 로컬 source base ce1b49f dirty artifact와 원격 clean CI·실제 preview/운영·복구·Release pending을 구분한다.

```json
{
  "reviews": [
    {
      "reviewer": "/root/resume04_quality",
      "candidate": "/root/resume04_evidence",
      "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 5 },
      "criterionReasons": {
        "correctness": "frozen15~18위의 정확한4canonicalID·staging번호와 실제8candidate/8decision을 별도 읽기 스크립트로 전수 대조해6confirm/2defer와 ledger append6개가 모두 일치했다. TJ의 samfree feature·역광OST 및 TJ/KY KICK BACK OP 전체 원문을 보존하고 SAM별칭/생략KY역광을 추정하지 않는다. 새4unit과 root314unit·양target28에서 원71/3317ID/URL·무관provider/unknown 및 보류KY2의 observed/null-evidence 보존을 확인했다.",
        "evidence": "원8candidate의 verification=unreviewed/HTTP200/정확request·finalURL/actualcheckedAt/bytes/hash와 별도 reviewer/reviewedAt/reason8개를 저장하고 confirm6의 전체 evidence를 ledger와 대조했다. root evidence-audit의 원71 deepEqual/order/bytesprefix·topmetadata·동결입력/helper/globalCSS 보존 결과와 자료 본문을 읽었다. 실제 raw미보존에 따른 재파싱/responsehash 독립재계산 불가와 권리·오디오·Google 미확인을 명시한다.",
        "value": "관측8번호 중 근거가 충분한6개만 정확한 업체의 확인값으로 보강해 안전 탐색을 전체28ID/52번호, 동결차트19ID/TJ19·KY17로 넓힌다. KY2를 성공 수치에 맞추어 확인하지 않고 관측으로 남겨 사용자가 별칭·부제 불확실성과 미수록을 혼동하지 않게 한다. 기존 번호·stableID·기존 상세/저장 흐름을 유지한다.",
        "maintainability": "변경하지 않은 safe metadata-only CLI/기존 ADR006 ledger를 재사용하고 소유 ledger끝6개만 append했다. 원71객체/순서/metadata/근거시각과 raw/staging/chart/identity/helper/globalCSS를 보존하며 후보/결정/검증/한계/인계가 JSON과 본문에 연결된다. 원문HTML·가사·시크릿을 남기지 않고 Git/package/공용파일/서버/CI/배포 소유권을 지켰다. 후속 판단 근거가 생기면 별도로 재검토할 수 있게 defer이유를 보존한다."
      },
      "reason": "할당된 실제공식조회·개별정체성/버전검토·근거있는append·불확실성/원본보존·안전영속기록을 수용한다. 현재 성분의 필수 수정 없음. 변경량·확인 성공 개수로 평가하지 않았다.",
      "evidence": [
        "jpoptoday/data/reviewed/provider-corrections.json",
        "jpoptoday/data/staging/legacy-catalog.json",
        "jpoptoday/data/chart-snapshot.json",
        "jpoptoday/contracts/provider-corrections.mjs",
        "jpoptoday/tests/reviewed-chart-cohort0400.test.mjs",
        "docs/data/provider-review-20261016-0400.md",
        "docs/data/provider-review-requests-20261016-0400.json",
        "docs/operations/runs/resume-20261016-0400-evidence-audit.json",
        "/tmp/jpop-0400-unit.txt",
        "/tmp/jpop-0400-preview-e2e-after-jsonfix.txt",
        "/tmp/jpop-0400-production-e2e.txt"
      ],
      "acceptance": "accepted_data_component; actualmetadata/별도편집/6confirm2defer·원71/3317/unknown/URL보존 수용. raw독립재파싱·오디오/권리/Google은 미확인. cleanCI·실제preview/운영·복구·Release는 root별도게이트/pending."
    },
    {
      "reviewer": "/root/resume04_quality",
      "candidate": "/root/resume04_app",
      "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 5 },
      "criterionReasons": {
        "correctness": "chart100의 canonicalID/rank join과 동결기간/fetchedAt/원래상세/SongRow를 보존하고 기존 confirmedNumbers 자격의 provider별Set·전체union으로 정적표시한다. 본인E2E는 UIhelper없이 servedcatalog/frozenchart에서 TJ19/KY17/union19·100분모와 모든100행·36바로가기의 순위/ID/제목/번호/anchor/기존URL·설명aria를 대조했다. 0미수록아님·관측배제/부제명의추정없음, 360JSoff/키보드·양targetrobots/noads와 최종100행bbox가 실제 preview/prod각28에서 통과했다.",
        "evidence": "앱final2 QA의 실제3viewport/checkedAt·dirty metadata/동일 입력hash를 읽고 영속JSON 및12PNG와 원본 bytes일치를 확인했다. 대표desktop2/mobile3/short1 PNG6개를 직접 열어 수치/분모/브랜드·마지막순위/행탐색·수정후badge분리를 확인했다. 모바일100행181버튼min94x44/저장100개44x44/badge44개높이16·console/ad/overflow0, 지정pixel 대비3표본6.52/6.23/6.18을 독립재계산했다. 초기navigation대기오류·자동PASS가 놓친실제겹침/원스크린샷과 수정후QA를 분리보존한다.",
        "value": "노래방업체를 정한 사용자가 보관차트에서 실제확인번호가 있는 곡만 원순위순으로 찾고 원행·기존상세근거로 이동할 수 있다. 전체100곡을 제거하거나 금영자체순위를 만들어내지 않으며 19+17을36곡으로 오해하지 않게 안내한다. 모바일JSoff·키보드에서도 마지막바로가기/기존상세를 사용할 수 있고 실제발견한badge겹침과 작은hit target을 고쳐번호와공식상태를 읽기쉽게 했다.",
        "maintainability": "chart.astro 한 소유소스에서 기존 trustedprojection·nativeanchor·SongRow를 재사용하고 새clientJS/의존성·공용CSS/계약/storage를 변경하지 않았다. fragment는순위대신stableID이며DOM계약/소유문서가 독립E2E와연결된다. 발견한모바일결함은chart-list에한정한scopedCSS로고쳐다른화면의공용레이아웃을건드리지않았고 root와freeze/서버종료/재빌드후증거를조율해artifact race를피했다. 실패/미실행과실제결과를인계했다."
      },
      "reason": "업체별수·union/분모·원100순위/날짜·정적탐색·브랜드/호환성·모바일실결함해소 수용 기준을 실제최종소스/독립회귀/화면에서 충족했다. 필수 수정 없음. 발견/보완된 이전결함을 숨기지 않으며 현재 결함으로 남았다고 만들지 않는다.",
      "evidence": [
        "jpoptoday/src/pages/chart.astro",
        "jpoptoday/src/components/SongRow.astro",
        "jpoptoday/contracts/confirmed-discovery.mjs",
        "jpoptoday/e2e/chart-confirmed-navigation.spec.ts",
        "docs/engineering/chart-confirmed-navigation-20261016-0400.md",
        "docs/operations/runs/resume-20261016-0400-ui-qa.json",
        "docs/operations/runs/resume-20261016-0400-ui-contrast.json",
        "docs/engineering/design/chart-confirmed-20261016-0400-desktop-first.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-desktop-summary.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-first.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-summary.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-anchor.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-short-last-shortcut.png",
        "/tmp/jpop-0400-preview-e2e-after-jsonfix.txt",
        "/tmp/jpop-0400-production-e2e.txt",
        "/tmp/jpop-0400-check-final.txt"
      ],
      "acceptance": "accepted_app_component; finaldirtyartifact SSR/독립count/100행bbox/360JSoff·keyboard·visualQA 수용. WebKit/Firefox/실제기기/보조기술·필드성능·사이트전체대비는미실행. cleanCI/실제preview·운영·복구·Release는root별도pending게이트."
    }
  ]
}
```
