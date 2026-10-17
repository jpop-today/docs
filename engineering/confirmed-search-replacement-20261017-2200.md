# Issue35 교체 담당의 독립 읽기 감사

작업은 [Issue35](https://github.com/jpop-today/jpoptoday/issues/35), run `resume-20261017-2200`이다. 담당 `/root/resume35_replacement`의 소유 파일은 이 문서 하나이며 제품 소스·테스트·Git·package/lockfile·CI·dist/서버·배포를 변경하지 않았다. 수용 기준은 [업체별 공식 확인곡 검색 계약](../product/spec.md#업체별-공식-확인곡-검색--issue35-2026-10-17)의 현재 행 구조/선택자·근거 판정·회귀와 배포 전 증거의 정합성을 독립적으로 읽고, 실증 결함만 관리자에게 전달하는 것이다.

## 감사 기준과 교체 경계

2026-10-17T18:15:03Z(10월18일03:15:03 KST)에 실제 읽기 확인한 source HEAD는 `ae1332d64912f4a7caf9eeb939f73b550c12369c`이고 source status는 clean이다. 작업 시작 source base `2964f7ea3f8c43f147422107f40629f51c010698`, docs base `d679cf57592b285a30d762784fdb62ae0bf73581` 및 기존 변경을 보존했다. docs의 기존 미커밋 파일은 다른 담당/root 소유이며 수정하지 않았다. 세 저장소 AGENTS와 product/spec·operations/status·roadmap·daily-development·continuous-development를 읽었다. 최신 GitHub Issue35 읽기는 성공했고 당시 open이었다.

실제 `environment_status`는 revision **459/current/running**, network unrestricted/enforced, Cloudflare secret/runtime binding ready였다. runtime/networking skill과 현재 network-policy를 읽었고 VPN configured는 false였다. 환경 준비 관측은 API 변경 권한이나 배포 성공을 뜻하지 않는다. 비밀 값·전체 환경·credential 파일을 읽거나 기록하지 않았다.

[실제 여섯 평가와 관리자 확인](../operations/runs/resume-20261017-2200-reviews.json)을 전부 읽었다. 인계 담당 합계는 evidence40/app39/quality40이며 다른 두 담당은 동률이다. 앱 담당의 유지보수성에 한 리뷰어가4, 다른 리뷰어가5를 주었고,4의 근거는 아래의 실제 위치 기반 CSS 결합이다. correctness 차단이나 변경량/말 수의 순위가 아니다. 관리자가 앱 담당을 active roster에서 제외·interrupt하고 이 교체 감사를 배정했으며, 원 점수·이유를 수정하지 않았다. 이 문서는 작업자 기록의 물리 삭제나 원 소스 이력 삭제를 주장하지 않는다.

## 현재 모바일 선택자의 정합성

소스 `src/components/Search.astro:28–38`의600px 이하 규칙은 `[data-list-view]` 안의 번호 셀에만 적용된다.32행은 두 열 grid와 각 업체 셀의 절반 폭,33행의 `.number-cell:nth-of-type(3)`는 금영 셀의 오른쪽 배치,34–37행은 업체명/배지 nowrap·번호44px·unknown 최소 높이를 정한다.38행은 저장 버튼44×44px다. `global.css`/차트 전체의 변경으로 확대하지 않았다.

두 생성 경로를 직접 대조했다.

| 경로 | 실제 구조 |33행 선택 결과 |
| --- | --- | --- |
| `src/components/SongRow.astro:6–10`의 SSR | `li` 안에 제목 `div` 하나, TJ `div`, KY `div`, 저장 `button` 순서다. | 같은 태그인 `div`의 세 번째 요소가 KY다. 저장 `button`은 div 순번을 늘리지 않는다. |
| `src/scripts/app.ts:150–166`의 JS 행 | 제목 `div`를 먼저 append하고 `['tj','ky']` 순서의 `div` 두 개, 저장 `button`을 append한다. | SSR과 동일하게 KY만 선택한다. |

현재 구조에서 잘못된 업체를 배치하는 실증 결함은 발견하지 못했다. 다만 제목/업체 사이에 새 `div`를 넣거나 provider 순서를 바꾸면 위치 선택자의 의미가 달라질 수 있다. 향후 행 템플릿 변경 시 **두 생성 경로와 Search33행을 함께 검토**하고 아래 회귀를 실행해야 한다. 이번 감사에서는 수용 기준을 충족한 소스에 근거 없는 재구현을 추가하지 않았다.

`e2e/confirmed-search.spec.ts:37–47`의 더보기는 동결 staging 곡 수로 유한 상한을 두고 클릭마다 실제 행 수 증가를 기다린다.216–265행의 모바일 회귀는 home/search/favorites의 `allVisibleIds`를 독립 TJ 집합/저장 ID와 대조한다.250–252행은 **비어 있지 않은 행·정확한 행 수·정확한 ID집합**을 먼저 요구하므로 빈 목록의 geometry 검사가 거짓 성공하지 않는다.263행은 JS-off SSR5행을 요구한다.221–244행은 번호/unknown/라벨/배지의 행 밖 이탈·서로 겹침·배지 여러 줄과 번호 복사 버튼의 **폭·높이 각각44px**를 확인하고 document 전체 가로 넘침도 검사한다.189–208행은 조건 버튼의 양축44px·focus·Enter/Space와 원래 브랜드 색을 별도로 검사한다.

[수정 후 실제 모바일 PNG](design/confirmed-search-20261017-2200/quality-fixed-confirmed-search-mobile-provider-rows.png)를 직접 열었다. 화면에 TJ 공식 확인32곡, 업체별 두 번호와 각각 한 줄 배지, 저장 버튼이 분리되어 보였다. 이는 이전 담당이 생성한 로컬 화면의 독립 읽기이며 이 감사 담당이 새 브라우저를 실행한 증거가 아니다. 실제 기기/다른 엔진/보조기술/필드 성능은 이 읽기 감사로 검증하지 않았다.

## 근거·보류·공개 경계

`contracts/public-catalog.mjs:35–47`은 current/playlist·provider-confirmed·conflict===false·문자열 번호와 비어 있지 않은 checkedAt/title/artist·공식 numeric URL 조건을 boolean 판정으로 공유한다. 검색의59–61행은 기존 number AND에 confirmed-tj/ky를 추가하며 선택하지 않은 업체의 관측을 확인으로 바꾸지 않는다. `confirmedNumbers`의 독립 evidence copy와 기존 export는 유지된다. `contracts/confirmed-discovery.mjs:2–4`의 Node hash/가수 그룹을 browser 검색에서 import하지 않는다.

`contracts/provider-lookup.mjs:6–30`은 HTTPS·정확한 TJ/KY path/namespace·일치 번호·추가/중복 query 금지 조건을 공유한다. 이는 실제 제목·가수·버전의 편집 검토를 대신하지 않는다. 날짜/미래 시각·response hash·원 관측과 action/번호의 정합성은 `contracts/provider-corrections.mjs`의 전체 ledger 검증에서 확인한 뒤 공개 projection을 만든다. browser predicate의 nonempty 검사만으로 새로운 외부 candidate를 승인하는 구조로 읽지 않았다.

독립 Python 읽기 검사에서 base2964의 원77개 객체 **전체내용·순서와 최상위 metadata**가 현재 ledger prefix와 같았다. 원77 JSON compact SHA256은 `c2396a2aa93c550e40dd8881527db06305fd01926f9498b54196a2db803e83cb`이며 `e2e/deferred-guide.spec.ts:106–108`의 고정 hash와 일치한다. 같은 파일109–122행은 SAM/Ado의 정확한 TJ 근거·금영 confirm 부재와 observed 상태를 유지하고,132행은 현재 **전체** ledger hash를 build-info와 대조한다. 최소77만 허용하고 새 항목 검증을 버린 변경이 아니다.

[원 후보와 별도 판단](../data/provider-review-requests-20261017-2200.json)의7개 항목을 현재 ledger77..83과 독립 대조했다. song/provider·문자열 번호·expectedObservedNumber·action·전체 제목/가수·URL·실제 checkedAt·responseSha256·편집 이유가7/7 일치했다. 후보의 verification은 `unreviewed`, 편집 판단은 별도이며 원 조회자는 `/root/resume22_evidence`다. 이 감사의 provider 재요청은0건이고 원 HTML이 보존되지 않아 응답을 재파싱했다고 쓰지 않는다. KY43862에 없는16th나 KY44466에 없는 OST를 보충하지 않았고, Tani KY null·SAM/Ado KY 보류를 새 확인/미수록으로 바꾸지 않았다. 음원/반주기 모델 동일성·자료 재배포 권리·Google 계정/심사 결과는 확인하지 않았다.

base2964와 현재의 `data/staging/legacy-catalog.json`, `data/chart-snapshot.json`, `data/legacy-identity-baseline.json`, `src/styles/global.css` 바이트를 실제 읽기 비교해 모두 동일했다. 따라서 이 범위에서는 원 관측/차트 기간·안정 ID·원래 blue→pink/pinkmintblue를 새로 쓴 변경이 없다. 검색/전체/조건 해제·history/기존 URL·storage 경로는 app.ts120–140/179–218과 신규 테스트에서 보존되는 것을 읽었다. 수집 실패 last-good 동작을 이 감사에서 새로 실행했다고 주장하지 않는다.

## 실제 검사 영수증과 남은 릴리스 게이트

아래는 root가 실행한 원 로그와 영속 영수증을 이 담당이 직접 읽은 결과다. 이 감사 담당의 테스트 실행으로 계상하지 않는다.

| 검사 | 읽은 실제 결과 | 경계 |
| --- | --- | --- |
| canonical `npm test` |325 PASS/0fail/0skip · `/tmp/jpop35-resumed-unit.log` | 재시작 환경의 root 실행. 이전 isolation/설정 실패를 삭제한 결과가 아니다. |
| 타입 |26files/0errors/0warnings/0hints · `/tmp/jpop35-resumed-type.log` | clean CI 결과와 별개다. |
| preview artifact gate |3347files/3336HTML/0indexable · `/tmp/jpop35-resumed-preview-artifact.log` | source2964f7e 기반 dirty local artifact다. |
| 전체 브라우저 |39 PASS/1.2m · `/tmp/jpop35-resumed-preview-e2e.log` | 최신 nonempty/폭 assertion 추가 전 실행이다. |
| 최신 assertion targeted |8 PASS/13.6s · `/tmp/jpop35-resumed-mobile-nonempty.log` | 최신 소유 파일 대상 root 실행이며 이후 전체39/clean CI를 대신하지 않는다. |

영속 연결은 [재시작 로컬](../operations/runs/resume-20261017-2200-resumed-local.json), [전체39](../operations/runs/resume-20261017-2200-resumed-e2e.json), [최신8](../operations/runs/resume-20261017-2200-nonempty-e2e.json), [이전 시각 실패/수정 증거 색인](../operations/runs/resume-20261017-2200-ui-qa.json)이다. `scripts/verify-dist.mjs:165–179/214–216`의 private/내부 response hash·광고/가사/외부 이미지 금지 검사를 그대로 읽었고, preview0index 성공을 광고/콘텐츠 정책 전체나 실제 Google 승인으로 확대하지 않는다.

[현재 v0.4.4 운영의 엄격 HTTP 영수증](../operations/runs/resume-20261017-2200-resumed-baseline-http.json)은2026-10-17T18:12:28.859Z–18:12:49.047Z의 root 실행이다. 제품 source `f959f3671b8aeb766fd469558634f9680635e58c`, production target에서27check/1attempt PASS와 전후 build-info, catalog/본문/정적 자산·비공개 경로404를 읽었다. 이는 복구용 이전 정상 artifact의 현재 운영 일치 증거이며 **이번 새 제품의 실제 old rollback→new restore 실행 증거는 아직 아니다**.

현재 읽기 감사의 결론은 **추가 필수 소스 수정 없음, 위치 선택자의 유지보수 결합을 명시한 로컬 범위 수용**이다. clean SHA CI·두 target 전체 재현·동일 SHA의 실제 preview/운영 브라우저·실제 이전 버전 복구/새 버전 재적용·Release는 root의 후속 실행 결과를 받아 별도 추가해야 한다. 이 감사와 버전 후보를 운영 반영/오늘 전체 개발 종료/AdSense 승인으로 쓰지 않는다. Google 계정 본인정보·법적 동의·실제 제출/통지는 운영자 단계다.

## 감사한 현재 파일 식별

| 파일 | SHA256 |
| --- | --- |
| `src/components/Search.astro` | `3425674f4b3e622b190def5bce46fda6bad1b4fc49be53ebfa0c080900a2410c` |
| `src/components/SongRow.astro` | `14ca8a3bd8795f6b15e010e3d90dea6d42ce3da51c27ef69180c33d20739e545` |
| `src/scripts/app.ts` | `690882d17f94c7d058980078cdca892386b3d80ddccb7db03d21cdf1b3751ba2` |
| `e2e/confirmed-search.spec.ts` | `3f18d9c0e404a8c83a32383950b3a88882d05aa42bbd92e31090de1817e8906b` |
| `tests/confirmed-search.test.mjs` | `e0603e17f871db4d215ed8285f422107828733992a5b2412e3d3e87611171f68` |
| `data/reviewed/provider-corrections.json` raw bytes | `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1` |

문서의 상대 링크는 실제 로컬 대상과 대조했다. 후속 실행 결과를 전달받기 전까지 위 관측 범위를 유지한다.

## 후속 읽기 · clean M의 CI·재현·실제 preview

2026-10-17T18:23:17Z(10월18일03:23:17 KST)에 새 읽기 턴에서 현재 source HEAD `ed22f924536d28fa990d7688b6ba352446984140`와 clean status를 직접 확인했다. 이는 [소스 PR36](https://github.com/jpop-today/jpoptoday/pull/36) 병합 M이며 원 base/첫 감사 HEAD와 구분한다. 환경은459/current/running으로 재확인했다. 아래의 **새 영수증만 추가로 읽었으며** 위의 원6개 평가 점수·이유나 소스는 바꾸지 않았다.

| 새 게이트 | 실제 읽은 결과와 SHA | 근거 |
| --- | --- | --- |
| main 앱·계약 CI | 같은 M의 app39057562960: canonical325/0fail, 타입0/0/0, preview artifact3347files/3336HTML/0index·브라우저39 PASS/52.9초, production artifact3347files/3336HTML/49index·브라우저39 PASS/33.4초. 별도 contracts39057563024도325/0fail. | [main CI의 허용된 실제 로그 행](../operations/runs/resume-20261017-2200-ci-main.json). 이전 dirty39나 targeted8로 최신 assertion의 전체 통과를 대신하지 않고, 이 같은 M의 CI39+39를 후속 근거로 읽었다. |
| 두 target 재현 |18:19:25.182797Z–18:19:57.099745Z, target마다 clean M으로2회 빌드. 각3347 전체 파일 집합과 모든 파일 SHA256이 동일하고 metadata dirty:false다. 두 target은 같은 catalog/ledger/chart hash를 사용하며 target별 manifest는 구분된다. | [전체 파일 재현 영수증](../operations/runs/resume-20261017-2200-reproducibility.json). **전체 CI archive와 로컬의 비교는 미실행**이며 이 결과로 주장하지 않는다. |
| 실제 전용 preview 배포 | 기존 c06ff6d6/Mf959f36에서 `cbf1faa6-973e-4b81-8360-df914d92bc4f`/동일 M으로 변경되고 API status success. 실제 URL은 [cbf1faa6 preview](https://cbf1faa6.jpoptoday-preview.pages.dev). | [관리자 cloud 배포/API 영수증](../operations/runs/resume-20261017-2200-preview-deployment.json),18:21:24.796239Z. 전용 preview 프로젝트이며 jpop.today 운영 전환과 다르다. |
| 실제 preview 엄격 HTTP |18:22:01.463Z–18:22:04.530Z, 같은 M/target preview·27check/1attempt PASS. 전후 build-info, catalog/본문/자산과 expected artifact의 일치·비공개/없는 경로404를 읽었다. | [preview HTTP 영수증](../operations/runs/resume-20261017-2200-preview-http.json). 정상 바이트/정책 게이트의 관측 범위이며 모든 CDN 동시 상태나 Google 승인을 뜻하지 않는다. |

이 후속은 root 실행의 영수증을 읽은 감사이며 이 담당이 CI·빌드·배포·HTTP를 중복 실행한 것이 아니다. 실제 배포 URL을 대상으로 하는 원격 preview workflow39057771259는 관리자 마지막 통보에서 진행 중이므로 이 문서에서 성공으로 확정하지 않는다. 실제 jpop.today의 새 M 운영·원격 브라우저, old rollback→new restore, Release는 아직 이 턴에서 수신/읽지 않은 후속 게이트다. main의 production artifact39 PASS를 실제 운영 브라우저39 PASS로 바꾸지 않는다. 새 사실을 추가한 뒤 이 문서를 다시 동결한다.


## 관리자 인계 마감 · 2026-10-17T21:01:05.112337+00:00

이 절의 작성/읽기 주체는 root다. 교체 담당이 마지막으로 직접 동결한 범위는 위18:23:17Z 코드/CI·재현·전용preview HTTP 감사다. 이후 원격운영/복구/Release 읽기 추가요청은 실행 중단으로 새 교체담당 결과를 남기지 못했고, 지금 협업목록은 root만 관측된다. 이 후속을 교체 담당이 읽었다고 기록하거나 원6평가 점수를 바꾸지 않는다. 추가 worker 없이 기존 인계 자료로 보고를 마감한다.

관리자는 [자료 담당 최종 보충수용](../operations/runs/resume-20261017-2200-final-evidence-acceptance.json)과 [품질 담당 최종 보충수용](../operations/runs/resume-20261017-2200-final-quality-acceptance.json)을 읽고 실제sameM 원격preview39/운영39·oldv04427/1→첫구카탈로그거부→newv04527/2, Release415990362·태그M/ReleaseDiscordsent1과 일치함을 확인했다. 이 두 기록은 각 기존 담당의 실제18:35 최종 읽기이며 새 점수가 아니다. [재개 API 상태](../operations/runs/resume-20261017-2200-reporting-recheck.json)와 [새 운영 엄격 HTTP](../operations/runs/resume-20261017-2200-reporting-resume-http.json)는 여전히 제품M/a268291a·27검사/1시도 성공이다. 최초 복원 CATALOG_MISMATCH와 짧은3회 원문URL 관측을 보존하며 전체CDN/연속업타임·Google승인을 보장하지 않는다. 교체 코드 감사의 현재 추가수정없음 판단과 실제최종통합 게이트를 수용한다. 관리자 완료보고·송신은 해당run의 후속 영수증에서 별도 확인한다.
