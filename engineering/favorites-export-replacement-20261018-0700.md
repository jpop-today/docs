# 즐겨찾기 번호 내보내기 교체 감사 · 2026-10-18 07시 재개

- Issue: [jpoptoday#38](https://github.com/jpop-today/jpoptoday/issues/38), run `resume-20261018-0700`, source [PR39](https://github.com/jpop-today/jpoptoday/pull/39).
- 스펙: [FAVORITES-001/DATA-001/CONTENT-001/DESIGN-001](../product/spec.md#즐겨찾기-번호-목록의-상태-보존--issue38-2026-10-18).
- 담당: 실제 collaboration `/root/resume38_replacement`. 읽은 source H1 `1ca336de00d60ac95d426165f5f26d02b2b454df`, docs base `dbc3196441c01368d621c6b65b29c1ddede1a1e2`, branch `codex/38-favorites-export-status`는 root 인계 기준이다. 이 담당은 Git을 실행하지 않았다.
- 소유: 품질 인계 문서의 링크 정정·후속 구획, 이 감사 문서, root가 추가 허용한 평가시점 품질 원문 스냅샷. 소스·공용 계약·package·lock·CI·서버·브라우저·배포·새 worker 생성은 수행하지 않는다.
- 재개 후 읽기: 2026-10-18 02:56 UTC부터. 같은 Issue·담당 역할을 이어간 것이며 사용량 중단을 새 팀이나 새 개발 실행으로 계산하지 않는다. root가 알린 `/tmp` 소실 때문에 과거 원시 로그·실행 가능한 artifact가 지금 존재한다고 주장하지 않는다.

## 교체 이유와 추적 링크 보완

[원 6개 평가](../operations/runs/resume-20261018-0700-reviews.json)와 [앱](../operations/runs/resume-20261018-0700-reviews-app.json)·[자료](../operations/runs/resume-20261018-0700-reviews-evidence.json)·[품질](../operations/runs/resume-20261018-0700-reviews-quality.json)의 실제 판단 근거를 모두 읽었다. root 집계는 자료40·앱40·품질38이다. 품질에 대한 두 평가가 첫 스펙 fragment의 날짜 누락을 각각 유지보수성4로 지적했고 나머지 세 기준은5다. 나머지 두 담당의40점 동률과 새 배포 대기는 별개다. 교체 담당은 원 점수·순위를 재작성하거나 변경량으로 비교하지 않았다.

편집 전 품질 문서의 바이트를 [평가시점 읽기 스냅샷](favorites-export-quality-20261018-0700-reviewed-snapshot.md)에 복사하고 SHA256 `429d914a5c135383960db811773c1b866e7609355473124b8e3bcbdf88804ffe` 일치를 직접 확인했다. 세 개별 평가 JSON도 집계 문서에 적힌 원 SHA256과 일치한다. 원 스냅샷은 당시 잘못된 링크·초기 실패·미실행 표현까지 보존한다. 작업 중인 품질 문서에서만 실제 spec 제목을 계산한 fragment `#즐겨찾기-번호-목록의-상태-보존--issue38-2026-10-18`로 정정하고 별도 후속 구획을 더했다. 파일 존재와 제목에서 산출한 fragment 일치를 검사하며, 원격 GitHub 화면 클릭 검증으로 확대하지 않는다.

root의 평가 영수증은 품질 담당 active roster 제외·실제 interrupt(이전 상태 completed)·이 교체 담당 spawn을 기록한다. 물리 삭제 API나 삭제 결과는 없으며 원 코드·평가·읽기 스냅샷을 보존한다.

## 소스와 독립 검증 계약 재감사

| 읽은 범위 | 코드에서 확인한 수용 근거 | 경계 |
| --- | --- | --- |
| `contracts/favorites-export.mjs`, `public-catalog.mjs`, `provider-lookup.mjs` | 공식 표시는 기존 `hasConfirmedNumber`의 current/playlist·provider-confirmed·conflict:false·실제 증거필드·일치하는 엄격 공식 numeric URL만 재사용한다. 업체별로 따로 판정한다. | formatter가 제목·가수·버전의 실제 일치나 제공사 조회일을 새로 검토하지 않는다. ledger의 검토 책임을 대신하지 않는다. |
| 관측/미확인·확인 필요 분기 | observed-unverified·conflict:false·숫자 문자열만 관측으로 내보낸다. `0`, 긴 숫자, 선행0을 관측 계약대로 보존한다. malformed confirmed는 관측으로 강등해 번호를 드러내지 않는다. unknown/null/suppress/충돌은 번호 없는 확인 필요며 `previousNumber`도 읽지 않는다. | 확인 필요를 미수록으로 설명하지 않는다. 한 업체의 확인을 다른 업체에 적용하지 않는다. |
| `src/scripts/app.ts`, `favorites.astro` | 복사에 화면 slice가 아닌 `filtered` 전체를 넘긴다. 빈 결과는 clipboard 호출 전 반환한다. 기존 쓰기/미해결 ID/URL/정렬·clipboard 거부 모달 경로를 재사용하고 안내의 `aria-describedby`와 JS-off 비활성·설명을 유지한다. | 읽기 감사다. 실제 OS clipboard 권한·다른 기기의 저장 상태를 확인한 것은 아니다. |
| `tests/favorites-export.test.mjs` | 대상 입력에는 기존 projection을 쓰지만 기대값은 raw+ledger+정정 후 번호 빈도로 계산해 formatter·확인 predicate·검색 matcher를 호출하지 않는다. 전3317곡 exact 문자열, 실제 충돌의 비어 있지 않음, SAM/Ado의 업체별 보류, original-ver suppress, synthetic 부정 URL/증거/선행0·deep-freeze·저장 bytes 보존 반례를 둔다. | 동결자료 oracle은 새 공식 조회나 사람의 제목·가수·버전 검토가 아니다. 합성 URL/날짜를 실제 근거로 기록하지 않는다. |
| `e2e/favorites-export.spec.ts`, 기존 `e2e/app.spec.ts` bulk assertion | raw+ledger 독립 기대값으로 clipboard 성공/거부·전체 선택·키보드 복귀·360px/44px 양축·>30/page2 전체·업체 AND·빈목록/미해결 ID·저장 원문을 검사한다. JS-off는 실제 비활성 context의 `noscript > p` 전체문구와 가시성을 요구한다. 기존 suppress/미수록 비추정 assertions도 유지한다. | 교체 담당은 테스트·브라우저를 재실행하지 않았다. 아래 실제 root 영수증과 구분한다. |

[동결 입력 영수증](../operations/runs/resume-20261018-0700-frozen-inputs.json)과 현재 파일을 직접 SHA256 비교했다. staging `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3`, ledger84 `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1`, chart `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97`, identity `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8`, global CSS `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889`는 모두 일치한다. 이는 마지막 정상 자료·3317 ID·기존 브랜드 자료의 보존 확인이며, 수집 장애를 새로 유발해 복구한 증거가 아니다.

formatter SHA256 `025a1dc15b76c9a05ad3864295a2049fe16ae206a912b5befb3c3bdd8466f26d`, unit `bfb6f82101427f93e1e043d1ba4c302f5824bb6c97797d553b7f0437eae5aa24`, E2E `f9a1320e4e2f86bd1ba9cb3509d0cc45484b91f58ec14cf1becc8f251c9fa97e`도 두 원 리뷰가 읽은 파일과 일치했다. 이 감사에서 소스 차단 결함은 찾지 못했다. 추가 제품 수정이나 새 제공사 조회를 만들지 않았다.

## 실제 실행 영수증과 아직 수용하지 않은 단계

| 증거 | 직접 읽은 사실 |
| --- | --- |
| [v0.4.5 기준선 내보내기](../operations/runs/resume-20261018-0700-baseline-export.json) | root는 Ado TJ확인/KY관측인데 상태 없이 `逆光 · Ado \| TJ 68868 \| 금영 76509`로 복사되는 문제를 실제 재현했고 저장 원문은 보존됐다. 신규 구현의 통과로 계산하지 않는다. |
| [working tree 단위/타입](../operations/runs/resume-20261018-0700-local-checks.json) | root의332/332·26files 타입0. result SHA 전 관측이며 특정 clean M 검증으로 확대하지 않는다. |
| [초기 전체 브라우저](../operations/runs/resume-20261018-0700-head-browser.json)·[JS-off 관측](../operations/runs/resume-20261018-0700-js-off-observed.json) | H `6edc05c512f999f38c07c1ba0d3e38892198a133`의42pass/2fail을 보존한다. 실제 JS-off childVisible/bodyIncludesNotice=true를 보고 parent 선택을 고쳤고, 별도 setlist 날짜 회귀는 root 소유로 보완했다. |
| [수정 focused 검증](../operations/runs/resume-20261018-0700-fixed-browser.json) | H1 `1ca336de00d60ac95d426165f5f26d02b2b454df`, preview build/verify exit0, favorites5+guide2 합계7pass/9.1초. 첫 실패를 덮어쓰지 않는다. |
| [당시 head CI](../operations/runs/resume-20261018-0700-head-ci-final.json)·[push CI 실제 결과줄](../operations/runs/resume-20261018-0700-head-ci-logs.json) | H1 app push39074009981 및 contracts push39074010008 성공. 앱 결과줄332/332·타입0·preview44/37.8초, production artifact44/53.3초를 읽었다. 당시 PR contracts39074014287 성공, PR app39074014264 진행 중이었다. 이 과거 영수증만으로 현재 PR 완료를 추정하지 않는다. |
| [02:56 UTC 재개 CI 조회](../operations/runs/resume-20261018-0700-resume0255-ci.json) | 같은 H1의 push/PR app·contracts 4개가 모두 completed/success임을 root가 새로 조회한 영수증에서 확인했다. 과거 진행 중 기록은 보존하며, 아직 최종 M의 main CI 결과는 아니다. |
| [02:59 UTC 운영 재확인](../operations/runs/resume-20261018-0700-resume0255-http.json) | root의 실제 `https://jpop.today`27검사/1시도 PASS다. sourceSha는 기존 v0.4.5 `ed22f924536d28fa990d7688b6ba352446984140`, catalog/ledger/chart hash도 해당 기존 artifact와 일치한다. 새 Issue38 운영 배포나 새 동작 성공으로 계산하지 않는다. |

## M 통합·전용 preview 후속 읽기

root가 PR39 병합으로 인계한 최종 M은 `110c583ec3460b4ca5e3b153deef7278d856f8a3`다. 아래 JSON을 직접 읽고 sourceSha·target·세 metadata hash의 동일성을 파일 기반 독립 대조로 확인했다. 이 담당이 CI·빌드·HTTP·브라우저를 실행한 결과는 아니다.

| 증거 | 직접 확인한 사실 |
| --- | --- |
| [M main CI](../operations/runs/resume-20261018-0700-main-ci.json)·[실제 결과줄](../operations/runs/resume-20261018-0700-main-ci-logs.json) | app39091961070·contracts39091961101은 같은 M push에서 success다. canonical332/332·타입0/0/0·preview44/43.4초·production44/54.2초 결과줄을 확인했다. |
| [M 재현](../operations/runs/resume-20261018-0700-reproducibility.json) | root가 target별 두 번 clean build의 전체 파일 집합/매 파일 SHA256을 비교해 각각3347파일 동일을 확인했다. HTML3336, preview 색인0/production49, dirty:false다. 해당 M의 로컬332/332·26files 타입0도 별도 기록돼 있다. CI archive 전체와 로컬 전체의 바이트 비교는 수행하지 않았다고 명시돼 있다. |
| [보존 재확인](../operations/runs/resume-20261018-0700-main-preservation.json) | 동결5입력·CSS/원3평가/원 품질 스냅샷 hash와40/40/38점 보존을 root가 확인했다. 교체 담당도 formatter/unit/E2E 파일 hash를 다시 읽어 원 리뷰와 일치함을 확인했다. |
| [이전 정상 artifact 회수](../operations/runs/resume-20261018-0700-old-artifact-recovered.json) | `/tmp` 소실 후 기존 v0.4.5 CI run39058025419의 artifact12110531488을 회수했다.3347파일·old source ed22f924·production/dirty:false와 입력hash를 기록한다. 과거 SHA를 새로 빌드한 복구물은 아니다. 실제 rollback 실행 증거는 아직 아니다. |
| [전용 preview API](../operations/runs/resume-20261018-0700-preview-deployment.json)·[엄격 HTTP](../operations/runs/resume-20261018-0700-preview-http.json) | `jpoptoday-preview` deployment `cf872f59-4a1d-42bd-b05b-f9c410bc9388`, 같은 M, 2026-10-18T03:01:10.636946Z 생성. 실제 URL의 strict27검사/1시도 PASS다. 시작/끝 metadata 검사2개와 본문·asset·비공개 경로25개의 기대 artifact hash 검사를 구분해 읽었다. |
| [preview 원격 workflow dispatch](../operations/runs/resume-20261018-0700-preview-dispatch.json) | `deployed-preview.yml`에 같은 M과 위 전용 URL을 전달한 exit0만 확인했다. workflow 원격44의 완료 증거는 아직 읽지 않았다. |

재현된 preview/production 및 실제 preview HTTP의 catalog SHA256은 `97c906d17795a94be30a546332366b819f2bc6e0eded600ce75a9643304d4123`, ledger `5275c44ef1cd30a57b0cf633be33be16d5be500ed8ee4446012cda54be939370`, chart `971bacf9cb65e5b7176e8a5aa59394d2c46ce00d0676aa033178dc11ae7540b9`로 모두 일치했다. sourceSha가 바뀌어도 입력값은 기존 v0.4.5와 같아 번호·검토 근거를 새로 늘린 변경으로 계산하지 않는다. preview 전체 noindex와 production49 색인 범위는 각 target의 artifact 검증 관측이며, 실제 광고 노출·Google 심사 결과를 검사한 것은 아니다.

## 실제 원격 preview·운영 후속 읽기

[원격 preview workflow](../operations/runs/resume-20261018-0700-preview-remote.json) run39092193863/job117571108562는 같은 M에서 completed/success다. [실제 결과줄](../operations/runs/resume-20261018-0700-preview-remote-logs.json)의 `Verify actual Pages behavior`44pass/1.2분을 직접 읽어 앞 절의 dispatch 대기를 해소한다.

[운영 API 영수증](../operations/runs/resume-20261018-0700-production-deployment.json)은 `todayjpop`/`jpop.today`의 기존 v0.4.5 deployment a268291a에서 신규 `bff966cf-c7af-41cf-b406-21cd007b28de`로 전환했음을 기록한다. 신규 생성시각은 2026-10-18T03:05:38.53579Z, sourceSha는 같은 M이다. 개인 Git metadata kim75503/todayjpop/dev와 자동 production false/preview none은 보존돼 있다. [실제 운영 HTTP](../operations/runs/resume-20261018-0700-production-http.json)는 같은 M·production·동일 catalog/ledger/chart hash로27검사/1시도 PASS다. metadata2개와 기대 artifact hash25개를 분리해 대조했고 모든 passed 및 실제/정규화 hash 일치를 확인했다.

[운영 검증 workflow](../operations/runs/resume-20261018-0700-production-remote.json) run39092435736/job117571818772와 [실제 결과줄](../operations/runs/resume-20261018-0700-production-remote-logs.json)은 같은 M의332/332·타입0/0/0·로컬44/41.9초와 **실제 production44/59.0초** 성공을 기록한다. `Check Actions deployment bindings`의 실제 줄은 Cloudflare Actions binding missing/deployment skipped이고 deploy step도 skipped다. root의 managed 배포와 workflow의 verify-only 검증 성공을 구분하며 자동 배포 binding이 설정됐다고 쓰지 않는다.

## 실제 rollback·재적용 후속 읽기

[복구 API 기록](../operations/runs/resume-20261018-0700-recovery.json)의 실제 실행은 2026-10-18T03:10:46.371212Z부터03:11:03.223826Z까지다. `todayjpop`이 신규 bff966cf/M에서 이전 정상 a268291a/ed22f924로 되돌아간 뒤 같은 신규 bff966cf/M으로 돌아왔고 두 API step·최종 success가 true다. 복원은 finally에서 시도하도록 기록돼 있으며 리소스 삭제는 없다.

[이전 v0.4.5 HTTP](../operations/runs/resume-20261018-0700-rollback-http.json)는 old source `ed22f924536d28fa990d7688b6ba352446984140`에서27검사/1시도 PASS, [신규 재적용 HTTP](../operations/runs/resume-20261018-0700-restore-http.json)는 최종 M에서27검사/1시도 PASS다. API deployment ID·각 sourceSha·target production을 직접 대조했고, 두 HTTP의 metadata2개/기대 artifact hash25개와 모든 passed 및 실제/정규화 hash 일치를 읽었다. 두 버전이 같은 자료 입력hash를 유지한다는 사실을 버전 전환 확인의 대용으로 쓰지 않았다. 이전 코드의 자체 재빌드나 문서상 복구계획을 실제 rollback으로 계산한 것이 아니다.

## 최종 post-restore·Release 읽기와 인계

[추가 post-restore 엄격 HTTP](../operations/runs/resume-20261018-0700-postrestore-http.json)는 2026-10-18T03:11:47.343Z에 같은 M·production·동일한 세 자료hash로27검사/1시도 PASS다. 각 passed·metadata2개/기대 artifact hash25개도 직접 대조했다. [일반 URL 관측](../operations/runs/resume-20261018-0700-postrestore-site.json)은 03:13:26.846Z에 쿼리 없는 `/build-info.json`→`/favorites/`→`/guides/build-your-setlist/`→`/catalog.json`→`/build-info.json`5회 모두200/PASS를 기록하며 catalog hash도 일치한다. 이 영수증의 범위는 해당 응답5개이며 전 CDN 위치·지속 가동 보장으로 확대하지 않는다.

[Release API 영수증](../operations/runs/resume-20261018-0700-release.json)은 실제 published `v0.4.6`, release ID416205945, 2026-10-18T03:13:14Z, draft:false/prerelease:false다. annotated tag object `5fee402924df9f97c5635e74910fb08709b35996`와 제품 M의 연결이 기록돼 있다. [Release workflow 실제 결과줄](../operations/runs/resume-20261018-0700-release-logs.json)은 run39092755809의332/332·타입0/0/0·브라우저44/32.7초와 publish job117573022406의 **03:13:15.0578566Z Discord sent/1attempt**를 확인한다. 이는 Release 알림이며 관리자 완료 보고 송신과 구분한다. 이 담당은 webhook·시크릿 값을 읽거나 송신하지 않았다.

앞 절들의 대기는 각 증거를 아직 읽지 않았던 시점의 범위다. 최종 읽기에서 같은 M의 main CI→두 target 로컬 전체재현→실제 preview44→managed 운영 배포/엄격27→실제 production44→old 복구27/1→M 재적용27/1→추가 엄격27/일반5→실제 published Release의 연결을 수용했다. 원 six peer 점수·원 실패42/2·working tree와 clean H1/M의 구분·사용량 중단/`/tmp` 소실·CI archive 전체 비교 미실행·Actions binding missing은 그대로 남는다.

교체 담당의 완료 범위는 지정 문서 보완과 영수증의 독립 읽기 감사다. Git·제품 소스/테스트·CI·HTTP·브라우저·배포·새 외부 조회·새 worker 실행을 수행하지 않았고, root와 두 원 담당의 후속 수용 판단을 대신하지 않는다. AdSense는 기술 준비와 Google 계정/신청/실제 심사를 분리하며 권리·실제 광고 노출·승인을 확정하지 않는다. 이 문서는 이 지점에서 후속 동료 읽기를 위해 동결한다.
