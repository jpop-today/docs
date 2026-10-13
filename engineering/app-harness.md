# Astro 관측 번호 미리보기 하네스

상태: Issue#5 구현 중. 2026-10-13 KST. 소스 기준 SHA `33e782cb2a49e6df82da5f747d2ad5ca1cd3e01f`. [소스 Issue#5](https://github.com/jpop-today/jpoptoday/issues/5), [제품 스펙](../product/spec.md), [ADR-004](../adr/004-observed-preview-read-model.md)에 따른다.

이 문서는 계약 하네스의 [기존 기록](harness.md)을 대신하지 않는다. 별도 Astro 앱의 공개 read model·빌드 산출물·브라우저 검증 연결을 기록한다. 미리보기 구현·로컬 성공·GitHub CI 성공·Cloudflare 미리보기·운영 전환·AdSense 실제 심사는 서로 다른 단계다.

## 공개 산출물 계약

공개 카탈로그는 `version: 1`, `songs`를 가지며 곡마다 `id`, `titleOriginal`, `titleKo`, `artistOriginal`, `artistKo`, `tags`, `availability`, `origin`, `karaoke`만 허용한다. `karaoke`의 `tj`, `ky`는 `status`, `observedNumber`, `conflict`만 가진다. 번호가 있으면 `observed-unverified`와 원래 숫자 문자열을, 없으면 `unknown`과 `null`을 사용한다. 공유번호 경고는 선택된 관측 번호의 서로 다른 ID 수로 계산한다. 출처·refs·evidence·확인일·가사·이미지는 공개 read model에 넣지 않는다.

현재 기준은 staging의 3,317개 ID다. 현재 3,313개는 기본 검색 대상이며 과거 차트의 4개 보관 ID는 검색에서 제외하고 상세 URL·저장 복구를 위해 유지한다. 파일 경로는 UTF-8 원래 ID의 `dist/songs/<id>/index.html`, canonical URL은 `https://jpop.today/songs/<encodeURIComponent(id)>/`다. 모든 상세는 JavaScript 없이 원제·가수·관측 번호·확인 필요 안내를 읽을 수 있어야 한다. 보관 상세는 보관 상태를 표시한다.

`build-info.json`의 허용 필드는 `schemaVersion: 1`, `sourceSha`, `legacyRevision`, `catalogSha256`, `songCount`, `currentSongCount`, `dirty`다. 빌드일을 번호 확인일로 쓰지 않는다. `catalogSha256`는 배포할 `catalog.json` 실제 UTF-8 바이트 해시이며, `sourceSha`는 full lowercase commit SHA다. CI에서는 `GITHUB_SHA`와 일치하고 `dirty: false`여야 한다. 로컬 변경 중 빌드는 dirty로 구분한다.

## 구현된 산출물 검사

소스 `scripts/verify-dist.mjs`는 읽기 전용 검사다. `npm run verify:dist`는 완성한 `dist`를 사용하고, 직접 실행 시 `node scripts/verify-dist.mjs /absolute/path/to/dist`로 다른 산출물을 검사할 수 있다. 경로가 없거나 준비 파일이 빠지면 비정상 종료하며 임의 파일·metadata를 생성하지 않는다.

- 공개 카탈로그의 모든 필드·ID·문자열 번호·상태·태그·가용성·공유번호 경고를 내부 staging의 명시적 선택필드와 대조한다. 3,317개 원래 ID가 모두 하나씩 존재해야 한다.
- 모든 곡의 정적 상세 파일과 모든 HTML의 title·canonical·noindex를 검사한다. 상세 본문에는 원제·가수·관측 번호·확인 안내가 있고, 보관 상태가 드러나야 한다. canonical은 파일 경로와 URL 인코딩까지 일치해야 한다.
- `index.html`, `404.html`, `catalog.json`, `build-info.json`, `_headers`, `robots.txt`가 있어야 한다. catch-all `_headers`의 `X-Robots-Tag: noindex`와 전체 크롤러의 `Disallow: /`를 검사한다.
- 심볼릭 링크·비정규 파일·숨김 경로·비공개 디렉터리·소스맵·허용하지 않은 파일 형식/위치를 거부한다. 공개 JSON은 두 파일만 허용하며 HTML과 내부 `_astro`의 CSS/JS, 루트 `robots.txt`·Cloudflare 설정 파일만 추가로 허용한다.
- HTML 이미지 요소, 외부 CSS URL, staging/evidence/refs payload 표식, 가사·이미지 필드, 알려진 광고 코드 표식을 거부한다. HTML script source는 내부 `/_astro/*.js`만 허용한다.

실제 첫 빌드의 compatibility JS에는 reviewed alias resolver의 `evidence: alias.evidence` 반환 코드가 남았다. 이는 직렬화된 근거 레코드가 아닌 인자 property 접근이므로, JS 자산의 따옴표 없는 evidence/refs 키가 동일 이름 property를 읽는 경우에만 허용한다. JSON과 HTML의 근거 키, JS의 quoted 키·literal/object/array payload, staging/raw 표식은 계속 거부한다. 실제 초기 bundle에는 refs 키나 staging 본문이 없음을 확인했다.

검사는 구조·표식·실제 바이트 대조에 기반한다. 임의 본문이 가사인지, 내용의 권리·공식 번호 진위가 무엇인지 의미 판단을 보장하지 않는다. 이미지·광고를 새로 도입할 때에는 별도 근거 검토와 artifact 계약 변경이 필요하다. 이 gate는 제공사 확인 완료·검색 색인 허용·AdSense 준비 완료를 선언하지 않는다.

## CI와 브라우저 실행 계약

추가한 `.github/workflows/app.yml`은 `main`, `codex/**` push, PR, 수동 실행에서 Node.js24와 lockfile을 사용한다. 권한은 `contents: read`다. 다음 순서로 실행하며 선행 단계가 실패하면 뒤 단계와 artifact 업로드를 실행하지 않는다.

```bash
npm ci
npm test
npm run check
npm run build
npm run verify:dist
npx playwright install --with-deps chromium
npm run test:e2e
```

E2E가 성공하면 `dist/`만 `jpoptoday-dist-<github.sha>`로 14일 보관한다. CI는 Cloudflare 배포를 수행하지 않는다. 배포는 동일 SHA artifact와 [배포 기록](../operations/deployment.md)의 공개 경계·대표 경로·rollback 확인에 연결해야 한다.

`npm test`는 기존 계약/CLI 테스트와 앱 read model 테스트를 수행하고, Playwright는 별도 `npm run test:e2e`로 실행한다. 로컬 Chromium이 이미 있을 때는 `PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH`로 실행 파일을 지정할 수 있다. CI는 변수를 지정하지 않고 Playwright가 설치한 Chromium을 사용한다.

브라우저 검증 대상은 한글·일본어·가수·번호·결합 검색, URL 복원/뒤로가기, 관측번호 복사·unknown 복사 없음, 충돌 안내, archived 검색 제외/상세 유지, 기존 `/song?id=`·필터 호환, 즐겨찾기 저장/해제와 기존 storage 읽기·쓰기 실패 보존, 모바일/키보드 흐름, 콘솔 오류, 실제 404다. 실제 테스트 구현과 통과 개수는 실행 후 기록해야 한다.

## 실행 상태와 남은 게이트

Astro 앱·E2E·CI 통합은 진행 중이다. workflow 파일 작성은 GitHub Actions 실행 성공을 뜻하지 않는다. 정확한 시각·검증 SHA·전체 단계 결과는 통합 담당자가 CI 확인 후 이 항목 또는 Issue/PR에 기록한다.

하네스 자체의 로컬 검증(2026-10-13 KST, Node.js24.19.0)은 `node --check scripts/verify-dist.mjs` 성공, 없는 dist 경로의 실패 종료, 실제 staging의 3,317 ID를 사용한 임시 합성 산출물의 성공(3,319 HTML)을 확인했다. 같은 임시 산출물에서 metadata 누락·비공개 data 디렉터리·심볼릭 링크·noindex 누락·잘못된 canonical·외부 script·공개 카탈로그 extra field·미검토 TXT 파일을 각각 거부했다. 합성 본문과 metadata는 실제 앱 빌드·운영 데이터 검증이 아니며 임시 산출물은 검사 후 삭제했다.

첫 실제 Astro 빌드에 `npm run verify:dist`를 실행하여 성공했다(2026-10-13 KST, Node.js24.19.0). 3,317개 정적 곡 상세·현재3,313/보관4·3,333개 noindex HTML·3,340개 파일과 공개 경계를 확인했다. metadata sourceSha는 기준 commit `33e782cb2a49e6df82da5f747d2ad5ca1cd3e01f`, `dirty: true`이므로 이는 커밋 전 로컬 앱 빌드 결과다. 확정 commit의 CI나 Cloudflare 미리보기 성공으로 기록하지 않는다. 최초 검사의 resolver evidence property 오탐은 위 제한적 런타임 property 구분으로 수정했다.

수정한 경계의 회귀 확인은 실제 dist의 임시 복사본 JS에 quoted evidence·literal evidence·literal refs·member 뒤 문자열 결합을 각각 주입하여 모두 거부했다. 원래 dist는 수정하지 않았고 임시 복사본은 삭제했다. 수정 후 원래 실제 dist의 `npm run verify:dist`도 성공했다.

확정 소스 `9cd5017883da50a5aabd702eb82d2ccb2e684ae1`을 clean 상태로 재빌드해 verify:dist가 같은 SHA·dirty:false를 확인했다. Node24 계약122·Astro7 check0error/0warning·build3333 HTML·Chromium8 E2E를 로컬에서 확인했다. 1440×1100 desktop/360×800 mobile, URL/뒤로가기·기존 주소·기존 문자열/객체/미해결 즐겨찾기·손상 localStorage 원본 보존·같은 탭 sessionStorage 복구·clipboard 거부 대체·JS 없는 본문·404를 검사했다. Browser plugin이 없어 시스템 Chromium154/Playwright fallback을 썼다. [콘셉트/실제 대조 기록](design/search-ui.md)은 기능 QA와 분리했다.

소스 [PR#7](https://github.com/jpop-today/jpoptoday/pull/7), Node24 [push CI](https://github.com/jpop-today/jpoptoday/actions/runs/38351080227)는 success를 확인했다. Node22 계약 CI도 success다. PR/main CI와 별도 Cloudflare 미리보기의 실제 HTTP/noindex/비공개 경계/복구 결과는 [실행 기록](../operations/status.md)으로 연결한다. WebKit·필드 성능·실제 광고·전체 카탈로그 공식 확인은 미실행이며 기존 운영 전환과 구분한다.

## 실제 배포 회귀와 후속 수정

첫 배포 source9cd의 GitHub 정상 HTTPS remote CI는7통과/1실패였다. search의 초기 목록을 내려받기 전에 번호 필터를 누르면 listener가 없어 조건이 적용되지 않는 준비 경쟁을 재현했다. sourceabef7b01637c9180998a221248868929cfabe5f0에서 provider/save/more/copy-list를 준비 후 활성화하고750ms 지연 catalog의 상태/URL 회귀를 추가했다. 최신 로컬9테스트와 same-SHA push appCI38351736867이 통과했다. 실제 새 배포URL https://b88c2b94.jpoptoday-preview.pages.dev (deployment b88c2b94-35a1-455c-bdc7-8f9a6dde97b8)와 source를 고정해 deployed-preview workflow로 다시 검사한다.

`deployed-preview.yml`은 workflow_dispatch의 deployment_url/source_sha와 기록된 기본 preview를 사용한다. 전용 jpoptoday-preview HTTPS host·fullSHA·실제 build-info 일치를 확인하고 그 source를 checkout해 Chromium9 흐름을 실행한다. GitHub runner의 정상 HTTPS/CA를 사용하며 로컬의 영구 NSS 변경이나 TLS 검증 해제를 요구하지 않는다. 실제 성공 run과 복구는 릴리스 기록이 기준이다.

최종 읽기 전용 리뷰에서 정상 localStorage 읽기/QuotaExceededError 쓰기만 실패하는 조건의 session 복원이 누락됨을 확인했다. raw가 실패 당시 originalRaw와 일치하는 version1 임시 envelope를 읽고, 정상 읽기라도 이전 실패의 목록을 같은 탭에 복원하도록 수정했다. 다른 raw로 변경된 영구 snapshot에 stale 임시 목록을 덮지 않는다. 정상 raw/객체 metadata 보존·저장 실패·페이지 이동·두 곡 유지·임시 안내의 회귀를 추가해 최신 로컬 Chromium10테스트가 통과했다.
