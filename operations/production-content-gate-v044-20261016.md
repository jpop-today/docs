# v0.4.4 정책·콘텐츠·운영 게이트 독립 감사

[Issue30](https://github.com/jpop-today/jpoptoday/issues/30) · `resume-20261016-1400` · 담당 `/root/replacement14_gate` · 통합 root.

현재 판단은 **최종 M3 `f959f3671b8aeb766fd469558634f9680635e58c`의 제품 기술 게이트 수용 및 v0.4.4 실제 게시 확인**이다.318단위·type0·같은SHA CI/전체로컬재현·실제preview31·운영strict/직접메일·실제이전버전복구/새M3재적용·실제jpop.today원격31을 확인했다. Release는10월16일19:59:31KST 게시됐고 별도 릴리스 Discord도 실제sent다.10월17일의 작업은 같은 실행의 감사·문서/완료보고 마감이며 새 제품 개발·재배포·새 Release가 아니다. 관리자 완료 Discord와 문서PR18·보고PR34 병합은 아래 root의 실제 마감 영수증으로 확인됐다. M1/M2 실패와 M3 첫timeout은 보존하고 Google 심사는 unknown을 유지한다.

## 계약·재개·증거 경계

- 관련 스펙: `CONTENT-001`/`DATA-001`/`SONG-001`/`PRIVACY-001`/`SEO-001`/`DESIGN-001`/`RELEASE-001`/`ADS-001`/`TEAM-001`. [Issue30 제품 계약](../product/spec.md), [ADR-003](../adr/003-evidence-and-publication.md), [ADR-006](../adr/006-reviewed-corrections-and-production.md), [ADR-007](../adr/007-deployment-response-verification.md)을 따른다.
- source base `312c6b687c504cf82090ad26bdd9d829b99a105f`, docs base `4b19570a1dbd4698777aaf461d77999c8ad5bbc6`, branch `codex/30-deferred-guide-20261016`. 최초 feature H `74ee5f1db6831cb0f66d148be25ffef42dab72b8`, [PR31](https://github.com/jpop-today/jpoptoday/pull/31) 최초 제품 merge M1 `86090cf8ac9fca79ebe1d890c8f1bb3c592f1aa6`이다. 아래 최초 실행 표의 M은 이 M1이며, 운영 결함 보완의 새 H2/M2 증거와 혼동하지 않는다.
- 유일 편집 파일은 이 문서다. Git·package·CI·서버·배포·공용 보고는 root 단독이며 이 담당은 변경하거나 실행하지 않았다. 새 agent·사용자 채팅을 생성하지 않았다.
- root 인계에 따르면 14:18경 사용량 한도로 이 감사가 실제 중단됐고, 19:03 KST 같은 root·같은 worker로 재개했다. 중단 당시 감사 문서가 없었음을 보존한다. 중단 중 background 작업이 성공했다고 추정하지 않는다. 재개 시각과 기존 CI가 실제 실행된 시각은 다르다.
- 이후 사용량 중단 뒤 10월17일01시KST에도 같은 Issue30/run과 기존 교체ID로 마감을 재개했다. root는 환경372/current/running/enforced·sourceM3/docsbase4b·실제운영b91/M3/false-none을 재확인했다고 인계했다. 이 담당은 아래 Release·자정후API/HTTP 영수증을 직접 읽었으며, 변경 없는 제품을 재배포하거나 새 릴리스를 만들지 않았다. 기존 Actions의 실제 게시와 AI가 중단 없이 계속 작업했다는 주장은 다르다.
- 이 담당은 세 저장소 AGENTS, spec, status, roadmap, daily-development, continuous-development를 직접 읽었다. 관리형 환경 시작의 `cloud-environment-runtime` 스킬도 읽었으며, 환경 준비 관측을 배포 성공으로 해석하지 않았다. 이번 감사는 로컬 파일·JSON·로그·HTML 본문 읽기와 해시 대조이며 새 provider 조회·원격 HTTP·브라우저·테스트·빌드·CI·배포·복구를 실행하지 않았다.
- 이 문서의 result commit SHA와 원격 저장은 root가 통합 후 기록한다. `/tmp` 자료는 임시이며 다음 세션에 남는다고 보장하지 않는다.

## 실제 평가와 교체

[시작·rotation 기록](runs/resume-20261016-1400.json)과 [실제 6개 리뷰](runs/resume-20261016-1400-reviews.json)의 리뷰 본문·24개 기준별 이유·점수·수용 범위·제약 및 manager의 `evaluation`을 읽었다. 세 작업자는 각각 다른 두 명에게 평가받았고 자기 평가는 없다. 각 리뷰는 네 항목 모두 5/5이며 한 리뷰 20/20, 담당별 두 리뷰 합은 다음과 같다.

| 초기 담당 | 두 리뷰 합 | 정확성/증거/가치/유지보수성 합 | 실제 판정 |
| --- | --- | --- | --- |
| evidence | 40/40 | 각 10/10 | 할당 근거·정책 범위 수용 |
| app | 40/40 | 각 10/10 | 할당 콘텐츠·UI 로컬 범위 수용 |
| quality | 40/40 | 각 10/10 | 할당 하네스·로컬 증거 범위 수용 |

합계와 항목별 합이 모두 완전 동률이다. 사전에 기록된 `quality/evidence/app` 순서에 따라 quality를 active roster에서 제외한 것이며 낮은 품질이 입증된 교체가 아니다. root의 실제 rotation 기록은 quality interrupt의 이전 상태가 `completed`, 신규 교체 담당이 `/root/replacement14_gate`, active roster가 evidence/app/replacement의 세 명임을 명시한다. 물리 삭제 API가 없으며 실제 삭제를 주장하지 않는다. 기존 결과·리뷰·제외 이력은 보존한다. 이 감사는 기존 6평가를 바꾸거나 새 평가를 대신 생성하지 않았다.

## 콘텐츠·자료·개인정보 독립 읽기

| 조건 | 직접 읽은 증거와 확인 범위 | 판단 |
| --- | --- | --- |
| 두 보류 사례의 원문·판단·시각 | `docs/data/provider-review-requests-20261016-0400.json`의 실제 네 candidate와 네 decision, [편집 근거](../content/guide-review-20261016-1400.md), `src/content/guides.ts`를 대조했다. TJ27031/TJ68868 confirm, KY43185/KY76509 defer이며 가이드의 전체 제목·가수·공식 URL·네 과거 checkedAt가 일치한다. | 수용. 조회 성공을 번호 확정으로 올리지 않는다. |
| samfree/SAM·Ado 생략 제목 | SAM과 samfree 명의 동일성 미확보, 금영 제목 셀의 실제 `..`와 버전 미확보를 본문에서 설명한다. KY 관측 번호를 유지하고 미수록·동일 음원/편곡·생략된 부제를 추정하지 않는다. | 수용. 음원·기기 제공·권리는 별도 unknown. |
| 실제 공개 모델의 업체별 상태 | clean M production의 `catalog.json`3317곡/3317고유ID와 두 정확한 ID의 레코드를 직접 읽었다. 두 TJ는 문자열27031/68868·provider-confirmed·conflictfalse·실제 title/artist/checkedAt 근거다. 두 KY는 문자열43185/76509·observed-unverified·conflictfalse·evidence/correction null이며 ledger77에는 두 TJ confirm만 있다. | 콘텐츠 설명과 공개 모델이 일치한다. 두 KY의 새 confirm/suppress·근거를 생성하지 않았다. |
| 기존 콘텐츠·자료 보존 | `/tmp/jpop-1400-base-guides.ts`와 현 소스에서 대상 글 앞의 다른 다섯 가이드 전체 prefix가 바이트 단위로 같다. 대상 글의 기존 네 URL/checkedAt와 추가 네 시각을 직접 읽었다. ledger bytes hash는 `a2d8ea8e9eba737a38a1f2968a0a1687498bf04fb0f8742ea7f42eedaef8f708`이며 기존 77항목 기준과 같다. staging/chart/identity bytes hash도 기준 문서와 일치한다. | 수용. 이번 편집일 10월16일을 새 번호 조회일로 쓰지 않는다. |
| 기존 상세 동선 | `src/pages/guides/[slug].astro`는 기존 두 stable ID 경로를 SSR 링크로 렌더하고 선택적 section links에만 scoped CSS·min-height44px를 둔다. clean M의 두 target HTML에서도 정확한 두 기존 상세 URL·편집일·기존4+추가4 source 시각을 읽었다. | 소스/SSR 범위 수용. 실제 키보드·브라우저 실행은 아래 root/앱 증거와 구분. |
| 수정된 개인정보 안내 | `src/pages/privacy.astro`와 dirty/clean M의 두 target privacy 본문을 읽었다. 적용일10월16일, 호스팅·브라우저 저장·이용 통계·광고를 나누고 운영 Web Analytics 코드 존재와 공식 설명상의 가능한 지표를 안내한다. 예전 “방문 분석 스크립트 없음” 문장은 없다. | 로컬 범위 수용. 모든 처리의 cookie/개인정보 미사용·IP 미처리·법률 전체 준수를 보장하지 않는다. |
| 실제 운영 분석 관측 | [AdSense 준비 문서](adsense-readiness-20261016.md), [안전한 HTTP 분류](runs/resume-20261016-1400-policy-http.json), [설정 field 존재 확인](runs/resume-20261016-1400-analytics-setting-presence.json)을 읽었다. 기존 일반 운영 HTML의 beacon 태그 존재와 project field 두 개 false는 서로 다른 관측이다. | 안내 교정 근거 수용. 삽입 계층·실제 실행/전송·payload·보관기간·전체 설정은 unknown. |
| ads.txt | [실제 분류 영수증](runs/resume-20261016-1400-adstxt-observed.json)은 HTTP200/text/plain59bytes/HTMLfalse/유효 Google 판매자형식1행이다. source/pinned 파일 부재로 운영404를 추정하지 않는다. | 존재/형식만 확인. 계정 일치·소유/생성 경로·Google 계정 판정 unknown. 게시자 값을 복사·생성·교체하지 않았다. |

고정 clean M privacy는 preview 5,328bytes/SHA-256 `e042774a7fbe0433807197034c54307a63d6021cd7a1591cb66f6377813b8ef6`, production 5,326bytes/`bcc03fd15d5109109133f24eab63b2171a882ad1125c4feb302b19a0544db50c`다. 대상 가이드는 preview 16,836bytes/`5dc3baf23a91920430e67b8f1c615b06a0f1e89adf0bc3963f3d8c07874adff3`, production 16,834bytes/`4bc575618300472e05d3f724526309f9ae4d6ffb2226dd3ca08409543ce40009`다. 두 파일 모두 preview `noindex, follow`, production `index, follow`를 직접 읽었다. 이 파일 읽기를 전체 운영 본문 검증으로 확대하지 않는다.

공식 Google·Cloudflare 문서의 URL·실제 확인 시각·요구와 해석은 evidence 담당이 실제 조회해 남긴 준비 문서에서 읽었다. 이번 감사가 그 공식 URL에 새 HTTP를 요청하거나 Google 계정을 확인한 것은 아니다. 문서의 공식 필수/조건부/내부 기준 구분을 수용한다. 임의 글 수·자료 수·트래픽·기간·버전1.0·모든 번호 확정·Actions binding·Search Console 완료를 보편적인 Google 심사 필수조건으로 만들지 않는다. 기술 준비 `preparing`, 실제 계정·소유권·신청·심사는 `unknown`이다.

## 로컬·CI·배포 게이트의 실제 상태

다음 표는 최초 M1 실행 당시의 증거·수용 범위·실패를 보존한다. 이후 보완의 현재 최종 상태는 아래 M3 표를 따른다.

| 단계 | 직접 읽은 증거 | 현재 상태 |
| --- | --- | --- |
| 기존 dirty 로컬 | `/tmp/jpop-1400-unit.txt`314/314·fail0, `check-final.txt`26files/0error/0warning/0hint, preview31(1.1m)/production31(57.7s), dist3347files/3336HTML·0/45index를 읽었다. | PASS인 기존 실행 로그. sourcebase312c+dirty 결과이며 clean CI/원격 결과로 쓰지 않는다. |
| 4 viewport/12 PNG | [QA JSON](runs/resume-20261016-1400-ui-qa.json)은1440×1100·768×1024·360×800 JSoff·360×320 JSoff의 네 PASS와 오류/경고/광고 빈 목록을 보존한다. [manifest](runs/resume-20261016-1400-ui-artifacts.json)의 영속12PNG 해시를 직접 계산해12/12 일치했다. | 기존 앱/품질 실행 증거 수용. 이 감사의 새 브라우저 실행이나 전체12화면 시각 검토를 주장하지 않는다. |
| feature H CI | [feature CI](runs/resume-20261016-1400-feature-ci.json)는 app/contracts PR/push 네 run success다. PR 이벤트의 artifact source는 synthetic merge `c8606524234c9898138dd98b10a6edc8acffe0b8`; push 이벤트 artifact는 exact H다. | 구분하여 수용. PR run metadata head만으로 artifact가 H라고 하지 않는다. |
| 제품 merge·같은 M CI | PR31/M은 [run](runs/resume-20261016-1400.json), 실제 선택 job 로그는 [main CI](runs/resume-20261016-1400-main-ci.json)에 있다. app38855538056/contracts38855538068, app job116826032395의314/type0/preview31와 production job116826032611의31, 두 artifact의 sourceM·3347files/3336HTML·0/45index를 읽었다. | 같은 M CI PASS. 원격 전체 CI archive와 로컬 파일의 전량 비교는 미실행. |
| clean M 반복 재현 | [재현 영수증](runs/resume-20261016-1400-reproducibility.json)은 target별 두 clean build의 전체 pathset·every-file hash 일치, 각3347files/3336HTML, dirtyfalse/sourceM이다. `/tmp/jpop-v044-preview-86090cf`·`/tmp/jpop-v044-production-86090cf` metadata와 관련 본문도 직접 읽었다. | 로컬 두 target 재현 PASS. 이 담당이 빌드하거나 CI archive를 비교한 것은 아니다. |
| 실제 preview 배포·원격 검증 | [preview 배포](runs/resume-20261016-1400-preview-deployment.json)는 CLI0, `49393954-5169-429d-863d-69df9f67439f`, sourceM/dirtyfalse/stage success다. URL은 <https://49393954.jpoptoday-preview.pages.dev>다. [원격 preview](runs/resume-20261016-1400-remote-preview.json)의 run38884926232/job116920245001 success, sourceM·targetpreview·0index/3347files,10:06:07UTC strict summary PASS/1attempt,10:07:16UTC31passed(48.6s)를 직접 읽었다. root는 이 strict 실행을27검사라고 인계했으며 보존된 선택 로그는 개별27검사 대신 최종 summary를 담는다. | 실제 배포·같은 M 원격 preview PASS. 전용 preview 프로젝트의 Cloudflare `production` label은 jpop.today 운영 전환이 아니다. |
| 재개 시 기존 운영 | [API](runs/resume-20261016-1400-resume-production-api.json)는 todayjpop/jpop.today `eb9161b3-d43b-45e4-beb8-9b068d456dbe`·v0.4.3/M48c58d5, 개인Git metadata/dev·자동false/none을 보존한다. [HTTP](runs/resume-20261016-1400-resume-production-http.json)는10:04:53–10:05:19UTC strict27/1attempt PASS다. | 기존 운영 정상 재확인. 새 M 운영 반영 결과가 아니다. |
| 새 M 운영·strict 본문 | [운영 배포](runs/resume-20261016-1400-production-deployment.json)는 CLI0, todayjpop/jpop.today `82b1b848-5d5f-4892-a15c-0a8a5265dff0`·sourceM/dirtyfalse/stage success이며 기존 개인Git metadata/dev·자동false/none을 보존한다. [첫 운영 HTTP](runs/resume-20261016-1400-production-first-http.json)의10:08:51–10:09:13UTC 실제27검사/1attempt 전체PASS를 읽었다. 전후metadata·catalog·robots/sitemap·대상가이드·대표상세·비공개경로404를 포함하며 privacy는 허용된 이메일보호 정규화 뒤 pinnedhash와 일치한다. | 실제 운영 반영·해당 strict 검사 묶음 PASS. 이후 별도 원격 회귀의 두 실패는 다음 행에 보존한다. |
| 실제 운영 원격 브라우저 | [production verify-only workflow](runs/resume-20261016-1400-remote-production.json)의 run38885429674/job116921918724는 같은 M이나 최종 failure다. 선행 local31·strict summary PASS/1과 실제 Actions binding missing/deploy skipped를 구분한다. [실패 상세](runs/resume-20261016-1400-remote-production-failure.json)는 actual remote29passed/2failed이며 두 `deferred-guide` JSoff360px 사례 모두 제보 href를 `new URL`로 읽는198줄에서 `TypeError: Invalid URL`이다. | **failed / 릴리스 게이트 보류.** local31/preview31/strict27로 덮지 않는다. 후속 실제 DOM 분류에서 직접 mailto가 Cloudflare 보호 링크로 바뀐 것을 확인했다. 수정/새 SHA 게이트는 아래 별도 기록한다. |
| 일반 운영의 실제 정책·콘텐츠 | [일반 Node HTTP 관측](runs/resume-20261016-1400-production-policy-observed.json)은10:08:55UTC privacy200·새분석안내/적용일/공식링크true·구문구false·beacontrue·Google광고요청scriptfalse,10:08:57UTC가이드200·두stablecase/명의·실제생략표기/과거checkedAt를 확인했다.10:09UTC ads.txt200/59bytes/기존hash/판매자형식1행·계정unknown도 보존한다. | 실제 교정 문구·두 사례 반영 수용. 일반privacy5936bytes와pinned5326, 일반가이드17201와pinned16834는 각각 `exactPinned:false`다. 일반 응답과 strict 응답을 바이트 일치로 합치지 않는다. |
| 실제 이전버전 복구/새버전 재적용 | [이번 recovery summary](runs/resume-20261016-1400-recovery-node-summary.json)의10:09:27–10:10:02UTC old/new state·HTTP true를 읽었다. [rollback API](runs/resume-20261016-1400-rollback-node-api.json)는82b/M→eb916/v0.4.3/M48c, [old state](runs/resume-20261016-1400-old-state-node-observed.json)는1observation/exactmetadata·home/sitemap hash 일치, [rollback HTTP](runs/resume-20261016-1400-rollback-node-http.json)는27/27·1attempt PASS다. 이후 [restore API](runs/resume-20261016-1400-restore-node-api.json)는eb916→82b/M, [new state](runs/resume-20261016-1400-new-state-node-observed.json)는1observation 일치, [restore HTTP](runs/resume-20261016-1400-restore-node-http.json)도27/27·1attempt PASS다. 두 API 모두 기존domain/Gitmetadata·자동false/none을 보존한다. | 이번 첫 controlled recovery 성공. 실제 이전버전 복구와 같은 새 M 재적용을 구분해 수용한다. 과거 run의 실패·성공을 이번 시도로 복사하지 않는다. |
| Release·완료 Discord·최종 저장 | 이번 v0.4.4 실제 tag/Release·완료 송신·docs/report 저장 영수증은 아직 읽지 않았다. 시작 알림과 완료 알림을 구분한다. | pending. 실행 뒤 root가 실제 링크·SHA·시각·sent/skipped/failed를 연결한다. |

초기 baseline transport 실패, Python 일반 HTTP403과 정상 Node200, worker Git 읽기 EPERM 후 대조 성공, root 보조 validator TypeError·Git proxy연결 실패와 후속 성공은 [실패 기록](runs/resume-20261016-1400-local-failures.json) 및 [run 후속](runs/resume-20261016-1400.json)에 보존돼 있다. 이력상의 해결 전 상태를 현재 성공으로 덮거나, 실패 원인을 검증 없이 단정하지 않는다.

## 운영 결함 후속 소스 읽기 · 새 SHA 게이트 대기

[실제 링크 분류](runs/resume-20261016-1400-js-off-report-defect.json)를 직접 읽었다. 10:16:11UTC 실제 samfree 상세 HTTP200의 제보 링크는 존재하지만 직접 mailto false·Cloudflare 보호 링크 true·decoder script true다. 실제 remote JSoff 실패와 함께 제품 결함을 입증하며, 테스트 URL 파서에 base를 추가하거나 보호 href를 테스트 안에서 복호화하는 것으로 수용하지 않는다.

[공식 Cloudflare 근거](runs/resume-20261016-1400-email-protection-reference.json)는 10:14:45UTC HTTP200으로 특정 HTML의 `email_off` 주석 범위만 이메일 난독화에서 제외하는 방법을 확인했다. 이는 root의 실제 공식 문서 조회 영수증 읽기이며 이번 감사의 새 원격 요청이 아니다. spec 후속 단락이 구현 전에 추가됐고 zone 전체 설정을 바꾸지 않는 범위다.

현재 작업 소스의 `src/pages/songs/[id].astro`·`src/pages/contact.astro`·`src/pages/privacy.astro`에서 기존 공개 이메일 anchor 주변의 좁은 `email_off` wrapping을 읽었다. 기존 제보 subject/body의 곡 제목·가수·안정 절대 URL과 공개 수신자, privacy 정책 본문을 그대로 보존하는 방향을 수용한다. root의 `e2e/deferred-guide.spec.ts` 후속180–238줄도 읽었다. JSoff360px에서 직접 mailto를 명시 검사하고 같은 수신자·subject/body·정확한 곡 URL, contact/privacy의 직접 이메일 링크·키보드 focus/기존 뒤로가기를 검사한다. 복호화나 URL base 추가로 실제 기능 검사를 약화하지 않는다.

[후속 로컬 영수증](runs/resume-20261016-1400-contact-fix-local.json)은 dirty M1+연락처 보완의 check/build/verify exit0·type0, 전체3317곡 HTML의 paired marker와 contact/privacy marker, 마커만 제거한 기존 템플릿 byte 비교 일치, 강화된 preview31(1.1m)를 보존한다. 이 담당은 이후 현재 `dist/build-info.json`의 source `25b178ffbd8916413c482b8fc9066b61d09b5e62`/production/dirtyfalse를 읽고 contact/privacy와 두 사례 상세 HTML의 개폐마커 각1쌍·직접mailto 존재·보호href 부재를 직접 확인했다. 전후 metadata 바이트도 같았다. 따라서 소스 마커가 컴파일 산출물에서 사라질 가능성은 이 네 경로 읽기 범위에서 해소됐다. 이 담당이 전체3317파일의 마커를 새로 검사하거나 빌드/브라우저를 실행한 것은 아니다.

root의 후속 인계는 [PR32](https://github.com/jpop-today/jpoptoday/pull/32)의 H2 `25b178ffbd8916413c482b8fc9066b61d09b5e62` → 최종 M2 `20de2bd985c994c8e92ffde40a35df55173a9af0` 병합이다. 아래 후속 증거는 최초 M1과 별도 source로 읽는다.

| 후속 단계 | 직접 읽은 증거와 범위 | 판단 |
| --- | --- | --- |
| H2 CI·로컬 | [후속 feature CI](runs/resume-20261016-1400-contact-fix-feature-ci.json)는 PR/push app/contracts 네 run success다. 보존된 exact H2 push job116925484201/116925484623은314/type0·preview31(38.0s)/production31(32.0s), 각3347files/3336HTML·0/45index를 기록한다. PR artifact는 별도 synthetic merge이며 exact H2와 구분한다. 후속 로컬 영수증의 clean H2 production31(1.0m) 추가도 읽었다. | 해당 H2·로컬 범위 PASS. M2의 같은 SHA CI와 실제 운영 효과는 별도 증거가 필요하다. |
| 후속 캡처 | [2PNG manifest](runs/resume-20261016-1400-contact-fix-screenshots.json)와 실제 두 영속 PNG를 직접 열었다. 두 곡의 원제·가수·TJ 공식확인/금영 관측·기존 번호·브랜드와360px 읽기 배치가 보인다. | 명시한 시각 범위 수용. 캡처는 제보 링크를 보기 전 위치이며 실제 href·Cloudflare 효과·메일 처리 증거가 아니다. |
| 후속 소스 수용 | [실제 소스 보충 리뷰](runs/resume-20261016-1400-contact-fix-source-reviews.json)는 evidence의 소스/회귀 설계 수용과 이 담당의 소스 및 컴파일 마커 확인을 보존한다. | 실제 읽기 범위만 수용. 기존6점수/순위는 변경하지 않았다. |
| 같은 M2 CI | [후속 main CI](runs/resume-20261016-1400-contact-fix-main-ci.json)의 sourceM2·app38886849822/contracts38886849872 success를 읽었다. app job116926565611은314/type0·preview31(42.4s), production job116926566123은31(40.9s)이며 각3347files/3336HTML·0/45index다. | 같은 M2 CI PASS. |
| M2 반복 재현·마커·동결 입력 | [후속 재현](runs/resume-20261016-1400-contact-fix-reproducibility.json)은10:23:25–10:23:54UTC 두 target별 clean2build의 전체 pathset·every-file hash 일치, 각각3347files/3336HTML·dirtyfalse/sourceM2다. [최종 artifact 검사](runs/resume-20261016-1400-contact-fix-final-artifact-check.json)는 두 target 각각3317곡+contact/privacy3319페이지의 정확한 paired marker와 기존globalCSS/ledger77/staging/chart/identity/helper bytes hash 보존을 기록한다. | 해당 로컬 재현·최종 검사 PASS. 전체 CI archive와의 파일 전량 비교는 미실행이다. |
| M2 실제 preview | [후속 preview 배포](runs/resume-20261016-1400-contact-fix-preview-deployment.json)는 CLI0, `914d714a-daf7-43c4-8c1b-7805b022944b`/sourceM2/dirtyfalse/stage success다. URL은 <https://914d714a.jpoptoday-preview.pages.dev>다. [후속 preview HTTP](runs/resume-20261016-1400-contact-fix-preview-http.json)는10:24:59–10:25:02UTC 같은M2/targetpreview·실제27/27·1attempt PASS다. [후속 원격 preview](runs/resume-20261016-1400-contact-fix-remote-preview.json)의run38887088270/job116927348602 success·sourceM2/0index·31passed(49.3s)도 직접 읽었다. | 실제 preview 배포·strict·원격31 PASS. |
| M2 운영 배포·직접 이메일 | [M2 운영 배포](runs/resume-20261016-1400-contact-fix-production-deployment.json)는 CLI0·todayjpop/jpop.today `3711936b-e75d-42f9-9e52-af9e2d312f55`/sourceM2/dirtyfalse/stage success와 기존dev/자동false-none을 기록한다. [실제 이메일 분류](runs/resume-20261016-1400-contact-fix-production-email-observed.json)는10:28:17–10:28:24UTC samfree/contact/privacy 각HTTP200·mailto1개·pinned 전체 opaque payload 일치·CF보호href없음이다. | 해당 실제 운영3경로의 직접 이메일 보존 수용. 메일 실행·발송/수신 증거가 아니다. |
| M2 운영 strict 실패 | [M2 첫 HTTP](runs/resume-20261016-1400-contact-fix-production-first-http.json)는10:28:06–10:29:06UTC contact BODY_MISMATCH 반복 뒤17시도/60초 `DEADLINE_EXCEEDED`로 실패했다. [주석 변환 분류](runs/resume-20261016-1400-contact-fix-marker-transform.json)는 같은 정상 요청 경로에서 contact/privacy가 trusted expected의 정확한 paired marker33bytes만 제거하면 actual 전체 bytes와 일치함을 확인했다. | failed를 보존한다. 직접 이메일 기능 보존과 strict verifier의 미지원 변환을 구분하며 새 verifier/최종 SHA 검증 전 통과로 올리지 않는다. |
| M2 일반 정책 응답 | [M2 운영 정책](runs/resume-20261016-1400-contact-fix-production-policy-observed.json)은 새 분석 안내·구문구 없음·실제beacon·Google광고요청script없음, 두case/과거checkedAt, ads.txt200/59bytes/기존hash/계정unknown을 확인했다. 일반privacy5693bytes와pinned5359, 일반가이드17201과pinned16834는 각각 exactPinnedfalse다. | 실제 정책·콘텐츠 반영 범위 수용. 일반 응답과 strict/pinned 전체 bytes 일치를 주장하지 않는다. |
| M2 당시 남은 최종 게이트 | verifier 보완 뒤 새 SHA의 CI·두 target 재현·preview·운영 strict/원격31·v0.4.3 복구→새 SHA 재적용·Release/완료 송신이 필요했다. | 당시 pending. 이후 M3 영수증은 아래 별도 수용하며 M1/M2 결과를 바꾸지 않는다. |

최초 M1 증거·29/2실패와 실제 복구 성공을 그대로 보존하고 새 SHA 결과로 바꿔 쓰지 않는다. 이 단계에서는 해결 검증 전 v0.4.4 tag/Release·완료 알림을 발행하지 않았다. 실제 최종 발행 결과는 아래 M3 표를 따른다.

## M2 주석 변환 후 strict verifier 보완 읽기

구현 전 spec에 추가된 “처리된 이메일 범위 주석” 계약과 root가 소유한 `scripts/verify-deployment.mjs`의103–179·341–355줄, `tests/deployment-healthcheck.test.mjs`의 기존 이메일 fixture/계약과 추가530–611줄을 직접 읽었다. 현재 소스 논리·보안 범위에서 필수 문제를 발견하지 못했다.

- trusted expected에 정확한 paired `email_off`로 둘러싸인 직접 mailto anchor만 별도 occurrence budget으로 분리한다. raw 수신자·query/entity·label을 포함한 전체 anchor가 동일해야 마커를 복원한다.
- 명시적 JSoff marked anchor는 기존 CF 복호화 허용 budget에서 제외한다. 같은 identity가 marked/unmarked 양쪽에 있어 위치가 모호하면 `CF_EMAIL_MISMATCH`로 거부한다. 직접 링크가 다시 보호 href로 바뀌어도 기존 복호화 경로로 수용하지 않는다.
- actual의 정확한 anchor 개수만 마커 복원에 사용하며, 기존 마커가 남아 있으면 그대로 둔다. 임의 주석·속성·추가 script·본문·누락/중복·불완전 쌍을 지우지 않는다. 마지막 `actual.equals(expected)` 전체 바이트 비교가 유지된다.
- 추가4테스트그룹은 정확한 복원/기존 unmarked CF 혼합 호환, JSoff 재보호와 budget 모호함 거부, raw/normalized hash와 안전한 receipt, payload/query/entity/label/속성/중복/부분마커/추가본문·script 변조 거부를 다룬다. 원시 이메일 payload를 receipt에 남기지 않는 기대도 있다.

소스 수용 당시에는 실행 결과를 앞당기지 않았다. 이후 다음 실제 영수증을 추가로 읽었다. UI3템플릿·직접mailto E2E 요구를 약화하거나 zone 전체 설정을 바꾸는 수정으로 수용하지 않는다.

| verifier 후속 단계 | 직접 읽은 증거와 범위 | 판단 |
| --- | --- | --- |
| 추가 소스 리뷰 | [marker 보완 소스 리뷰](runs/resume-20261016-1400-marker-fix-source-reviews.json)는 evidence와 이 담당의 실제4기준 읽기 수용·필수변경없음·실행/새SHA pending 경계를 보존한다. | source 논리/합성회귀 설계 수용. 기존6점수·순위 불변. |
| 로컬76/318·기존 M2 HTTP | [marker 보완 로컬](runs/resume-20261016-1400-marker-fix-local.json)은targeted76/fail0·전체318pass/fail0/skipped0이다. [현 M2 HTTP](runs/resume-20261016-1400-marker-fix-current-M2-http.json)는10:33:04–10:33:20UTC sourceM2/production·27/27·1attempt PASS다. contact/privacy/대표상세의 raw/expected/normalized hash와 정확한마커복원1개씩을 읽었다. | 보완 verifier로 기존 M2를 검증한 실제 성공. 새 M3 운영 성공으로 쓰지 않는다. 이 담당의 명령 실행이 아니다. |
| exact H3 CI | [marker feature CI](runs/resume-20261016-1400-marker-fix-feature-ci.json)는 H3 `bec6d04a264f9464002240d6bf19ad24d35965a7`의 PR/push app/contracts4run success다. exactH3 push job116930580879는318/type0·preview31(38.5s), production job116930580375는31(34.2s)이며 두 target은3347files/3336HTML·0/45index다. | 해당 H3 CI PASS. PR synthetic artifact와 exact push를 구분한다. |
| 최종 M3·같은 SHA CI | [PR33](https://github.com/jpop-today/jpoptoday/pull/33)의 M3는 `f959f3671b8aeb766fd469558634f9680635e58c`다. [marker main CI](runs/resume-20261016-1400-marker-fix-main-ci.json)의 app38888357099/contracts38888357160 success, app job116931533545의318/type0·preview31(40.9s), production job116931534164의31(30.4s)를 직접 읽었다. 두 target은 각각3347files/3336HTML·0/45index다. | 같은 M3 CI PASS. 최종 단위 기준318·UI31을 구분한다. |
| M3 clean 재현·최종 artifact | [marker 재현](runs/resume-20261016-1400-marker-fix-reproducibility.json)은10:36:51–10:37:26UTC 두 target별 clean2build 전체pathset/everyfile 일치·각3347files/3336HTML·sourceM3/dirtyfalse다. [marker 최종검사](runs/resume-20261016-1400-marker-fix-final-artifact-check.json)는 각target3317곡/3319정확paired메일anchor 및globalCSS/ledger77/identity/staging/chart/providerhelper hash 보존을 기록한다. | 로컬 재현·명시한 artifact 검사 PASS. 전체 CI archive와의 전량 파일 비교는 미실행이다. |
| M3 실제 preview | [marker preview 배포](runs/resume-20261016-1400-marker-fix-preview-deployment.json)는 CLI0·`c06ff6d6-9c19-4751-8cd6-d8c5be39507e`·M3/dirtyfalse/stage success다. URL은 <https://c06ff6d6.jpoptoday-preview.pages.dev>다. [marker preview HTTP](runs/resume-20261016-1400-marker-fix-preview-http.json)는10:38:39–10:38:41UTC 같은M3/preview·27/27·1attempt PASS다. [marker 원격 preview](runs/resume-20261016-1400-marker-fix-remote-preview.json)의run38888585713/job116932290836 success·M3/0index·31passed(50.4s)도 읽었다. | 같은 M3 실제 preview 배포·strict·원격31 PASS. |
| M3 실제 운영 배포·strict | [marker 운영 배포](runs/resume-20261016-1400-marker-fix-production-deployment.json)는 CLI0·todayjpop/jpop.today `b91efaaa-b971-442b-b520-1505968ef532`·M3/dirtyfalse/stage success, 기존domain/Gitmetadata-dev·자동false-none을 보존한다. [marker 첫 운영 HTTP](runs/resume-20261016-1400-marker-fix-production-first-http.json)는10:41:00–10:41:25UTC 첫build-info 요청이REQUEST_TIMEOUT, 두 번째시도는27/27 PASS다. | 실제 M3 배포·strict PASS/2attempt. 첫 timeout을 지우거나 metadata불일치/캐시 원인으로 단정하지 않는다. |
| M3 실제 직접메일·정책 | [marker 이메일 관측](runs/resume-20261016-1400-marker-fix-production-email-observed.json)은10:41:11UTC samfree/contact/privacy 각HTTP200·직접mailto1개·원래 전체 opaque payload 일치·CF보호href없음이다. [marker 정책 관측](runs/resume-20261016-1400-marker-fix-production-policy-observed.json)은10:41:13UTC 새privacy·기존구문없음·beacontrue/Google광고scriptfalse·guide2case/과거checkedAt·ads.txt200/59bytes/1행/계정unknown을 기록한다. | 명시한 일반 응답의 실제 기능·정책 범위 수용. privacy5693/pinned5359·guide17201/pinned16834는 exactPinnedfalse이며 전송payload/계정승인은 unknown이다. |
| 최종 M3 실제 복구·재적용 | [M3 recovery summary](runs/resume-20261016-1400-marker-fix-recovery-node-summary.json)의10:41:51–10:42:31UTC old/new state·HTTP true를 읽었다. [rollback API](runs/resume-20261016-1400-marker-fix-rollback-node-api.json)는b91/M3→eb916/v0.4.3/M48c, [old state](runs/resume-20261016-1400-marker-fix-old-state-node-observed.json)는1observation/exactmetadata·body일치, [rollback HTTP](runs/resume-20261016-1400-marker-fix-rollback-node-http.json)는27/27·1attempt PASS다. 이후 [restore API](runs/resume-20261016-1400-marker-fix-restore-node-api.json)는eb916→최종b91/M3, [new state](runs/resume-20261016-1400-marker-fix-new-state-node-observed.json)는1observation 일치, [restore HTTP](runs/resume-20261016-1400-marker-fix-restore-node-http.json)도27/27·1attempt PASS다. domain/Gitmetadata/dev·자동false-none을 보존한다. | 최종 M3 후보의 실제 첫 복구·재적용 성공. 전체 Issue30에서는 M1에 이은 두 번째 controlled recovery다. 원 helper의 “current run first” 표현은 보존됐고 `managerScopeClarification`이 정확한 범위를 정정한다. M2는 strict 실패로 복구 미실행이다. |
| 최종 M3 실제 운영 원격31 | [marker 운영 verify-only](runs/resume-20261016-1400-marker-fix-remote-production.json)는run38889097513/job116933967026 success·sourceM3·verifyOnlytrue·actualURLjpop.today다.318pass/type0·두target3347files/3336HTML·0/45index,preview/production strictsummary각1attempt PASS를 읽었다.10:45:06UTC첫31(38.9s)은localproduction,10:46:03.7446859UTC최종31(53.5s)은canonical SHA 검증 뒤 실제jpop.today 브라우저다. | 같은 M3 실제 운영 원격31 PASS. 실제 ActionsCFbindingmissing/deployskipped를 확인했으며 root managed 배포와 구분한다. 첫 운영 HTTP의 timeout/2attempt 기록은 별도로 유지한다. |
| v0.4.4 실제 tag·Release | [Release 영수증](runs/resume-20261016-1400-release.json)을 직접 읽었다. version-release38890640129의 검증 job116939015889는318pass/fail0·type0·31passed(40.2s)를 기록한다. annotatedtag `1503836a52b93fab3aa3ed78883828ff44a5027e`는 commit M3를 가리킨다. [v0.4.4](https://github.com/jpop-today/jpoptoday/releases/tag/v0.4.4)는Release414863520·draftfalse/prereleasefalse·published2026-10-16T10:59:31Z, 즉10월16일19:59:31KST다. | 실제 게시 확인.10월17일 새 제품 릴리스로 쓰지 않는다. |
| 실제 릴리스 Discord·남은 완료 전달 | 같은 Release 영수증의 explicit release sender job116939603834는10:59:32.2047361UTC·19:59:32.2047361KST에1attempt actualsent를 기록한다. 이는 관리자 완료보고 송신과 별도다. | 릴리스 알림 sent 수용. 관리자 완료 Discord·최종docs/report PR저장 및 결과SHA는 여전히 pending이며 root가 실제 영수증으로 마감한다. |
| 10월17일 기존 운영 재확인 | [자정후 API](runs/resume-20261016-1400-after-midnight-production-api.json)는기존b91/M3·todayjpop/jpop.today·dev/자동false-none을 보존한다. [자정후 HTTP](runs/resume-20261016-1400-after-midnight-production-http.json)는2026-10-16T16:02:21–16:02:49UTC, 즉17일01:02:21–01:02:49KST sourceM3/production·27/27·1attempt PASS다. 전후metadata·입력hash·전체본문 비교/허용marker복원·비공개404를 읽었다. | 기존 실제 운영의 새 재조회 PASS. 새배포·새개발·24시간연속AI실행 증거로 쓰지 않는다. |

## 남은 사실과 최종 수용 조건

최초 소스·로컬 읽기 이후 발견된 두 제품 회귀 실패와 M2 strict 실패는 위에 보존했다. 보완된 최종 M3의 소스·같은SHA CI/재현/실제preview31·운영 strict/직접메일·최종 실제복구/재적용·실제운영31까지 읽어 RELEASE-001 기술 범위를 수용한다. Google/권리/운영자 사실의 승인이나 Release 게시·알림 발송을 대신하지 않는다. 일반 운영 privacy의 교정 문구·가이드 두 사례와 strict artifact 응답은 별도로 연결했다. 일반 응답의 beacon·소비된주석과 strict의 정확한 marker복원은 확인됐지만, 응답 경로별 삽입 주체·설정/캐시 계층 전체와 정확한 원인은 미확인이다. strict 성공 한 묶음으로 모든 CDN·실제 analytics payload·Google 크롤러 판정까지 확정하지 않는다.

원자료 전체 이용 권리·가사/이미지 허락·현장 기기/음원/편곡·문의 실수신/회신·실제 개인정보 운영 관행·Google 계정 자격/소유권/신청/심사·ads.txt 계정 일치·분석 payload/보관기간은 **unknown**을 유지한다. 이들은 별도 운영자 입력 또는 추가 사실 확인이 필요하며, 이번 무광고 제품 개선을 모두 중단할 이유로 사용하지 않는다. 광고/CMP 설치·광고 요청/클릭·문의 발송·계정 로그인·법적 동의를 이 담당은 수행하지 않았다.

[실제 보충 읽기](runs/resume-20261016-1400-supplemental-reviews.json)의 evidence10:08:32–10:09:29UTC·app10:08:51–10:10:07UTC 수용을 직접 읽었다. 두 담당 모두 source/cleanlocal/CI/실제preview 범위만 수용했고 당시새production·복구·remote31·Release/완료송신은pending으로남겼다. 이를 이후실패한원격31의수용으로확대하지않는다. 원본6점수·순위는동결이며보충읽기는새점수/순위가아니다. 후속 source 수용은 각 영수증에 연결했다. 원담당 evidence/app의 이번 최종 Release 반영 감사 읽기는 아직 예정이므로 완료로 쓰지 않는다.

## 동결·인계

10월17일KST 같은 실행의 최종 마감에서 이 문서를 동결하고 편집권을 root에 인계한다. 실제 읽은 소스·HTML·과거 metadata·6평가/rotation·각SHA의 CI/로컬재현/preview/운영/복구·최종Release·알림·자정후재조회 경로와 수용 조건은 위 표에 남겼다. 이 담당이 테스트·빌드·브라우저·provider/운영HTTP·Git·CI·배포·복구·메일/광고·계정 작업을 실행한 것은 아니다. 후속 마감에서는 영수증 파일만 읽었다.

기술·제품 Release는 수용하며 추가 필수 제품 수정은 남기지 않는다. 전체 CI archive 전량 비교는 미실행, 전체 권리·기기/음원·Google 계정/심사·문의 실수신·실제 analytics payload는 unknown이다. 남은 것은 원담당의 실제 최종 읽기, root의 관리자 완료 Discord와 최종PR저장/결과SHA 연결이다. root는 실제 마감 영수증을 공용 기록에 추가하며, 이번 인계를17일의 새 개발/릴리스나 계정 승인으로 해석하지 않는다.


## 관리자 최종 저장·송신 확인 · 2026-10-17 KST

위 pending은 각읽기시점이력이다. [원담당두최종수용](runs/resume-20261016-1400-final-supplemental-reviews.json)은실제Release와M3기술/콘텐츠게이트를확인했고,관리자는교체담당의최종Release반영문서와두읽기의4기준근거를대조했다. 원래6평가/점수/완전동률교체이력은불변이다.

문서PR18/mainc99ef7f·보고PR34/mainR2964f7e actualmerged,저장완료Discord38930361073은17일01:14:31.8649811KST actualsent1attempt다. [송신](runs/resume-20261016-1400-completed-discord.json)·[보고2파일차이](runs/resume-20261016-1400-source-report.json)·[보고CI](runs/resume-20261016-1400-report-ci.json)·[최종운영API](runs/resume-20261016-1400-final-production-api.json)를확인했다. 운영/제품/tag는M3이며후속보고커밋을배포하지않았다. 두후속보완PR32/33·최종M3실제원격31을구분한다. 이기록은root의송신후마감이며작업자가명령을실행한것으로바꾸거나Google심사를확정하지않는다.
