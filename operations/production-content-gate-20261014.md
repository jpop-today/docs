# 운영 전환 콘텐츠·정책 게이트 · 2026-10-14

`ADS-001` · `CONTENT-001` · `DATA-001` · `SONG-001` · `PRIVACY-001` · `SEO-001` · `RELEASE-001`. [소스 Issue #13](https://github.com/jpop-today/jpoptoday/issues/13), [제품 스펙](../product/spec.md), [ADR-006](../adr/006-reviewed-corrections-and-production.md), [심사 기록 기준](adsense.md)을 따른다. 검토 담당은 `/root/replacement_20261014_production_gate`이며 이 문서만 편집한다. 코드·Git·CI·배포는 root가 수행한다.

작업 base는 소스 `9d8cfb716e3ab5194e6711d19ec2b10a297089ad`, 문서 `d7ccc98bb20c8d997c0ac20e8264528c63b77aab`, branch `codex/13-production-transition-20261014`이다. 검토는 한국 시간 2026-10-14 14:31–14:32에 실제 파일과 로컬 산출물, 공식 Google 문서를 읽으며 진행했고 14:37에 정책, 14:40에 최종 고정 산출물, 14:49와 후속에 root의 운영 실행 기록, 15:02에 최종 GitHub 검증/Release/태그를 재확인했다. 이 담당자의 직접 검사와 root가 수행한 배포·HTTP·복구 결과를 구분한다. 문서 검토가 Google의 정책 준수 판정이나 심사 승인을 뜻하지 않는다.

## 현재 판정과 증거 범위

광고 없는 앱의 콘텐츠·자료 경계와 실제 운영 전환 기술 게이트를 통과했다. 처음 인계받은 운영 기준은 개인 저장소 `kim75503/todayjpop/dev`의 `94d615f5e355f4b00ebbb824547ac6dfa034ad18`이었다. 이후 root의 실제 실행 기록에서 기존 `todayjpop` 프로젝트와 `jpop.today` 도메인에 조직 앱 `b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8`를 배포하고 원본으로 canonical을 바꾼 뒤 새 앱을 다시 복원했으며 최종 새 앱 운영 HTTP26개가 통과한 것을 확인했다. 첫 원본 HTTP 검사는 잠시 남은 이전 응답으로 실패했고, 후속 재시험에서 pinned 원본 root 바이트·metadata404·sitemap12와 새 앱 재복원의 root/metadata/sitemap24를 모두 확인했다. 실제 production 원격 Chromium14개와 v0.3.0 Release도 완료됐다. 이 담당자가 배포/rollback/브라우저를 실행하거나 직접 Cloudflare canonical deployment를 조회한 결과는 아니다. Google 실제 계정·권리 전체·심사 승인과 자동 CI 운영배포 credentials는 아래와 같이 별도 상태다.

| 게이트 | 직접 읽거나 실행한 결과 | 남은 검증 |
| --- | --- | --- |
| 사용자 가치·탐색 | 홈/검색/상세/가이드/즐겨찾기·정정 문의가 명확히 연결됨. 네 가이드는 제목 찾기, 업체 번호 차이, 개인 저장, 차트 기간을 설명함. 동일 SHA 로컬/원격 production Chromium14개 통과를 root 인계와 workflow에서 확인 | 실제 필드 성능·WebKit 등은 이번 검증 범위 밖 |
| 번호 정확성·unknown | ledger 43entry 중 confirm30/suppress13. 확인된 번호가 최소 하나인 ID16개. 원본 3,317 ID와 staging revision6d 유지. suppressed 값은 null/unknown이며 미수록으로 쓰지 않음 | 실제 번호·정체성 근거는 [행별 검토](../data/provider-corrections.md), 전체 목록 검증은 미완료 |
| 정정한 번호 복사 | 튜링 러브 상세에 잘못된 KY `57803`의 `data-copy` 없음, 확인한 TJ `52536`의 복사 버튼 있음. 공식 결과·확인일·정정 이유와 버전 경고 렌더 직접 확인. 동적 검색·필터·전체 목록 복사와 기존 저장값 회귀는 root의 같은 SHA 브라우저 결과로 확인 | 제공사 전체 수록·실제 모든 반주기 모델/오디오 검증은 하지 않음 |
| 운영 색인 | 최종 clean production artifact에서 일반12개와 current·공식근거·무충돌 상세12개, 합계24개를 확인. 3,317 상세 중 나머지3,305개는 noindex/sitemap 제외. confirmed ID16개 중4개는 남은 공유번호 conflict로 제외. root의 운영 HTTP26 기록에서 색인·robots/sitemap 등 게이트 통과 확인 | 실제 검색엔진 색인/Search Console는 아직 확인하지 않음 |
| preview 경계 | 최종 clean preview 로컬 검사에서 3,333 HTML 전체 noindex, sitemap URL0, robots 전체 차단, 광고 코드 없음. 실제 preview HTTP26 기록과 원격 브라우저 workflow success도 읽어 확인 | 실제 운영 artifact와 동일 입력 hash·SHA 연결은 아래 확인. 새 실제 운영 브라우저 결과는 별도 |
| 가사·이미지·내부 자료 | public catalog는 곡 표기·번호 상태·검토 근거만 사용. verifyDist가 가사/외부 이미지 payload·광고 코드·raw/staging/ops/reports/releases 경계를 검사해 통과 | 번호/메타데이터의 재배포 허용 범위 전체는 미확인. 비공개 처리는 권리 허락의 증거가 아님 |
| 개인정보 안내 | 계정 없음, 로컬 검색, 검색 URL의 전달 가능성, local/sessionStorage, 클립보드, Cloudflare 요청 처리, 외부 링크, 문의자료 처리, 광고/분석 없음이 구현 설명과 대응함 | 실제 광고를 도입하기 전에 수집·쿠키·공급업체·거부/철회·적용 지역 동의 흐름을 다시 반영 |
| 운영·문의 신뢰 | 소개는 독립 서비스와 TJ/KY 비제휴를 명시. 기존 공개 이메일과 mailto, 정정 제보에 필요한 곡 주소·업체·제목·번호·출처 안내를 확인함 | 이메일을 실제 발송하지 않았고 수신함 소유·운영자의 응답/보관 기간 실행은 확인하지 않음 |
| 같은 SHA·복구·자동 덮어쓰기 | 동일 SHA CI/preview와 production artifact를 확인. root의 실제 배포·원본 API/HTTP rollback·새 앱 API/HTTP 복원·기존 개인 Git 자동배포 pause 결과와 최종 운영 HTTPS26개, production 원격 Chromium14개 통과를 확인 | GitHub 자동 운영배포는 CF credential missing으로 실제 skip. 실제 운영은 root의 managed binding 직접 배포 |
| AdSense | 실제 계정·소유권 확인·검토 요청·결과를 읽지 않음. 상태 `unknown` | 운영자가 실제 계정 절차를 진행하고 Google 통지/상태를 확인해야 함 |

## 직접 실행한 로컬 검사

14:31–14:32에 소스 `0be17904174e673b56a021873085aceee09243f3`의 clean preview 로컬 산출물에 대해 `node --input-type=module`로 `verifyDist()`를 호출했다. `build-info.json`의 검사 시작/종료 바이트가 같은지도 확인했다. 이후 14:40에 root가 고정한 `/tmp/jpop-v030-preview-dist`, `/tmp/jpop-v030-production-dist`를 아래처럼 각각 검사했다. 두 폴더는 실제 로컬 검증용 산출물이며 공개 artifact나 운영 URL이 아니다.

`verifyDist({distPath:<각 고정 경로>, expectedTarget:<preview 또는 production>, expectedSourceSha:'b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8'})` 두 호출이 모두 통과했다. 별도 읽기 코드로 3,317 상세의 meta와 sitemap 수, 네 가이드의 편집 표기/본문 날짜/공식 링크도 확인했다.

| 항목 | 실제 결과 |
| --- | --- |
| 최종 source SHA | `b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8` |
| target / dirty | preview와 production 모두 검사 / false |
| 파일 / HTML / 상세 | 3,341 / 3,333 / 3,317 |
| current / archived | 3,313 / 4 |
| indexable | preview0 / production24(일반12 + 상세12) |
| 상세 noindex | preview3,317 / production3,305 |
| 가이드 편집 표기 | 4개 모두 `jpop.today 편집`, `<time datetime="2026-10-14">`, 공식 TJ/KY 링크 존재 |
| ads.txt | 두 artifact 모두 없음. 임의 publisher 값을 발행하지 않았으며 실제 계정 검증은 별도 |
| catalog SHA-256 | `73b50974c365bc3accf552beae6a41d0c565c7f80da8281b11672d6f26a4a1fe` |
| ledger SHA-256 | `0644fc5e74b1d3b456a0fc5d5e939fa55eafee7f238e91d2730bb609e215628f` |
| chart SHA-256 | `971bacf9cb65e5b7176e8a5aa59394d2c46ce00d0676aa033178dc11ae7540b9` |

처음 공유 `dist`에서 dirty production 산출물의 sitemap24를 읽었으나 root가 preview 빌드로 전환하면서 `expectedTarget:production` 검사는 target 불일치로 한 번 중단됐다. 이는 잘못된 운영 산출물 판정이나 배포 실패 증거가 아니다. 공유 빌드 중간 관측을 제외하고 이후 안정된 clean 검사와 고정 폴더의 두 target 검사를 확정 결과로 기록했다.

GitHub 읽기 API로 [앱 CI38517512773](https://github.com/jpop-today/jpoptoday/actions/runs/38517512773), [계약 CI38517512519](https://github.com/jpop-today/jpoptoday/actions/runs/38517512519), [원격 preview38517673196](https://github.com/jpop-today/jpoptoday/actions/runs/38517673196)의 `completed/success`와 동일 head SHA `b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8`를 직접 확인했다. 원격 preview의 Chromium14개 통과는 root의 실행 인계이며 이 담당자가 브라우저를 다시 실행한 결과가 아니다. [preview HTTP26 기계 기록](releases/preview-http-v0.3.0-20261014.json)은 root의 14:38:03 KST 요청 결과를 읽어 확인했다. 실제 URL은 [5011a963 preview](https://5011a963.jpoptoday-preview.pages.dev)다.

## 실제 운영 배포·복구 기록의 읽기 확인

다음은 root가 실제 실행한 기계 기록을 이 담당자가 읽어 대조한 결과다. 새 앱을 기존 origin에 배포해 URL·브라우저 저장값을 이어 읽는 경로이며, 개인 Git 저장소 이력과 Cloudflare 프로젝트/도메인을 삭제하지 않았다.

| 실행 증거 | 확인한 결과 |
| --- | --- |
| [개인 Git 자동배포 pause](releases/production-pause-v0.3.0-20261014.json) | before `production_deployments_enabled:true`, preview `all`에서 after `false`, `none`으로 변경. owner/repo/production branch와 두 도메인 보존 |
| [새 운영 배포](releases/production-deployment-v0.3.0-20261014.json) | deployment `2be02785-8a37-4bba-a713-08847a2fa6f7`, 14:43:54 KST deploy success, ad_hoc/dev, commit `b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8`, dirty:false |
| [첫 운영 HTTP](releases/production-http-first-v0.3.0-20261014.json) | 14:45:01 KST, `https://jpop.today`, production/b0f4/clean, 26check 모두 passed |
| [실제 원본 rollback](releases/production-rollback-original-v0.3.0-20261014.json) | 새 `2be02785`에서 원본 `3f622896-f6c3-4cb0-b39d-b6b9678d6d0c`/`94d615f5e355f4b00ebbb824547ac6dfa034ad18`로 canonical 전후 변경 |
| [새 앱 다시 복원](releases/production-restore-v0.3.0-20261014.json) | 원본 `3f622896`에서 새 `2be02785`/b0f4로 canonical 전후 변경. 개인 Git 자동배포는 false/none 유지 |
| [최종 복원 후 운영 HTTP](releases/production-http-v0.3.0-20261014.json) | 14:55:21 KST, query 없는 기본 `https://jpop.today`, production/b0f4/clean, 26check 모두 passed, 세 입력 hash가 위 고정 산출물과 일치 |
| [첫 복구 probe 실패 보존](releases/production-rollback-probe-failed-v0.3.0-20261014.json) | 14:49:55–14:50:40 KST, 20attempt 모두 ready:false. root와 sitemap은 원본으로 바뀌었지만 고정 metadata 주소가 계속 새 앱 응답을 반환 |
| [후속 실제 원본 HTTP 복구](releases/production-rollback-http-v0.3.0-20261014.json) | 14:54:35 KST, passed:true. 운영 root의 SHA-256 `89114fcdb75168f6339249f47d1322a4282064b47ca0ab3f69db54f3bff26a3e`가 pinned 원본과 같고 metadata404·sitemap12 확인 |
| [후속 새 앱 HTTP 복원](releases/production-restore-http-v0.3.0-20261014.json) | 14:54:37 KST, passed:true. 새 root SHA-256 `9b6bb27d9849d5cf8bc0a6133a3173118dde3d95d8454042dc4dd7b956594860`, metadata200/b0f4·sitemap24 확인 |

첫 rollback의 canonical 전후는 API로 확인했지만 첫 원본 HTTP probe는 실패로 보존했다. 실제 기록에서 root는 원본 바이트로 바뀌고 sitemap은12개로 바뀌었지만 고정 metadata URL이 이전 새 앱 응답을 계속 반환했다. 후속 제한 시간 내 재시험은 cache-busting query가 붙은 진단 요청의 metadata404와 pinned 원본 root 바이트·sitemap12를 함께 확인했고, 이어 새 앱의 metadata/b0f4·root 바이트·sitemap24로 재복원됨도 확인했다. TLS 검증과 기존 proxy/CA를 유지한 HTTP 요청이며, 진단 query 응답의 확인을 모든 캐시 노드에서의 즉시 전환 보장으로 해석하지 않는다. 마지막에는 query 없는 기본 운영 URL의 전체 HTTP26개도 다시 통과했다.

Cloudflare의 Git source metadata가 여전히 `kim75503/todayjpop/dev`라는 사실과 새 공개 artifact의 실제 source SHA/조직 소유권을 구분한다. 위 전환은 기존 프로젝트에 검증 산출물을 직접 배포하고 이전 Git 자동배포를 가역 중지한 것이다. Git 연결 자체가 조직 저장소로 변경됐거나 향후 GitHub CI 자동 운영배포가 이미 가능한 것으로 설명하지 않는다. [운영 workflow38517937051](https://github.com/jpop-today/jpoptoday/actions/runs/38517937051)은 success였지만 Cloudflare Actions binding missing으로 배포·원격 검증 단계가 실제 skipped였다. root가 별도 managed binding으로 실행한 실제 배포와 이 workflow를 구분하며 cloud credential을 Actions secret으로 복사하지 않았다.

마지막 [production verify-only38519245940](https://github.com/jpop-today/jpoptoday/actions/runs/38519245940)의 completed/success와 head SHA b0f4를 GitHub 읽기 API로 직접 확인했다. 배포 단계는 verify-only 의도대로 skipped이고 실제 production SHA/브라우저 검증 단계는 success였다. root 인계에서 실제 운영 원격 Chromium14/14(16.5초), 로컬 production14/14(10.2초), 최종 홈의 외부 스크립트 수0을 확인했다. 이 문서 담당자가 그 브라우저/홈 측정을 재실행한 것으로 쓰지 않는다.

[v0.3.0 GitHub Release](https://github.com/jpop-today/jpoptoday/releases/tag/v0.3.0)는 15:01:10 KST에 실제 published, draft:false/prerelease:false로 읽었다. Git tag `refs/tags/v0.3.0`가 b0f4 commit을 가리키는 것도 직접 확인했다. [Release workflow38519652701](https://github.com/jpop-today/jpoptoday/actions/runs/38519652701)은 같은 SHA의 completed/success다. 검증된 운영 릴리스와 AdSense 승인은 별개다.

## 자체 콘텐츠 보강

첫 파일 검토에서 네 가이드에 공개 편집 담당·본문 검토일이 없고, 일부 설명이 모든 표시번호와 목록 복사를 관측 번호로 표현했다. root가 수정한 최종 소스 `b0f4a48fadf3c28af5ccb161b6d49ae95c0716e8`와 두 고정 산출물을 다시 읽어 실제 프로젝트 편집 표기와 오늘 본문 검토일, 공식 확인/관측/정정한 번호 구분, 억제한 번호의 복사 제외 설명, 차트 요청기간과 응답 검증 구분을 확인했다. 가이드 본문 확인일은 전체 제공사 자료 확인일을 뜻하지 않는다.

가이드 수나 색인 URL24개라는 수치는 Google이 충분한 고유 콘텐츠로 인정했다는 증거가 아니다. Google의 사이트 연결 안내는 전체 사이트를 검토한다고 설명하므로 noindex·광고 제외가 해당 화면의 심사 대상 제외를 뜻하지 않는다. 검색·번호 비교·정정 근거·저장 기능의 실제 효용과 검토된 자체 설명을 계속 개선하며, 가이드의 반복 표현이나 수천 개 정형 상세를 고유 편집물로 포장하지 않는다. 가사·앨범 이미지·YouTube 캡처·근거 없는 난이도/음역을 추가하지 않았다.

## 오늘 다시 읽은 공식 Google 근거

한국 시간 2026-10-14 14:31–14:32에 아래 여섯 공식 도움말, 14:37에 게시자/프로그램 정책을 연결된 웹 도구의 직접 `open` 결과와 본문으로 확인했다. 로그인·실제 계정 화면은 확인하지 않았다. 상세 정책 적용 기준은 [전일 정책 조사](adsense-policy-review-20261013.md)도 참조한다.

| 공식 근거 | 직접 확인한 의미 |
| --- | --- |
| [페이지 준비](https://support.google.com/adsense/answer/7299563?hl=ko) | 고유 콘텐츠·방문자 관련성·사용하기 쉬운 탐색을 평가해야 함. 외부 자료를 가져온 사실이나 양만으로 가치가 생기지 않음 |
| [사이트 연결](https://support.google.com/adsense/answer/7584263?hl=en) | 코드/ads.txt/meta 소유권 확인 방법, 실제 검토 요청, 공개 운영 사이트와 크롤러 접근을 확인해야 함 |
| [사이트 관리](https://support.google.com/adsense/answer/12131223?hl=en) | 소유권 확인 이후 정책 검토를 수행하고 실제 AdSense 사이트 상태가 Ready인 뒤 광고 게재 가능 |
| [계정 설정](https://support.google.com/adsense/answer/7402256?hl=ko) | 결제 정보·사이트 연결·전체 사이트 검토가 별도 절차. 2026년6월부터 본인 확인이 요구될 수 있고 실제 요청은 운영자가 처리 |
| [개인정보 필수 콘텐츠](https://support.google.com/adsense/answer/1348695?hl=ko) | Google 광고 도입 시 쿠키/공급업체/개인 맞춤 광고·거부 방법을 실제 설정에 맞게 공개해야 함 |
| [ads.txt 가이드](https://support.google.com/adsense/answer/12171612?hl=ko) | ads.txt 사용은 적극 권장. 실제 계정의 정확한 publisher 값을 사용하고 공개 파일·계정의 상태를 별도 확인 |
| [Google 게시자 정책](https://support.google.com/adsense/answer/10502938?hl=ko) | 저작권 침해·허위 제휴, 가치 부족/미완성/부가 가치 없는 복제 화면, 광고로 인한 사용자 동작 방해를 제한. 현재 무광고·공개 경계 검사가 전체 정책 준수 판정을 대신하지 않음 |
| [AdSense 프로그램 정책](https://support.google.com/adsense/answer/48182?hl=ko) | 본인 광고 클릭·클릭 유도·자동 클릭/노출·인위적 트래픽 금지. 기능 검증은 광고 없는 artifact로 수행하며 실제 광고를 클릭하거나 노출 실험을 하지 않음 |

사이트 연결 문서의 한국어 URL 직접 요청은 Google `sorry` 리디렉션으로 도구 오류가 났다. 같은 공식 문서 ID의 영어 URL은 본문을 읽는 데 성공했다. 한국어 요청을 성공한 것으로 기록하지 않았고 제한을 우회하거나 계정에 로그인하지 않았다.

## 운영자가 처리할 계정 단계와 기술 후속

| 단계 | 필요한 실제 정보·행동 | 진행 경계 |
| --- | --- | --- |
| 계정·법적 동의 | 실제 운영자 계정, 본인정보·지급/세금·지역 자격·약관 및 요청된 신원 확인 | 에이전트가 추측하거나 허위로 대신 제출하지 않음. 공개 문서나 artifact에 개인 신분/결제 자료를 남기지 않음 |
| 사이트 소유권 | AdSense에 등록된 정확한 jpop.today URL과 계정이 제공한 검증 방법·publisher 값 확인 | 원본에 값이 있었다는 이유로 현재 계정 소유권을 확정하지 않음. 검증값을 받은 뒤 광고 없는 meta 경로 등 구현 가능 |
| ads.txt | 실제 계정의 행과 현재 root 파일·계정 상태 대조 | 임의 publisher 값 생성 금지. ads.txt 존재·Authorized는 사이트 Ready와 별개 |
| 콘텐츠/권리 | 번호·메타데이터 재배포 허용 범위와 문의 처리, 자체 콘텐츠·제공사 근거 갱신 | 권리 확인이 안 된 가사/이미지는 공개하지 않음. 번호 검색 확인을 전체 재배포 허락으로 해석하지 않음 |
| 검토 요청·결과 | 제출 시각·운영 SHA·Google 실제 통지/정책센터/사이트 상태 | 실제 Ready를 본 뒤에만 승인으로 기록. 거절 이유는 실제 통지와 당시 artifact에 연결 |
| 광고 활성화 | 실제 광고 범위, 개인정보/CMP/동의·거부·철회, 모바일 오인 클릭 방지 검토 | 검색/개인 목록/미확인·빈 상세/404·preview는 기본 광고 제외. 실제 광고 자동클릭·인위적 노출 테스트 없음 |

계정 정보가 아직 없다는 이유로 번호 정정·콘텐츠 개선·광고 없는 앱의 검증/PR/복구/운영 전환을 중단하지 않는다. 현재 계정 단계의 unknown과 기술적으로 독립적인 배포 게이트를 분리해 보고한다.

## 통합 전 두 동료의 후속 수용 확인

자료 담당 resume_20261014_evidence와 하네스 담당 resume_20261014_harness가 최종 문서·frozen 두 target·실제 HTTP/복구 JSON을 읽기 전용으로 대조했다. 두 담당 모두 차단 사실 오류나 기술 통과와 권리/Google 계정 unknown의 혼동을 발견하지 않았으며 링크15개 존재를 확인했다. 하네스 담당은 연결된 GitHub8읽기로 같은M CI/skip/verify-only/Release를 독립 확인했다. 이 후속 확인은 기존3명6개 ranking·교체 뒤의replacement 산출물 수용 검사이며 새 순위나 교체를 만들지 않았다.

표의 ledger/chart hash는 build-info와 같은 JSON.parse 후 JSON.stringify 결과의 SHA-256이다. 예를 들어 ledger pretty-JSON 파일 바이트 hash afc086…와 semantic hash0644fc…가 다른 것은 형식 차이이며 검증한 행이 달라진 것이 아니다. 공개 catalog hash는 실제 catalog.json bytes를 기준으로 한다.
