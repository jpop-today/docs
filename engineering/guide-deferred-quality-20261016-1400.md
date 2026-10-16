# 판단 보류 가이드 품질 계약과 검증

2026-10-16 KST · `resume-20261016-1400` · [Issue30](https://github.com/jpop-today/jpoptoday/issues/30).

담당 `/root/resume14_quality`, 통합·Git·package·빌드·서버·실제 E2E·배포 담당 `/root`. 스펙은 CONTENT-001, DATA-001, SONG-001, SEO-001, DESIGN-001, HARNESS-001과 [Issue30 계약](../product/spec.md#판단-보류-사례와-곡-상세-연결--issue30-2026-10-16), ADR-004/006/007이다. 소스와 docs 브랜치는 `codex/30-deferred-guide-20261016`이며 기준 SHA는 각각 `312c6b687c504cf82090ad26bdd9d829b99a105f`, `4b19570a1dbd4698777aaf461d77999c8ad5bbc6`, 조직 규칙 기준은 `b7aad18fe2f793453b6505bb150f359d05ae3554`다. 직접 `rev-parse`와 브랜치를 읽어 확인했다. 결과 commit·PR·배포 SHA는 root의 실제 실행 뒤 연결한다.

소유 파일은 [기존 표기 비교 E2E](../../jpoptoday/e2e/title-artist-guide.spec.ts), [신규 판단 보류 E2E](../../jpoptoday/e2e/deferred-guide.spec.ts), 이 문서 세 파일이다. 소스·데이터·원래 테스트·공용 설정과 다른 담당의 diff를 수정하지 않았다. 추가 하위 에이전트를 생성하지 않았다.

## 사용자 동선과 독립 기대값

검증 동선은 `/guides/` → 기존 표기 비교 가이드 → 두 사례의 기존 안정 ID 상세 → TJ 공식 근거와 KY 관측 상태 비교 → 곡 주소를 포함한 제보 링크 → 브라우저 뒤로가기 → 문의와 정보 정정 → 가이드 목록이다. 두 모바일 검증은 360×800, JavaScript off, 실제 Tab·Enter 이동으로 이 동선을 실행한다. mailto는 제목·본문·주소를 읽고 검증하며 실제 메일을 보내지 않는다. 공식 외부 링크는 href·target·rel을 확인하며 조회를 새로 수행하지 않는다.

기대값은 [실제 04시 요청·판단 기록](../data/provider-review-requests-20261016-0400.json)을 읽어 테스트 안에 별도로 정의했다. 테스트는 가이드 데이터나 UI 집계 함수를 가져오지 않는다. 타입 전용 `Song` import는 런타임 가이드 구현을 불러오지 않는다. URL 보안 규칙만 실제 ledger와 동일한 `contracts/provider-corrections.mjs`의 `officialLookupMatches`를 공유해 www KY·port·query의 서로 다른 oracle이 재발하지 않게 했다.

| 안정 ID | TJ 공식 확인 | KY 판단 보류 | 실제 조회 시각 UTC |
| --- | --- | --- | --- |
| `jpop-playlist-samfree-ルカルカ-ナイトフィ-ハ-f6722acd10` | 문자열 `27031`; `ルカルカ★ ナイトフィーバー` / `samfree(Feat.巡音ルカ)` | 문자열 `43185`; `ルカルカ★ナイトフィーバー` / `SAM feat.巡音ルカ`; 명의 동일성 미확보 | TJ `2026-10-15T19:06:49.340877Z`; KY `2026-10-15T19:06:53.142654Z` |
| `jpop-playlist-ado-逆光-c8c1ba8b20` | 문자열 `68868`; `逆光(映画 'ONE PIECE FILM RED' OST)` / `Ado` | 문자열 `76509`; `逆光(ウタ from ONE PIECE FILM RED)..` / `Ado`; 생략된 제목·버전 미확보 | TJ `2026-10-15T19:06:53.828995Z`; KY `2026-10-15T19:06:56.561637Z` |

UTC 네 시각은 10월 16일 한국 시간 04시 06분의 과거 조회다. 이번 가이드 본문 확인일 10월 16일과 새 번호 조회를 구분한다. 이 두 KY lookup 성공을 confirm이나 suppress로 바꾸지 않는다.

공개 `/catalog.json`의 3317개 고유 ID와 두 정확한 ID·원문·current/playlist를 확인하고, 두 TJ는 실제 ledger77의 confirm·공식 URL·제목·가수·checkedAt와 대조한다. 두 KY는 ledger 항목 없음, `observed-unverified`, `evidence: null`, `correction: null`, 기존 번호 문자열 보존을 요구한다. `/build-info.json`의 ledger digest도 읽은 ledger77과 대조한다. 이 검증은 UI 구현의 기대값을 다시 계산하지 않는다.

## 영속 하네스 변경

기존 `title-artist-guide.spec.ts`는 본문 확인일 `2026-10-15` → `2026-10-16`, 공식 자료 개수 4 → 8만 수정했다. SPYAIR·JANE DOE의 기존 네 실제 URL/checkedAt, 이전 다섯 가이드의 10월 14일 날짜, 제목·canonical·robots/sitemap·JS-off·문의 동선과 오류/광고 검증은 유지했다.

신규 `deferred-guide.spec.ts`는 세 실제 브라우저 케이스를 추가한다.

1. Desktop SSR 응답에서 두 기존 상세 링크를 확인하고, 사례별 네 원문/번호/공식 자료 URL·실제 시각, 공개 카탈로그와 ledger의 서로 다른 상태, 신규 링크의 최소 44×44px를 검사한다.
2. samfree 사례에서 360px·JS-off·Tab/Enter로 가이드 목록부터 정확한 상세에 도달한다. TJ만 공식 결과와 조회 시각을 표시하고, KY는 관측 레이블과 번호를 유지하며 공식 근거 링크를 만들지 않아야 한다.
3. Ado 사례에서 같은 동선을 별도로 실행해, 다른 가수의 동일 제목이나 생략된 KY 제목을 완전한 TJ 버전으로 합치지 않도록 한다.

두 모바일 케이스는 기존 파랑→분홍 브랜드·분홍/민트/파랑 배경, viewport 가로 넘침, 프레임워크 overlay·빈 오류 본문·console error/warning·pageerror·광고 요소/요청 없음도 검사한다. 제보 주소의 subject/body에 정확한 원제·가수·기존 절대 곡 URL이 들어 있어야 한다. 이 기능은 새 가이드나 곡 ID·URL·저장 키를 만들지 않는다.

## 직접 수행한 확인

| 시각·실행 주체 | 명령·읽은 자료 | 실제 결과와 한계 |
| --- | --- | --- |
| 2026-10-16, quality | 세 AGENTS, spec Issue30, status/roadmap/daily-development/continuous-development, 협업·하네스, 실제 요청 JSON, ledger77, 기존 E2E/URL 계약 읽기 | 기준과 범위 확인. 운영 HTTP나 실제 E2E를 실행한 증거가 아니다. |
| 2026-10-16, quality | `cd /workspace/jpoptoday && ./node_modules/.bin/playwright test --list` | exit 0, **31 tests / 8 files** 수집. 기존28 + 신규3이며 브라우저를 실행하지 않았다. |
| 2026-10-16, quality | `git -C jpoptoday diff --check` 및 소유 파일 diff/status 읽기 | exit 0, 기존 E2E diff 두 줄 확인. 다른 담당의 소스·문서 변경을 보존했다. |
| 2026-10-16 14:04:07 KST, quality | Node/branch/SHA 읽기와 `date -u` | Node `v24.19.0`, 위 두 브랜치·기준 SHA 확인. |

클라우드 스킬의 `environment_status`는 current/running, revision332, network enforced를 직접 반환했다. credential의 값은 읽거나 출력하지 않았다. 프런트엔드 테스트 스킬에서 Browser plugin이 카탈로그에 보이는 것을 확인했으나, 이 작업의 명시적 **기존 Playwright/Chromium만 사용·root만 실제 실행** 범위를 우선했다. Browser·다른 브라우저·다운로드·설치·네트워크 우회를 사용하지 않았다. 검증 명령을 수집했다는 사실과 렌더링/배포 성공을 구분한다.

## root 실행 요청과 남은 증거

소스 초안이 동기화된 뒤 root에게 다음 순서로 실행을 요청했다. 서버 시작·빌드·dist 변경은 quality가 수행하지 않는다. 기존 Chromium 실행 경로와 Node24를 유지한다.

```sh
cd /workspace/jpoptoday
npm run test:e2e -- e2e/title-artist-guide.spec.ts e2e/deferred-guide.spec.ts
npm run test:e2e
```

첫 명령은 5개, 전체 명령은 31개가 수집된다. 기존 단위314·타입 검사·두 target의 build/verify:dist·동결 재현·마지막 정상 자료·3317 ID/URL/storage 불변은 root가 기존 실행 계약으로 검사한다. 동일 commit의 CI·원격 preview/production·실제 운영 본문·복구/재적용은 별도의 실제 증거가 필요하다. `JPOP_SCREENSHOT_DIR` 지정 시 기존 두 장과 신규 `guide-deferred-desktop.png`, `guide-deferred-samfree-mobile.png`, `guide-deferred-samfree-detail-mobile.png`, `guide-deferred-ado-mobile.png`, `guide-deferred-ado-detail-mobile.png`를 생성할 수 있다.

quality가 직접 실행한 것은 수집과 읽기 전용 확인이다. 아래 root 실행 로그와 캡처는 직접 읽어 검토했으며, 실행 주체를 quality로 바꾸지 않는다. 시작 시 운영 eb916/M48c의 첫 HTTP transport failure도 성공으로 바꾸지 않으며 첫 실패와 후속 결과를 각각 보존한다. 동료 평가는 요청 후 실제 산출물과 증거를 읽고 수행하며 이 기록에는 점수를 만들지 않았다.

### root 실행 증거

2026-10-16 14:13 KST 후속 검토: root가 전달한 실제 로그·`dist/build-info.json`·아래 캡처를 quality가 직접 읽었다. 대상은 **base source `312c6b687c504cf82090ad26bdd9d829b99a105f` 위 dirty local v0.4.4 후보의 preview**다. 최종 commit·clean CI·배포 증거로 해석하지 않는다.

| 실행 주체·명령 | 실제 결과 | 직접 읽은 로그 |
| --- | --- | --- |
| root, `npm test` | 314 pass, fail0, skipped0, `8449.292416ms` | `/tmp/jpop-1400-unit.txt` |
| root, 최종 `npm run check` | Astro 26 files, errors0/warnings0/hints0 | `/tmp/jpop-1400-check-final.txt` |
| root, 최종 preview `npm run build` | 3336 pages, preview sitemap0 | `/tmp/jpop-1400-preview-final-build.txt` |
| root, 최종 preview `npm run verify:dist` | 3317곡·3313 current/4 archived, 3336 HTML·0 indexable, 3347 files | `/tmp/jpop-1400-preview-final-dist.txt` |
| root, 최종 preview `npm run test:e2e` | **31 passed (1.1m)**; 신규3과 기존 표기 비교2가 전체 회귀 안에서 각각 통과 | `/tmp/jpop-1400-preview-e2e.txt` |

읽은 최종 E2E 로그 SHA-256은 `c122962406f6182ed6b4577f7ff1f0091912fb1eac91f3e31c44ebb1b2913c0b`다. 초기 local step·artifact 메타데이터는 [root local 기록](../operations/runs/resume-20261016-1400-local.json)에 있으며, 개인정보 문구 수정 뒤 최종 재빌드·검증은 위 `*-final-*` 로그와 전체 E2E를 기준으로 한다. `/tmp` 원본은 이번 세션의 직접 증거이며 영구 보존·clean CI 기록은 root가 별도로 연결한다.

quality가 확인한 캡처는 `/tmp/jpop-1400-e2e-preview/guide-deferred-desktop.png`와 같은 폴더의 `guide-deferred-{samfree,ado}-{mobile,detail-mobile}.png` 다섯 장이다. desktop에서는 7·8번 사례와 두 상세 링크를, 모바일 가이드에서는 읽을 수 있는 줄바꿈·명확한 링크 포커스와 이웃 내용 간 간격을, 모바일 상세에서는 기존 두 번호 열 안의 TJ 공식 확인과 KY 관측 레이블·번호를 확인했다. 이 캡처만으로 화면 아래 공식 자료 전체나 제보 동선 완료를 주장하지 않으며, 해당 동작·실제 시각·href·44px·overflow·오류·광고0은 통과한 E2E assertion이 검증한다.

#### 발견한 운영 분석 안내와 범위 보완

root가 추가한 [Issue30 범위 보완](../product/spec.md#issue30-범위-보완--운영-분석-안내-불일치)과 `src/pages/privacy.astro`를 직접 읽었다. 기존의 분석 스크립트가 없다는 안내는 실제 일반 운영 HTTP의 Cloudflare Insights script 관측과 맞지 않았다. 현재 root 소스는 Cloudflare Web Analytics의 방문·성능 통계가 포함된다고 안내하고 공식 데이터 수집·통계 항목 문서에 연결한다. Google 광고 요청 코드 없음은 별도로 설명하며, 모든 처리에서 쿠키·개인정보를 사용하지 않는다는 포괄 보장이나 법률 준수·심사 승인으로 넓히지 않는다.

[설정 presence 기록](../operations/runs/resume-20261016-1400-analytics-setting-presence.json)의 프로젝트 필드 미설정은 모든 주입 계층이나 실제 payload의 부재를 뜻하지 않는다. source 수정·최종 preview 타입/빌드/E2E 통과까지 확인했으며 실제 운영 안내가 바뀌었다고 쓰지 않는다. 개인정보 문구의 수집 범위·실제 payload·동의 필요성은 별도 후속이다. 실제 ads.txt의 HTTP200·판매자 형식 한 행도 [비밀 없는 관측](../operations/runs/resume-20261016-1400-adstxt-observed.json)에 한정하며 계정 일치·소유권·Google 승인 증거로 해석하지 않는다.

[첫 HTTP 실패 원본](../operations/runs/resume-20261016-1400-baseline-first-http.json)의 `TRANSPORT_OR_PARSE_ERROR`/passed:false와 [후속 strict verifier 기록](../operations/runs/resume-20261016-1400-baseline-http-retry.json)의 old M48c·27 checks/1 attempt passed:true를 모두 직접 읽었다. 후속 통과가 첫 실패 원인을 확정하거나 당시 성공을 뜻하지 않는다. [root 실패 기록](../operations/runs/resume-20261016-1400-local-failures.json)에 socket EPERM·worker git EPERM·urllib403과 정상 경로의 후속 관측·원인 미확정을 함께 남겼다.

14:13 KST 당시 수용 상태는 local preview 하네스·최종 빌드·전체31 E2E와 캡처 검토까지였다. 당시 local production·clean commit CI·두 target 재현·동일 SHA 원격 preview/production·운영 반영·복구/재적용은 이 담당에게 실행 증거가 인계되지 않았다. 소스 두 E2E 파일은 freeze를 유지했다. 이후 production과 네 viewport QA·peer 검토 결과는 아래에 추가한다.

## local production·네 viewport QA 후속 수용

2026-10-16 14:17 KST에 root의 `/tmp/jpop-1400-production-e2e.txt` 전체와 `/tmp/jpop-1400-production-dist.txt`, 고정 `/tmp/jpop-v044-production-local-final/build-info.json`을 직접 읽었다. **production target 로컬 전체31 passed (57.7s)**, 3317곡·3313 current/4 archived·3336 HTML·45 indexable·3347files다. 대상은 여전히 base `312c6b687c504cf82090ad26bdd9d829b99a105f` 위 `dirty:true` 후보이며, catalog/ledger/chart digest는 preview와 같다. E2E 로그 SHA-256은 `82428d6cd9478672c2781ba17111df40f65ef232e09112df6305a5ffb1a27d5b`다. root 실행·quality 로그 읽기 수용이며 clean CI나 운영 배포는 아니다.

[앱 QA JSON](../operations/runs/resume-20261016-1400-ui-qa.json)의 네 결과를 읽었다. 1440×1100/768×1024는 JS-on, 360×800/360×320은 JS-off이며 모두 PASS, 오류/경고/광고 배열0, 가이드·두 상세의 scrollWidth가 viewport와 같다. 신규 링크는 desktop 800×54.296875, tablet 704×54.296875, 모바일 samfree 328×79/Ado 328×50.5다. 모든 환경에서 Tab로 도달한 링크의 3px outline과 화면 안 위치, 두 공식 검색 링크·공식 대조·제보의 정확한 href를 기록했다. 짧은 320px 화면에서도 두 신규 링크와 각 다음 동작이 viewport 안에 들어온다.

[영속 캡처 목록](../operations/runs/resume-20261016-1400-ui-artifacts.json)의 12개 실제 `engineering/design/deferred-guide-20261016-1400-{1440x1100,768x1024,360x800,360x320}-{top,samfree-link,ado-link}.png`를 모두 열어 제목/검토일·브랜드 그라데이션·읽기 폭·줄바꿈·링크 포커스·인접 내용 간격을 확인했다. 12개 파일의 SHA-256을 직접 계산해 manifest와 12/12 일치함을 확인했다. 화면 경계 밖 다음 문단은 스크롤 대상이며 요소의 수평 넘침·겹침으로 보이지 않았다. 이 검토는 캡처·실행기록 검토이며 quality의 새 브라우저 실행이 아니다.

같은 후속에서 source base의 `src/content/guides.ts`를 읽기 전용 `git show`로 받아 현재 객체와 독립 비교했다. `2026-10-16T05:17:20.965Z`에 다른5글 전체 객체, 기존4출처 전체, 기존1·2·3·4·6절과5절둘째문단의 deepEqual, 총6글/8출처·추가4 source의 실제 후보 원문/URL/checkedAt·2상세URL을 모두 PASS했다. ledger bytes hash도 기존 `a2d8ea8e9eba737a38a1f2968a0a1687498bf04fb0f8742ea7f42eedaef8f708`과 같았다. 이는 source·문서·기존 실제 요청의 독립 읽기 대조이며 새 provider 요청을 수행하지 않았다.

두 target의 local 빌드·verify/31 E2E와 앱 네 환경 QA는 할당된 로컬 수용 범위에서 충족한다. **clean commit CI·두 target 반복 재현·동일 SHA 원격 preview/production·운영 반영·복구/재적용·Release는 아직 별도 root 실행 증거가 필요하다.** 계정·권리·문의 실수신·실제 분석 payload·Google 심사도 unknown을 유지한다.

## 실제 동료 평가 2개

root 요청 뒤 evidence의 두 문서·이전 실제 요청/ledger77·root 정책/analytics 영수증, app의 두 source·작업 문서·독립 QA JSON·영속12캡처와 두 target root E2E 로그를 읽어 작성했다. 아래는 자기 평가가 아니며 할당된 수용 기준에 대한 평가다. 두 대상 모두 필수 수정이 필요한 정확성·유지보수 결함을 발견하지 못했다. 작업량이나 문서 길이로 차등하지 않고, 아직 root가 수행하지 않은 CI/배포나 명확히 unknown으로 남긴 계정 단계를 자동 감점하지 않아 같은 점수를 부여했다. 사전 동률 순서 `quality/evidence/app`과 최종 교체 결정은 root의 전체6평가 집계에 맡기며 바꾸지 않는다.

```json
{
  "peerReviews": [
    {
      "reviewer": "/root/resume14_quality",
      "candidate": "/root/resume14_evidence",
      "reviewedAt": "2026-10-16T05:17:21Z",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "guide-review의 네 전체 원문·번호·정확 URL·과거 checkedAt와 TJ confirm2/KY defer2는 실제 0400 요청 및 ledger77과 일치한다. SAM=samfree·생략 부제·음원 동일성을 만들지 않으며 본문 편집일/조회일·unknown/미수록을 구분한다. adsense-readiness도 공식 필수/조건부/내부 기준과 실제 신청·승인 unknown을 구분해 할당 계약을 충족한다.",
        "evidence": "실제 요청 JSON·응답 metadata hash/bytes·ledger와 동결 입력 hash·기존4시각을 연결하고, 이전 원문 HTML을 다시 파싱하지 않았음을 명시했다. 정책별 공식 URL과 실제 확인시각, urllib403/정상Node200·첫HTTP실패/strict27검사1attempt성공·beacon 관측/fieldfalse·ads.txt200과 계정일치unknown을 분리했다. root 영수증과 source privacy를 직접 대조해 문서와 모순된 사실을 발견하지 못했다.",
        "value": "앱이 과거 metadata 조회 성공을 번호 확정으로 오해하지 않도록 두 보류 사례의 안전한 표현·기존 상세 동선을 구체적으로 제공했다. 운영 HTML의 분석태그와 기존 privacy 안내 불일치를 발견해 root의 사실 교정으로 이어졌고, AdSense 준비에서 운영자 입력이 필요한 항목과 계속 가능한 개발을 구분해 실제 다음 행동을 만들었다.",
        "maintainability": "소유한 두 문서에서 기준 SHA·담당 범위·정확한 자료/시각·후속 담당·미검증 범위를 찾을 수 있고, source/ledger/계정·광고 설정을 임의 변경하지 않았다. 실제 영수증을 링크하고 비밀/판매자 값을 남기지 않으며 실패 이력을 보존했다. 할당 범위에서 재개를 막는 깨진 연결이나 실증된 유지보수 결함을 발견하지 못했다."
      },
      "reason": "실제 자료와 정책/운영 영수증을 대조한 결과 할당된 근거 검토와 준비 상태 분류를 수용한다. 확인되지 않은 권리·계정·실수신·실제 분석 payload는 정확히 unknown이며 평가를 낮출 근거가 아니다.",
      "evidence": [
        "/workspace/docs/content/guide-review-20261016-1400.md",
        "/workspace/docs/operations/adsense-readiness-20261016.md",
        "/workspace/docs/data/provider-review-requests-20261016-0400.json",
        "/workspace/jpoptoday/data/reviewed/provider-corrections.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-policy-http.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-baseline-first-http.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-baseline-http-retry.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-analytics-setting-presence.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-adstxt-observed.json",
        "/workspace/jpoptoday/src/pages/privacy.astro"
      ],
      "acceptance": "accepted: 실제 근거·출처/시각·판단 한계·운영 분석 안내 불일치와 교정 수용·준비 상태 및 다음 담당 분류 충족. 배포/Google 심사 결과는 이 평가의 완료 주장이 아니다.",
      "requiredChanges": [],
      "nonBlockingFollowUps": [
        "root가 최종 동일 SHA CI/원격 preview·운영·복구 결과를 연결하고 privacy 일반 운영 본문을 재확인한다.",
        "운영자가 안전한 계정 UI에서 실제 신청 상태·ads.txt 계정 일치·문의 실수신·권리 및 필요한 동의 항목을 확인한다."
      ]
    },
    {
      "reviewer": "/root/resume14_quality",
      "candidate": "/root/resume14_app",
      "reviewedAt": "2026-10-16T05:17:21Z",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "실제 두 source diff와 독립 객체 대조에서 기존 제목/URL·다른5가이드 전체·기존4출처를 보존하고 해당글 날짜만16일로 바꿨다. 추가4 원문/URL/checkedAt·SAM명의/생략부제 보류와 정확한 두 stable 상세 링크가 실제 요청과 일치한다. 두 target 전체31 E2E에서 TJ 공식 확인과 KY observed/null/string번호·SSR/색인·제보 동선을 검증해 요구를 충족한다.",
        "evidence": "작업 문서는 직접 source 비교·sandbox git EPERM 및 후속 PASS·Browser 도구 부재와 허용된 기존Playwright 경로·root 실행과 본인 QA를 구분한다. 네 실제 viewport QA JSON의 PASS/0errors/0warnings/0ads·focus/44px/overflow와 영속12PNG를 직접 읽고 열었다. 12개 hash가 manifest와 모두 일치하고 root preview31/production31 실제 로그가 구현 동선을 뒷받침한다.",
        "value": "읽기만 하던 표기 비교 글에 실제 판단 보류 두 유형과 해당 기존 곡 상세로 가는 직접 링크를 추가해 이용자가 TJ 근거와 KY 관측을 바로 비교하고 제보할 수 있다. 키보드·360px JS-off·320px 짧은 화면에서도 새 링크가 명확하게 보이고 조작돼 사용자 동선의 실제 공백을 해결한다.",
        "maintainability": "선택적 caseId/links를 기존 가이드 템플릿에서 SSR로 렌더하고 신규 링크에만 scoped CSS를 적용해 전역 스타일·데이터·의존성을 늘리지 않았다. 기존 글 전체와 출처를 보존하며 source/DOM 계약·재현 명령·실패/미실행 범위를 문서로 인계했다. 할당된 두 소스에서 실증된 호환성 또는 유지보수 결함을 발견하지 못했다."
      },
      "reason": "할당 소스와 실제 로컬 두 target 하네스·독립 네 환경 QA를 대조한 결과 기능·접근성·보존 요구를 수용한다. 깨진 링크·원문/시각 변조·관측번호의 공식확정 오표현·신규 레이아웃 결함을 발견하지 못했다.",
      "evidence": [
        "/workspace/jpoptoday/src/content/guides.ts",
        "/workspace/jpoptoday/src/pages/guides/[slug].astro",
        "/workspace/docs/engineering/guide-deferred-cases-20261016-1400.md",
        "/workspace/docs/data/provider-review-requests-20261016-0400.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-ui-qa.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-ui-artifacts.json",
        "/workspace/docs/engineering/design/deferred-guide-20261016-1400-1440x1100-top.png",
        "/workspace/docs/engineering/design/deferred-guide-20261016-1400-768x1024-samfree-link.png",
        "/workspace/docs/engineering/design/deferred-guide-20261016-1400-360x800-samfree-link.png",
        "/workspace/docs/engineering/design/deferred-guide-20261016-1400-360x320-ado-link.png",
        "/tmp/jpop-1400-preview-e2e.txt",
        "/tmp/jpop-1400-production-e2e.txt",
        "/tmp/jpop-1400-production-dist.txt",
        "/tmp/jpop-v044-production-local-final/build-info.json"
      ],
      "acceptance": "accepted: 기존6가이드/5글/4출처·추가4과거자료·2안정상세·관측/공식 구분·신규44px·SSR·360px JS-off/키보드·브랜드/무광고 요구를 로컬 범위에서 충족. 원격 배포는 별도 root 게이트다.",
      "requiredChanges": [],
      "nonBlockingFollowUps": [
        "root가 clean 동일 SHA의 두 target 재현·CI·원격 preview·실제 운영 본문·복구/재적용을 완료하고 결과 SHA를 연결한다."
      ]
    }
  ]
}
```
