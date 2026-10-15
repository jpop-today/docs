# 제품 스펙 v0.4

상태: 구현 기준 v0.4 · 2026-10-14 KST · 최종 책임: 프로젝트 운영자, 일상 구현·검증·배포: 위임된 에이전트.

## 목적과 사용자

한국 노래방에서 부를 JPOP를 찾는 사람이 한글·원제·가수·번호로 검색하고, TJ와 금영의 차이를 이해하며, 저장한 곡을 현장에서 다시 꺼낼 수 있게 한다. 번호가 있다는 주장과 실제 확인된 사실을 구분하고, 목록에 없다는 이유로 미수록이라고 단정하지 않는다.

첫 방문자는 검색과 번호 복사를, 반복 방문자는 즐겨찾기·선곡 목록을, 곡을 모르는 사람은 실제 데이터에 근거한 가이드·가수 페이지를 사용한다. 제품 언어는 한국어이며 곡명·가수의 일본어 원문을 함께 제공한다.

## 사용자 흐름

홈 → 한글/일본어/가수/번호 검색 → 결과에서 사업자별 상태 비교 → 번호 복사 또는 곡 상세 → 즐겨찾기 저장 → 즐겨찾기 목록에서 다시 조회·번호 목록 복사.

검색 실패 → 검색어·조건 해제와 제보 경로. 수집 지연 → 실제 수집 시각과 지연 안내. 미확인 번호 → 미확인 표기와 근거 제보. 번호 수정 → 기존 URL·저장 ID가 계속 같은 곡을 가리킴.

## 요구사항과 수용 기준

| ID | 요구사항 | 검증과 완료 기준 |
| --- | --- | --- |
| SRC-001 | 개인 원본 이력·운영 배포·이용 범위 조사 후 조직 이관 | dev/main 두 SHA·이력·파일 출처 기록, 운영 SHA는 실제 API/대시보드 증거; 기존 org 이력 보존 |
| DATA-001 | 문자열 번호, 사업자별 registered/unknown/unlisted 구분, 출처·확인일 추적 | 계약 테스트, 실제 데이터 검증 결과 분리; 충돌·증거 누락은 staging에 남기고 임의 보정 금지 |
| PREVIEW-001 | 기존 관측 번호를 사실대로 설명하는 앱 미리보기 | observed-unverified/unknown read model은 v1 승격과 분리; 상세 충돌 경고·noindex·광고 없음; [ADR-004](../adr/004-observed-preview-read-model.md) |
| HARNESS-001 | 잘못된 수집/검증 실패가 마지막 정상 스냅샷을 지우지 않음 | 빈 입력·깨진 JSON·동일 경로·쓰기 실패·rename 실패에서 기존 바이트 보존 테스트 |
| SEARCH-001 | 한글·일본어·가수·번호·결합 검색, 필터와 URL 복원 | 기존 대표 곡·복합 검색 E2E, 결과 없음 복구, 필터 조합·새로고침·뒤로가기 |
| SONG-001 | 원제·한국어명·가수·사업자 번호·근거를 공유 가능한 상세에서 제공 | JS 없이 본문 렌더, 고유 title/canonical, 복사 동작, 이전 /song?id= 주소 연결 |
| FAVORITES-001 | 기존 개인 저장 목록 유지, 저장·해제·목록 복사 | jpop_today_favorites 문자열 ID/{id} 입력 유지, 미해결 ID 보존, 저장 불가시 안내 |
| CONTENT-001 | 실제 비교·조사로 쓴 자체 가이드·가수 설명 | 편집 근거·작성/확인일·검토자, 중복/빈 본문 거부, 권리 불명 가사·이미지 비공개 |
| SEO-001 | 정적 본문·내부 링크·정확한 sitemap/canonical/HTTP 상태 | 빌드 산출물 검사, 링크 검사, 실제 404, 검색/개인화 페이지 index 전략 확인 |
| PRIVACY-001 | 실제 수집·저장·광고에 맞춘 안내와 필요한 동의 | 개인정보·문의·운영자 안내, localStorage 설명, 지역별 요구 검토; 법적 동의는 운영자 처리 |
| RELEASE-001 | 동일 SHA의 CI→미리보기→운영과 복구 증거 | dist 전용 배포, 비공개 원본 404, 호환성 스모크, 이전 정상 deployment 복구 확인; [ADR-007](../adr/007-deployment-response-verification.md)의 실제 본문/해시·전후 metadata·시간 상한 검사 |
| ADS-001 | 콘텐츠·권리·크롤링·광고 설정을 점검하고 실심사 결과 추적 | 준비 항목 증거, 소유권 확인, 운영 반영 확인, 심사 제출일/결과 기록; 승인 보장 없음 |
| DAILY-001 | 매일09시 Asia/Seoul에 관리자·세 신규 담당 개발 | 실제 Codex 예약 생성·실행 상태 분리, 시작/완료와 재개 기록; Actions 보고와 AI 실행 구분 |
| REPORT-001 | 오전 전일/계획·완료 개선·CI/CD·릴리스의 Discord/채팅 보고 | secret 기반 송신, missing/skipped·failed·sent 구분, 전날 실제 기록·검증 근거, 비밀값/멘션 보호 |
| TEAM-001 | 각 결과를 두 동료가 평가하고 최저 담당 교체 | 총6개 실제 근거 리뷰·4항목0..5, 관리자 확인·동률 규칙·active roster 제외/신규 생성 이력 |
| DESIGN-001 | 원본 브랜드와 그라데이션 색 유지 | 원본 CSS SHA·색 근거, 대비·360px/키보드/검색/저장 브라우저 확인; [색상 기록](../engineering/design/legacy-gradient.md) |

## 정보 구조

- /: 검색 진입, 실제 관리 목록 소개, 검토된 가이드와 차트 연결
- /search 또는 기존 /filter: 검색·필터. 기존 query를 어댑터로 지원
- /songs/<stable-id>/: 정적 곡 상세. 기존 /song?id=는 호환성 진입 경로
- /artists/<stable-id>/: 실제 관리곡 비교와 근거 있는 가수 정보
- /favorites: 사용자 기기에 저장된 선곡 목록, 인덱싱 제외
- /chart: 공식 차트의 기간·수집 시각·출처, 관리 목록과 구분
- /guides 및 개별 가이드: 문제를 해결하는 자체 편집 콘텐츠
- /coverage: 관리 목록의 TJ/KY 관측/확인 범위를 분모와 함께 표시
- /about, /contact, /privacy, /faq, 실제 404

기존 /chart, /coverage, /guides/*, .html URL과 /#favorites-view를 조사된 규칙으로 보존한다. 새 경로를 이유로 기존 즐겨찾기를 초기화하지 않는다.

## 품질 목표

모바일 360px에서 가로 넘침 없이 검색·번호 복사·저장이 가능하고 키보드·보조기술로 주요 동작을 사용할 수 있어야 한다. 번호 복사·즐겨찾기·광고 클릭 영역을 명확히 구분한다. Lighthouse·Core Web Vitals는 측정한 환경과 실제 필드 데이터를 구분한다. 목표는 LCP ≤2.5s, INP ≤200ms, CLS ≤0.1이며 현재 달성 사실로 쓰지 않는다.

첫 릴리스에 로그인·결제·서버 DB·추천 모델은 도입하지 않는다. 원문 가사 전체 공개·음역/난이도 자동 추정·검색엔진만을 위한 대량 페이지는 기본 기능에 포함하지 않는다. 광고는 검색과 번호 사용을 방해하지 않도록 별도로 검증한다.

일일 개발·동료 평가·보고·릴리스는 [ADR-005](../adr/005-daily-agent-management.md)와 [실행 계약](../operations/daily-development.md)을 따른다. 전일 대비 변화가 없거나 검증이 실패하면 그 결과와 다음 작업을 보고하며 내용 없는 버전을 게시하지 않는다.

## 결정과 미확인 사항

정적 Cloudflare 배포를 유지하고 콘텐츠·데이터·UI를 분리한다([ADR-001](../adr/001-static-architecture.md)). ID는 편집값과 분리한다([ADR-002](../adr/002-stable-identity.md)). 권리·출처 증거를 별도 관리한다([ADR-003](../adr/003-evidence-and-publication.md)). Cloudflare 운영 프로젝트·branch/SHA는 [실제 기준선](../operations/cloudflare-baseline.md)으로 확인했다. AdSense 계정 상태·자료 권한·새 앱 deployment와 rollback 실행은 확인 전까지 unknown이다.

## 검토 정정·차트·실제 운영

[ADR-006](../adr/006-reviewed-corrections-and-production.md)에따라동결staging을유지하고번호정정근거를별도ledger로적용한다. 원본ID/관측을보존한v2공개모델에서공식확인/관측/unknown과정정이유·공식제목/가수·실제확인일을구분한다. 오류번호는필터·개별/bulkcopy에서제외하고저장ID는유지한다. 차트rawalias는원본identity observation으로seed한canonicalId에고정하며순위바뀜에index join하지않는다. 완전한100곡/날짜/ID/tuple/미래시각과실패보존을검증한다.

preview는모두noindex/광고없음,production은홈·검토가이드/안내·공식근거가있는현재상세만index하고sitemap과meta를일치시킨다. 나머지상세·검색·저장·호환·404는noindex다. 기존todayjpop도메인origin유지와개인Git자동배포가역중지·orgartifact배포·실제복구/HTTP/브라우저를검증한다. 단순문서/preview완료로실제운영이나AdSense승인을주장하지않는다.

## 가수별 확인곡 탐색 계약 · Issue6 (2026-10-15)

`/artists/`는 검토 근거가 있는 current/playlist 곡을 관리자료의 `artistOriginal` 정확한 문자열별로 묶는 정적 진입점이다. 별칭·공동명의를 추정해 합치지 않으며 가수 약력/난이도/권리/추천을 생성하지 않는다. 번호는provider-confirmed·공식numeric URL/실제시각·문자열번호·conflict:false인 업체만 표시한다. observed/unknown/archived/chart-only는확인곡으로포장하지않는다. 가수anchor는정확한원문에서결정해재정렬/다른곡추가로바뀌지않으며곡URL·storageID는원본대로다. 개별가수thinpage는만들지않는다.

CONTENT-001/SONG-001/SEO-001/DESIGN-001: 홈/메뉴→가수별확인곡→원문제목·가수·업체근거→기존곡상세와저장. JS-off/360px/키보드 및 preview전체noindex/무광고와production검토경로만색인, trustedprojection의schema/출처검증·비공개자료404·frozenbuild/sameSHA CI/실제복구·origin본문을수용한다. 공식조회실패는관측값을unknown으로일괄변경하지않고마지막정상자료를유지한다. 실제반증이있는경우만ledger suppress하며실조회증거를기록한다.

## 안전한 공식 번호 조회·표기 비교 계약 · Issue20 (2026-10-15)

DATA-001/HARNESS-001: `scripts/provider-metadata.py`는 TJ/KY 공식 numeric 검색의 번호·제목·가수 셀만 추출한다. 엄격 UTF-8/2MiB 상한·일치하는 단일 결과·공식 요청/최종 URL·HTTP200·제한된 timeout을 검증하고 안전한 오류 code만 출력한다. `parse_metadata(body, provider, number)`는 number/title/artist만 반환하며 offline parse가 실제 source/checkedAt/검토 성공을 만들지 않는다. `fetch_metadata`의 실제 성공만 응답 hash/바이트/실제 조회시각과 unreviewed candidate를 기록한다. 추출은 사람의 제목·가수·버전 검토/ledger confirm을 대신하지 않는다. 원문 가사/응답/주변문맥/시크릿/서명URL은 로그·출력·공개artifact에 남기지 않는다.

수집 결과는 별도 새 candidate 파일에 exclusive/atomic하게 게시하며 기존 reviewed ledger/staging/chart/마지막 정상 snapshot을 수정하지 않는다. 기존 경로·symlink·쓰기/수집/부분파싱 실패는 보존하고 실패로 반환한다. 합성 fixture만 회귀에 사용하며 실제 가사를 fixture로 복사하지 않는다. 문자열 번호/선행0·ID/URL/storage·unknown/미수록 구분은 유지한다.

CONTENT-001/SEO-001/DESIGN-001: `/guides/title-artist-variants/`는 기존 실제 공식 대조(2026-10-15 03시 기록)의 SPYAIR·JANE DOE 표기 사례로 제목/가수/공동명의·업체·버전을 함께 확인하는 자체 가이드다. 구두점만으로 자동 동치/다른곡을 단정하지 않고 정확한 기존 원문·공식URL/actual checkedAt를 표시하며 새 조회나 권리/음원·난이도/추천·심사 결과를 만들지 않는다. 기존5가이드의 본문/확인일·URL을 보존하고 새6번째만 실제 편집일을 기록한다. JS-off/360px/키보드·무광고/preview noindex·production 검토경로와정확한sitemap·동일SHA CI/복구/실운영 본문검사를 수용한다.

## 가수별 업체 확인곡 수 · Issue24 (2026-10-15)

DATA-001/CONTENT-001/SEO-001/DESIGN-001: 동결차트11~14위현재playlist4ID의TJ/KY관측8번호를공식metadata-only로실제조회하고원문제목·가수·버전을대조한근거만ledger에append한다. 기존63항목/원본입력·stableID·URL/storage·unknown을보존한다. 실패/불명은confirm하지않는다.

`/artists/`는global과각가수바로가기/그룹에서TJ·금영의공식확인곡수를정적표시한다. 업체별각ID한번,전체확인곡수는합집합이며TJ+금영단순합이아니다.0은공식미수록을뜻하지않는다.기존anchor/명칭·공식근거/checkedAt·원문그룹/공동명의분리·JSoff/360px/키보드·브랜드/preview전체noindex와무광고를유지한다.독립실자료계산/실제근거·두target동결재현/sameSHA CI·실제복구/원격운영을수용한다.
