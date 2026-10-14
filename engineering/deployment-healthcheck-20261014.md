# 배포 응답의 실제 일관성 검사

2026-10-14 KST · `resume-20261014-2000` · [jpoptoday#15](https://github.com/jpop-today/jpoptoday/issues/15) · RELEASE-001/HARNESS-001 · [ADR-006](../adr/006-reviewed-corrections-and-production.md) · [ADR-007](../adr/007-deployment-response-verification.md).

담당: `resume_2000_harness`. source 기준 `3422391c3f4876cd68e324eeadbb5219fd243959`, docs 기준 `648e5b338b56f292cb652728cdc997860a8db840`. source 브랜치 `codex/15-deployment-healthcheck-20261014`. 소유 파일은 source `scripts/verify-deployment.mjs`, `tests/deployment-healthcheck.test.mjs` 및 이 문서뿐이다. Git·공용 package/lockfile·workflow·빌드·브라우저·실제 배포는 root가 수행한다. 이 문서의 로컬 테스트 성공을 운영 배포 성공으로 읽지 않는다.

## 문제와 수용 기준

v0.3.0 전환의 실제 rollback에서 Cloudflare API canonical 변경 이후 기본 metadata URL과 실제 앱 응답의 전환 시점이 달랐다. 원인은 특정 cache 계층으로 확정하지 않았다. [보존된 실패](../operations/releases/production-rollback-probe-failed-v0.3.0-20261014.json), [원본 복구 HTTP](../operations/releases/production-rollback-http-v0.3.0-20261014.json), [새 앱 복원 HTTP](../operations/releases/production-restore-http-v0.3.0-20261014.json)가 관측 근거다.

새 helper는 이미 검증한 고정 artifact와 실제 origin 응답을 대조한다. API pointer나 한 JSON의 SHA만 맞는 결과를 전체 앱 반영으로 처리하지 않는다. metadata는 매 요청 새 nonce를 붙이고 실제 home/catalog/robots/sitemap/HTML/asset은 query 없는 URL로 검사한다. 실패한 시도와 마지막 결과를 함께 보존하며, 기존 receipt를 덮어쓰지 않는다.

## CLI와 함수

```sh
node scripts/verify-deployment.mjs <https-origin> <verified-dist> <new-receipt.json>
```

성공 exit 0, 응답 불일치·상한 초과·입력/출력 실패 exit 1이다. `EXPECTED_SOURCE_SHA`(없으면 `GITHUB_SHA`)와 `EXPECTED_TARGET`(없으면 `JPOP_BUILD_TARGET`)을 설정하면 local artifact 자체도 해당 값과 일치해야 한다. CLI는 Node24의 `node:http.setGlobalProxyFromEnv()`로 기존 fetch proxy 설정을 적용하고 종료 시 원래 dispatcher/agent를 복원한다. 기존 CA/TLS 검증을 유지하며 HTTPS 오류 무시나 credential 추가 header를 사용하지 않는다. proxy 초기화 실패의 원문에는 주소·비밀값이 있을 수 있어 고정 error code만 출력한다.

모듈은 `verifyDeployment(options)`, `validateDeploymentUrl(value, target)`, `DeploymentVerificationError`를 export한다. `verifyDeployment`에는 `url`, `distPath`, 선택 `outputPath`, `expectedSourceSha`, `expectedTarget` 및 테스트용 `fetchImpl`, `now`, `sleep`을 전달한다. 기본 함수 결과는 성공/실패 receipt다. 잘못된 입력/artifact/출력 경로는 안전한 code의 exception이며 CLI가 동일하게 실패 처리한다. 외부 의존성 없는 모듈과 주입 테스트는 Node22 계약 CI에서도 실행할 수 있다. Node24 CLI transport의 실제 HTTP 실행은 root 검증으로 남긴다.

기본 상한은 6회, 전체 HTTP 시도 120초, 요청과 body 각각을 포함한 요청당 10초, 재시도 간격 2초, 응답 4MiB다. metadata는 추가로 16KiB 상한을 적용한다. 설정은 30회/전체300초/요청30초/간격10초/응답8MiB를 넘길 수 없다. timeout은 AbortController와 독립 Promise timer로 처리하며 응답 stream의 대기·취소가 무기한 실행을 늘리지 않게 한다. 준비 단계의 local artifact 읽기와 receipt fsync는 HTTP 전환 대기 시간 상한의 대상이 아니다.

## 확인하는 응답

- preview는 `jpoptoday-preview.pages.dev` 또는 그 프로젝트의 한 단계 소문자/숫자 subdomain만 허용한다. production은 `jpop.today`와 8자리 소문자 hex의 `*.todayjpop.pages.dev` deployment URL만 허용한다. HTTPS origin 이외의 port·사용자정보·경로·query·fragment와 URL 정규화로 숨겨진 입력을 거부한다.
- local build-info schema2의 정확한 필드, 40자리 source/legacy SHA, 64자리 catalog/ledger/chart hash, `dirty:false`, 현재곡 수와 전체곡 수를 확인한다. local catalog 실제 바이트 digest와 개수를 확인한다.
- 각 시도 앞뒤의 nonce metadata가 local metadata의 모든 필드와 일치해야 한다. ledger/chart digest는 공개되지 않는 원문 대신 검증한 local metadata의 hash와 비교한다. 원문 입력의 semantic hash 및 콘텐츠/정책 검증은 앞선 `verify-dist`의 별도 게이트다. helper만 실행하고 전체 공개 artifact가 안전하다고 선언하지 않는다.
- catalog는 local frozen JSON 바이트, home·robots·sitemap·검색·즐겨찾기·이전 상세 진입·가이드·소개·문의·개인정보·FAQ·범위·차트는 각 local 파일 바이트와 같아야 한다. home의 local `_astro` CSS/JS도 같은 hash로 확인한다. 실제 관측한 Cloudflare 이메일 변환의 제한된 정규화만 다음 계약에 따라 허용한다.
- 기본 상세 표본은 current의 공식확인/비충돌 곡 하나와 억제번호/unknown 곡 하나다. URL은 `encodeURIComponent(id)`, local 파일은 원래 stable ID다. 사용자 지정 `detailPaths`는 최대10개이며 catalog에 실제 존재하는 ID의 canonical encoding만 허용한다. slash/backslash/control/점 경로·부정 percent encoding을 거부한다.
- private staging/script/report 진입과 존재하지 않는 경로는 실제404이고 local `404.html` 바이트와 같아야 한다. preview의 모든 응답에 header noindex가 있어야 하며 production home에 blanket noindex가 있으면 실패한다. index 대상 본문·robots/sitemap의 상세 정책은 검증한 artifact 바이트에 연결한다.
- 모든 요청은 GET·`redirect:error`·`credentials:omit`이다. Accept 외의 앱 요청 header를 만들지 않는다. 3xx 또는 redirect-follow 결과, metadata404, timeout, 잘못된 digest/본문/끝 metadata를 실패 처리한다.

## 실제 Cloudflare 이메일 변환

root의 첫 실제 v0.3.0 baseline helper는6회/41.069초 이후 `/contact/`의 `BODY_MISMATCH`로 실패했다. metadata와 앞선 요청들은 일치했으며 실패 receipt를 보존했다. 원인은 실제 응답에 적용된 Cloudflare 이메일 주소 난독화였다. 이 실패를 성공으로 고쳐 쓰거나 기존 origin 설정을 바꾸지 않았다.

`normalizeCFEmailHTML(actualBytes, expectedBytes)`는 검증한 expected HTML의 정확한 `<a href="mailto:주소">주소</a>` 또는 `<a href="mailto:전체HTML인코딩payload">원래텍스트label</a>`만 기준으로 사용한다. 실제 응답의 정확한 `<a href="/cdn-cgi/l/email-protection#HEX"><span class="__cf_email__" data-cfemail="HEX">[email&#160;protected]</span></a>`에서 두 HEX를 각각 XOR 첫바이트 키로 해독한다. 유효한 UTF8이고 같은 기대주소이며 그 anchor 개수를 넘지 않을 때만 원래 anchor로 바꾼다. 일반 mailto link의 `<a href="/cdn-cgi/l/email-protection#HEX">원래텍스트label</a>` 형태는 해독한 **전체 raw HTML payload**와 원래 label이 정확하게 expected와 같아야 한다. query를 parse하거나 주소만 비교하지 않고 `&amp;`를 `&`로 바꾸거나 subject/body/곡ID를 버리지 않는다. 두 형태는 같은 expected 발생 횟수 예산을 사용한다. 정확한 `<script data-cfasync="false" src="/cdn-cgi/scripts/5c5dd728/cloudflare-static/email-decode.min.js"></script>` 하나를 제거한 결과 전체 바이트가 expected와 같아야 성공이다. 다른 형태·속성·주소·중복 anchor·unknown script·다른 본문 변경은 허용하지 않는다. 이 정규화는 `BODY_MISMATCH`의 HTML에만 적용되며 JSON/CSS/JS/robots/sitemap에는 적용하지 않는다.

root가 저장한 실제 응답 파일과 frozen v0.3.0 contact를 이 함수로 로컬 대조해 PASS했다. raw SHA256은 `76851083c98e6e266da4087fd44c7c834306c2bda3c6a91256af58d16623a65b`, normalized/expected SHA256은 `5d461ba338a5ff06722606edc45d63b7d9b99e240e22a5fa0f79747ade280ebd`, 적용 anchor1/decoder1이었다. 실제 원문 HTML·동적 HEX는 커밋하지 않았고 fixture는 합성 주소로 작성했다. 이 보완 후 새 실제 HTTP 전체 결과는 root가 별도로 검증한다.

span 보완 후 root의 두 번째 baseline은 contact/privacy를 통과했지만 Butter-Fly 상세의 query mailto link에서6회/49.006초 뒤 실패했다. [첫 실패](../operations/runs/resume-20261014-2000-healthcheck-failed-email-transform.json)와 [상세 실패](../operations/runs/resume-20261014-2000-healthcheck-failed-detail-transform.json)를 보존한다. 실제 해독 payload는 expected의 `mailto:`를 제외한 전체 raw HTML href와 같았다. 위 일반 mailto 형태를 추가하고 root가 저장한 상세 HTML과 frozen 상세를 로컬 대조해 PASS했다. raw SHA256은 `5128030b3b20b62245b941568ed1643236afe44169e6237980601994019eead2`, normalized/expected SHA256은 `55b0786f7d987dd2196e6df58ada5a6104cfc2683dbc32bc761453d03d97140c`, anchor1/mailtoAnchor1/decoder1이다. 이 두 단계의 로컬 보완을 전체 실제 HTTP 재검증 성공으로 승격하지 않는다.

## 안전한 증거와 마지막 정상 결과

receipt는 origin, local source/target/세 hash, 실제 시작/확인 시각, 상한, 각 시도의 고정 request path·HTTP status·바이트 수·SHA256·성공 여부·error code를 담는다. 승인한 이메일 변환에는 `rawSha256`, `normalizedSha256`, 종류와 anchor/decoder 개수를 추가하고 일반 mailto 형태가 있으면 `mailtoAnchors`도 기록한다. metadata query 값, 응답 원문, response header, 원문 error, auth 값, credential 환경은 저장하지 않는다. 본문/hash 대조 실패는 해당 request check도 `passed:false`로 표시하며 `expectedSha256`을 남긴다. 이후 재시도가 통과해도 앞선 실패와 code는 attempt 이력에 남고 최종 code만 null로 정리한다.

출력은 같은 디렉터리의 임시 파일을 exclusive 생성하고 write/fsync/close한 뒤 exclusive hard link로 완성된 receipt를 게시한다. 처음부터 출력이 있으면 네트워크 요청 전에 멈추며, 검사 중 다른 writer가 출력한 파일도 교체하지 않는다. 임시 파일은 자기 생성 파일만 정리한다. 마지막 성공 파일의 바이트를 기존 결과의 존재만으로 지우거나 실패 JSON으로 바꾸지 않는다. 실행별 새 output 이름은 root가 정한다.

실제 origin 한 번의 관측은 모든 CDN 지점의 동시 전환이나 Google 승인 보장이 아니다. schema2가 없는 예전 개인 원본은 이 helper의 성공 대상으로 추정하지 않는다. 그 복구는 별도 고정 원본 바이트/404 증거로 확인한다. schema2의 v0.3.0과 새 결과 간 복구는 각 target의 별도 고정 dist로 검사할 수 있다.

## 실제 로컬 검증과 인계

Node24.19.0에서 `node --test tests/deployment-healthcheck.test.mjs`를 실행해 최종70 tests PASS, 실패/skip0을 확인했다. 최초40 테스트 때 sandbox의 기본 subprocess 실행은 파일 단위 wrapper1만 보고하여 직접 `node tests/deployment-healthcheck.test.mjs`로 개별 집계를 확인했고, 지원된 추가 network permission으로 같은 `node --test`를 실행하여 개별 집계를 다시 확인했다. 실제 외부 HTTP를 테스트가 요청한 것은 아니다.

검증은 stale 뒤 정상 재시도·끝 metadata 전환·SHA/target/legacy/ledger/chart/dirty/schema 불일치·catalog/home/robots/sitemap/상세/asset/404 혼합·preview header 경계·production blanket noindex·redirect/404/500·원문 secret 포함 error·신호를 무시하는 client timeout·끝나지 않는 body timeout/cancel·전체 deadline·Content-Length/실제 stream oversize·이전 결과 보존·동시 writer·symlink·경로 조작·설정 상한을 포함한다. Unicode/일본어/literal% stable ID와 실제 frozen staging→reviewed projection의3317ID/current3313도 확인했다. 이메일 span 보완은 서로 다른 XOR 키/zero 키·raw/normalized receipt·틀린 주소/홀수HEX/잘못된UTF8/중복 anchor·속성/본문/script secret 주입·누락/중복/미확인 decoder 거부17개를 추가했다. query mailto 보완은 원래 fullpayload/label·두 형태의 예산 공유·변경 subject/body/곡ID·entity decode/순서 변경/누락 query/다른label 거부·extraattrs/HTML inject·receipt 보존13개를 추가했다.

root는 동일 SHA CI 및 target별 `verify-dist`를 먼저 통과시키고 새 결과의 preview/production HTTP, 브라우저 회귀, 실제 rollback→복원, workflow 연결을 검증한다. 담당자는 Git·빌드·배포·실제 복구·Release·Discord를 실행하지 않았다. 두 동료의 실제 파일 리뷰와 후속 수정은 root가 통합 기록에 연결한다.

## root의 최종 로컬·현재 운영 baseline 검증

root가 새 working tree로 실행한 `/tmp/jpop-v031-unit-final-query.log`를 직접 읽어 전체274 tests PASS·실패0·skip0을 확인했다. 하네스70개와 기존204개를 함께 실행한 결과이며 clean commit의 CI 결과와 구분한다.

root는 Node24 기본 fetch/기존 proxy·CA로 `https://jpop.today`의 현재 v0.3.0을 frozen production artifact와 검사했다. `/tmp/jpop-v031-healthcheck-baseline-complete.json`을 직접 읽었으며 실제 시작 `2026-10-14T11:29:20.999Z`, 완료 `11:29:56.681Z`(한국 시간20:29:56), 1attempt·25 HTTP checks·35.682초·실패0·최종 `passed:true/code:null`이었다. catalog·HTML/robots/sitemap·선택 상세·local asset·private404와 앞뒤 metadata가 통과했고, contact/privacy의 span형2곳과 Butter-Fly/Don't Say Lazy 상세의 일반 mailto형2곳이 제한된 정규화 후 artifact hash와 일치했다. receipt는 각 원문 hash·정규화 hash·expected hash·적용 형태/개수를 보존한다.

이 실제 HTTP 대상의 source는 **`b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8`의 v0.3.0 production**이다. 실행한 helper 코드는 **base `3422391…` 이후 미커밋 새 하네스**이며 이 성공은 v0.3.1 운영 배포 성공이 아니다. 처음두 실패의6회/41.069초 및6회/49.006초 기록도 그대로 보존한다. clean SHA PR·CI·신규 동일 SHA preview/production·실제 rollback은 root의 다음 게이트이고, 현재 baseline 관측을 모든 CDN 지점 동시 전환이나 AdSense 승인으로 해석하지 않는다. 최종 receipt의 원격 영속 보존·SHA 연결은 root가 담당한다.

## 하네스 담당의 읽기 전용 동료 평가

`2026-10-14T20:24:41+09:00`에 실제 파일·root의 로컬 로그·캡처를 읽고 평가했다. 자기 하네스 산출물은 평가하지 않았고 다른 담당 파일을 수정하지 않았다. source 기준은 `3422391…`의 미커밋 변경이며 clean SHA CI·preview/live는 이 평가 시점에 pending이다. 서버 port 충돌로 중단된 root 테스트 시작은 worker 코드 결함의 근거로 사용하지 않았다.

| 대상 / 항목 | 0..5 | 읽은 근거와 판단 |
| --- | ---: | --- |
| 콘텐츠 / 정확성 | 5 | `src/content/guides.ts`·docs `content/number-checking-guide-20261014.md`를 읽고 ledger의 Butter-Fly 두 가수/네 번호·movie confirm/original suppress·튜링 러브 KY 반증/TJ confirm을 독립 Node 읽기로 대조했다. 네 공식 URL/checkedAt는 실제 ledger와 정확히 일치했다. 기존4가이드와 mapper를 base Git 읽기 결과와 바이트 비교해 그대로 보존한 것을 확인했다. unknown≠미수록·실제 번호 확인일과 편집일 구분·권리/심사 미확인 경계를 충족한다. |
| 콘텐츠 / 근거·검증 | 5 | root 템플릿 `src/pages/guides/[slug].astro`와 `e2e/guide.spec.ts`를 읽었다. 실제 `/tmp/jpop-v031-preview-e2e.log`의19PASS/20.7초, `/tmp/jpop-v031-production-e2e-fresh.log`의19PASS/21.4초 중 가이드 두 흐름 통과를 확인했다. `guide-mobile.png`를 직접 열어360px 제목·날짜·첫 단계를 읽었고 `evidence.json`의overflow false/errors[]를 확인했다. CI·운영 반영으로 부풀리지 않았다. |
| 콘텐츠 / 사용자 가치 | 5 | 업체/기기 → 가수 → 버전 → 근거/확인일 → unknown → 제보 순서가 실제 번호 선택에 연결된다. 같은 제목·다른 가수, 같은 가수·다른 버전, 다른 곡의 오번호라는 세 사례가 기존 정의형 가이드에 구체적인 판단 근거를 더한다. 불필요한 개인정보/가사/이미지를 요구하지 않는다. |
| 콘텐츠 / 유지보수성 | 4 | 기존 정적 sections·편집 표시를 유지하고 optional sources만 추가했다. 다만 dated 사례 본문·sources·별도 docs의 번호/시각은 수정할 때 ledger를 다시 대조해야 하며 E2E 자체는 모든 본문 주장/네 URL/시각을 ledger와 동시 검증하는 계약 테스트가 아니다. 현재 독립 대조는 모두 정확하고 이 제한은 차단 결함이 아니다. |
| UI / 정확성 | 5 | `src/pages/coverage.astro`, `e2e/coverage.spec.ts`, docs `engineering/coverage-ui-20261014.md`를 읽고 공개 projection을 독립 계산했다. playlist3299, TJ12/3123/164, KY10/2481/808, 목록10ID/18번호가 맞으며 차트18을 분모에서 제외한다. 표시 번호는 공식 근거+비충돌 조건으로 제한하고 corrected51714·suppressed57803 및 안정 상세 주소/문자열 번호 계약을 보존한다. |
| UI / 근거·검증 | 5 | 두 실제 E2E 로그의 coverage 세 흐름 PASS를 확인했다. `coverage-desktop.png`1440×1100과 `coverage-mobile.png`360×800을 직접 열어 상태 표·summary·이동 링크·원본 브랜드를 확인했고 overflow false/errors[] 기록도 읽었다. JS-off keyboard/detail 이동과 exact 브랜드 palette는 E2E에 연결되며 로컬 dirty 증거와 원격 검증을 구분한다. |
| UI / 사용자 가치 | 5 | 공식 대조/관측/확인 필요를 같은 분모로 비교하고 실제 근거가 있는10곡부터 상세로 이동할 수 있다. ID와 업체별 번호 건수·충돌·보류의 차이를 설명하고 시각과 공식 링크를 번호 옆에 보여 재확인에 도움이 된다. 모바일 한 열 배치와44px 링크, JS 없는 사용 흐름이 현재 제품 목적에 맞는다. |
| UI / 유지보수성 | 4 | 기존 `indexableSong`·`songHref`·provider evidence와 공용 스타일을 재사용하고 CSS를 페이지 범위로 제한했다. 빌드 집계는 동적이지만 고정 E2E 수치와 docs 기준선은 향후 ledger 변경 때 함께 갱신해야 한다. 그 갱신 기준이 문서에 있어 현재 차단 결함은 아니다. |

콘텐츠19/20, UI19/20이다. 두 대상 모두 할당된 로컬 수용 기준을 충족했고 이 읽기 평가에서 병합을 막는 오류는 발견하지 않았다. 전체6평가 합산·최저 담당 선택·교체·실제 운영 게이트는 root가 별도로 수행한다. 변경량·말수·다른 동료의 점수를 기준으로 점수를 맞추지 않았다.
