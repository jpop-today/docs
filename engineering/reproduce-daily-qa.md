# 일일 QA 재현과 다음 실행 인계

2026-10-14 · [Issue #9](https://github.com/jpop-today/jpoptoday/issues/9) · REPORT-001, TEAM-001, RELEASE-001. 기준은 [일일 계약](../operations/daily-development.md), [당시 보고 감사](../operations/report-artifact-audit-20261014.md), [당시 UI QA](design/preview-20261014-qa.md)다.

## 영속 소스와 실행 상태

원격 재현 기준은 소스 SHA [`d040daa0627d32e35f5a9238be174f8a483617d9`](https://github.com/jpop-today/jpoptoday/tree/d040daa0627d32e35f5a9238be174f8a483617d9), 버전/태그 `v0.2.1`이다. 이 문서 담당자는 로컬 HEAD·package 버전과 `git rev-parse 'v0.2.1^{commit}'`의 같은 SHA를 직접 확인했다. 원격 태그 게시·릴리스·CI·배포 완료는 root의 [2026-10-14 완료 기록](../operations/daily/2026-10-14.md)에서 실제 run과 SHA를 확인한다. 작성 시작 시 root 인계는 unit 148·check/build/artifact·로컬 browser 10·main CI PASS, 새 preview HTTP 13 PASS이며 원격 browser workflow와 태그 push는 진행 중이었다. 이 담당자가 그 실행을 반복하거나 원격 완료를 직접 확인한 결과는 아니다.

아래는 **재실행 준비 명령**이다. 이번 문서 작업에서 설치·테스트·빌드·캡처·dispatch·라이브 송신은 실행하지 않았다. 2026-10-14 00:21 UTC(09:21 KST)에 명령 정의, 상대 파일 링크 7개 존재, 고정 SHA의 소스 경로 7개(`git cat-file -e`), shell 블록 4개 구문(`bash -n`), E2E 정의 10개를 읽기 전용으로 확인했다. 공유 checkout의 Git 변경과 통합은 root가 맡는다. 독립 재현 담당자는 Node **24.5 이상**(앱은 24+, 송신 transport는 24.5+)과 Git·npm, 로컬 Chromium 또는 Playwright Chromium을 준비한다.

```sh
JPOP_QA_SOURCE_SHA=d040daa0627d32e35f5a9238be174f8a483617d9
JPOP_QA_CHECKOUT="$(mktemp -d /tmp/jpoptoday-daily-qa.XXXXXX)"
git clone https://github.com/jpop-today/jpoptoday.git "$JPOP_QA_CHECKOUT"
cd "$JPOP_QA_CHECKOUT"
git checkout --detach "$JPOP_QA_SOURCE_SHA"
test "$(git rev-parse HEAD)" = "$JPOP_QA_SOURCE_SHA"
node --version
npm ci
```

`main` 최신 코드로 고정 SHA를 대신하지 않는다. 태그를 기준으로 사용한다면 원격에서 가져온 `v0.2.1^{commit}`이 위 SHA인지 먼저 확인한다. `/tmp`와 켜 둔 서버·미커밋 변경은 다음 날 남아 있다고 가정하지 않는다.

## 보고·평가·송신 회귀

```sh
node --test tests/report*.test.mjs tests/discord-notify.test.mjs
```

| 영속 검사 | 재현 범위 |
| --- | --- |
| [report-cli.test.mjs](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/tests/report-cli.test.mjs) | P2 AUDIT-01 원래 입력(schemaVersion 99/timezone UTC)과 `null`·배열 전일 파일을 실제 `morning` CLI에 넣는다. exit 1, 새 출력 없음, 합성 완료 문구 미노출, 입력 바이트 보존을 확인한다. 임시 입력은 테스트가 생성·정리한다. |
| [report.test.mjs](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/tests/report.test.mjs) | KST 날짜·연도 전날·불가능한 날짜, 정상 전일 완료와 누락 fallback, 잘못된 날짜/스키마/시간대/본문/배열, CI SHA, 3명×2리뷰 합계와 자기·중복·누락·빈 근거 배열·범위 밖 점수 거부를 확인한다. 함수 테스트 범위이며 모든 CLI 실패 조합을 대신하지 않는다. |
| [discord-notify.test.mjs](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/tests/discord-notify.test.mjs) | 주입 mock으로 목적지 검증·비밀 제거·멘션 억제·200/204·429 제한·timeout/5xx 중복 재송신 방지·proxy/CA 유지·missing-secret skip을 확인한다. 네트워크 요청은 없으며 mock `sent`는 라이브 전송 증거가 아니다. |

구현은 [build-report.mjs](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/scripts/build-report.mjs)다. CLI `morning/completed`와 JSON `plan/complete`를 구분한다. `evaluateTeam`은 리뷰 형태·합계만 검증한다. 실제 파일을 읽었는지, 근거와 점수가 맞는지, 항목별/사전 순서 동률 처리는 root가 실제 리뷰 6개와 일일 계약으로 판단한다. 합성 테스트를 실제 평가로 복사하지 않는다.

당시 `/tmp` 감사 24개·수정 후 39개는 독립 실행 기록이다. 위 영속 테스트는 중요한 회귀를 보존하지만 그 감사의 전체 입력 조합·24/39개를 그대로 재현한다고 주장하지 않는다. Secret 값·전체 env를 출력하지 않고, 재현을 위해 `npm run notify:discord`나 실제 webhook을 호출하지 않는다.

## 로컬 앱과 캡처

```sh
ASTRO_TELEMETRY_DISABLED=1 npm run check
ASTRO_TELEMETRY_DISABLED=1 npm run build
ASTRO_TELEMETRY_DISABLED=1 npm run verify:dist
JPOP_SCREENSHOT_DIR="$(mktemp -d /tmp/jpop-daily-screenshots.XXXXXX)"
CI=1 PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium \
  JPOP_SCREENSHOT_DIR="$JPOP_SCREENSHOT_DIR" npm run test:e2e
```

시스템 Chromium 경로가 다르면 실제 실행 파일을 지정한다. 시스템 브라우저가 없으면 `npx playwright install chromium` 후 위 명령의 `PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH` 지정 없이 실행한다. 로컬 실행은 `JPOP_TEST_BASE_URL`을 설정하지 않은 상태에서 기존 4321 서버를 종료하고 수행한다. [Playwright 설정](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/playwright.config.ts)이 저장소 서버를 시작한다. 새 빌드의 `build-info.json`은 같은 sourceSha·dirty:false여야 한다.

기존 [E2E 10개](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/e2e/app.spec.ts)는 지연 초기화, 검색/필터/URL 복원/뒤로, 기존 URL·ID, 문자열/객체/미해결 저장값, 손상 storage·쓰기 거부 보존, clipboard fallback, JS 없는 상세·보관 곡·가이드, 360px 키보드 검색/넘침, 비공개 파일 404·noindex·광고 제외를 확인한다.

`$JPOP_SCREENSHOT_DIR/desktop.png`는 1440×1100 최초 상태, `mobile.png`는 360×800 한글 검색 상태의 full-page 캡처다. 실제 이미지를 열어 색·본문·넘침·focus를 검토하고 실행 환경/URL/SHA/상태를 함께 기록한다. 캡처 생성 자체가 시각적 합격 판정은 아니다. 당시 [desktop](design/screenshots-20261014/desktop-initial.png)·[mobile 검색/저장](design/screenshots-20261014/mobile-search-saved.png)·[복사 dialog](design/screenshots-20261014/mobile-copy-fallback.png)는 보존된 독립 캡처이며 기존 E2E의 두 이미지와 상태가 다르다. 폰트·Chromium·viewport·focus·필터·저장 상태를 맞추기 전 픽셀 동일성을 판단하지 않는다. 당시 팔레트/대비/픽셀 비교와 HTTP 20개 전체는 위 E2E에 포함된 것으로 표시하지 않는다.

## 원격 미리보기와 다음 날

실제 원격 주요 동작은 root가 신뢰하는 `main`의 [deployed-preview workflow](https://github.com/jpop-today/jpoptoday/blob/d040daa0627d32e35f5a9238be174f8a483617d9/.github/workflows/deployed-preview.yml)를 아래처럼 명시 입력으로 실행한다. workflow는 고정 소스를 checkout하고 배포 `build-info.json`의 sourceSha·dirty:false를 확인한 뒤 같은 E2E 10개를 실행한다.

```sh
JPOP_TEST_BASE_URL=https://12a43fb9.jpoptoday-preview.pages.dev
EXPECTED_SOURCE_SHA=d040daa0627d32e35f5a9238be174f8a483617d9
gh workflow run deployed-preview.yml --repo jpop-today/jpoptoday --ref main \
  -f deployment_url="$JPOP_TEST_BASE_URL" -f source_sha="$EXPECTED_SOURCE_SHA"
gh run list --repo jpop-today/jpoptoday --workflow deployed-preview.yml \
  --event workflow_dispatch --limit 5
```

dispatch 접수와 PASS를 구분하고 해당 run URL·실제 conclusion·checkout SHA를 남긴다. `EXPECTED_SOURCE_SHA`는 workflow의 identity 검사용이며 로컬 `npm run test:e2e` 단독 실행은 이 값을 검사하지 않는다. proxy/CA를 유지하고 TLS 검증을 우회하지 않는다. 원격 E2E·preview HTTP 성공을 운영 `jpop.today` 전환·rollback·AdSense 승인으로 해석하지 않는다.

다음 날짜 root는 Issue #9의 최신 활성 run, 완료 JSON/대응 일일 문서, 실제 리뷰 6개·교체 이력과 최신 main/태그/CI/배포 SHA를 먼저 확인한다. 이 문서의 고정 SHA는 2026-10-14 회귀 재현 기준이며 다음 날의 최신 기준선을 대체하지 않는다. 재실행 시 명령·시각·환경·SHA·PASS/FAIL/not_run·로그/캡처 위치를 새 완료 기록에 남기고 기존 독립 감사·평가·제외 이력은 보존한다.
