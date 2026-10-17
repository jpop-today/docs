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

## 차트의 업체별 공식 확인곡 탐색 · Issue27 (2026-10-16)

DATA-001/CONTENT-001/SEO-001/DESIGN-001: 동결차트15~18위현재playlist4ID의관측TJ/KY8번호는metadata-only실제공식조회/원문제목·가수·버전대조에서근거가확보된것만기존71ledger끝에추가한다.원71전체/순서·staging/chart/3317ID/URL/storage/unknown과실패lastgood를보존하고실패/불명은confirm하지않는다.

`/chart/`의100곡/원래순위/동결기간·수집시각·요청기간경계를유지한다.업체별공식확인곡수와순위순바로가기는기존confirmedNumbers의current/playlist·provider-confirmed·conflictfalse·공식numericURL/실제근거조건에서고유ID로계산한다.전체는합집합이며0은미수록아니다.새안정ID기반anchor로원래차트행/기존상세로이동하고SSR/360pxJSoff/키보드·원래blue→pink/pinkmintblue·광고없음/preview전체noindex를검증한다.관측값으로확인을추정하거나새로운최신차트/권리/Google승인을만들지않는다.독립실자료계산·동결전체재현·같은SHA CI/실제preview/실제복구·운영을수용한다.

## 판단 보류 사례와 곡 상세 연결 · Issue30 (2026-10-16)

CONTENT-001/DATA-001/SONG-001/SEO-001/DESIGN-001: 기존 `/guides/title-artist-variants/` URL과다른5가이드전체본문/확인일을보존한다. 해당글에이전실제공식metadata조회/독립검토의samfree/SAM명의대조보류와Ado逆光의금영생략부제보류를추가한다. 새조회/확인일·명의동일성/음원·권리·심사결과를추정하지않으며기존4출처시각과추가4출처의실제checkedAt를각각보존한다. 이글의편집일만실제재검토일로갱신하고번호조회일과분리한다.

사례→기존2곡상세→업체별공식확인/관측과근거의동선을SSR·360pxJSoff·키보드/44px대상으로검증한다. 기존ledger77·동결자료/3317ID·번호string·URL/storage·unknown/미수록·실패lastgood·원래gradient를보존하며공식검색의성공을번호확정으로표현하지않는다. preview전체noindex/무광고·production본문/색인·같은SHA CI/preview/실제복구·운영을수용한다.

ADS-001 준비점검은실제공식정책/운영증거와계정·권리/문의실수신·법적동의unknown을분리한다. 임의페이지수·전체번호확정·버전1.0·Actions자동배포binding을Google심사필수조건으로만들지않는다. 실제계정의소유권/신청/검토결과없이신청완료/승인을기록하지않는다.

### Issue30 범위 보완 · 운영 분석 안내 불일치

실제운영HTML에서Cloudflare WebAnalytics비콘script를확인했으므로광고없음과분석없음을구분한다. root가추가로 `src/pages/privacy.astro`를단독소유하고공식Cloudflare설명/실제설정·수집미확인범위를대조해'분석스크립트없음'단정을교정한다. 본문이실제운영관측보다넓은cookiefree/개인정보미수집·전체법률준수를주장하지않아야한다. 다른계정/리소스/광고설정은추정변경하지않는다. 실제 `/ads.txt`200/판매자형식1행관측은파일존재와계정일치/소유권·승인증거를구분하며값을생성/로그/교체하지않는다.

### Issue30 운영 브라우저 실패 후 보완 · JavaScript 없는 문의

2026-10-16T10:12:34Z 실제 production 검사38885429674에서29통과/2실패했다. Cloudflare가 곡 제보 mailto를 `/cdn-cgi/l/email-protection#…`로 바꾸어 JS 없는 화면의 직접 제보 링크가 작동하지 않는 실제 제품 결함이다. 로컬31·preview31·운영엄격27 성공으로 이를 대신하지 않는다. 공식 Cloudflare 이메일 난독화 문서(10:14:45Z 실제200)의 특정 HTML `email_off` 범위 제외 방법으로 공개 문의 링크만 보호한다. zone 전체 설정을 변경하지 않는다. 공개되어 있던 동일주소·곡 제목/가수/안정URL·subject/body를 유지하며 JSoff 키보드 제보/문의 링크의 직접 mailto를 실제 새 SHA preview/운영에서 검증한다. root는 Git/공용 하네스·릴리스 통합, 앱담당은 사전배정한 좁은 템플릿 범위, evidence/교체감사는 추가 읽기로 수용한다. v0.4.4 tag/Release는 이 실패 해결·동일 새 SHA CI/preview/운영/실제복구 전 발행하지 않는다.

### Issue30 엄격 비교 보완 · 처리된 이메일 범위 주석

최종후보 M20de2bd 운영에서 공개3직접mailto/원래전체payload 보존과보호href없음을실제확인했지만, Cloudflare는email_off개폐주석을제거했다. 기존verifier는contact의BODY_MISMATCH를17attempt/총60초실패로보존했다. root만 `scripts/verify-deployment.mjs`와 `tests/deployment-healthcheck.test.mjs`를보완한다. 신뢰한expectedartifact에정확한paired email_off로둘러싸인mailtoanchor가있고 actual의동일raw주소·query/entity·label·attribute·개수가맞는경우에만그쌍을복원해전체바이트를대조한다. 임의주석/추가script/다른본문·누락/중복·수신자/제목/본문/label변화는허용하지않는다. 이명시적JSoff직접메일범위의anchor가다시CF보호href로변하면기존복호화정규화로수용하지않고거부한다. 기존unmarked이메일보호계약은계속지원한다. 기존실패를덮지않고새SHA CI/preview/운영/복구를다시검증하며E2E직접mailto요구를유지한다.


## 업체별 공식 확인곡 검색 · Issue35 (2026-10-17)

SEARCH-001/DATA-001/SONG-001/FAVORITES-001/CONTENT-001/DESIGN-001: 기존 number 필터에 `confirmed-tj`, `confirmed-ky`를 추가한다. 검색/홈/저장의 기존 q·has/only·tag·anime/artist·page·history·URL/storage 의미와값을보존하고선택조건은AND로해석한다. 두업체를모두선택하면양쪽에근거가있는곡만통과한다. 전체버튼은number조건을해제하며기존검색어/다른필터는그대로다. 공유URL·새로고침·뒤로가기/조건해제·0건안내와키보드/360px·JSoff비활성안내를검증한다. 선택하지않은업체의관측번호를공식확인으로포장하거나0건을공식미수록으로설명하지않는다.

공식확인판정은기존 `confirmedNumbers`와정확히같이 current/playlist·provider-confirmed·conflict:false·문자열번호·일치하는공식numeric URL·실제근거필드조건을재사용한다. Node전용artist그룹과browser-safe판정을분리하되기존export/API·원검증의의미를변경하지않는다. 원본관측/unknown/suppress·archived/chart-only·근거누락/충돌은공식확인필터에서제외한다. 실제제목·가수·버전검토는projection shape검사로대신하지않는다.

동결차트20/22/24/25위current/playlist4ID의TJ4/KY3관측7번호만실제metadata-only조회후개별검토한다. 기존ledger77항목/순서/전체내용을유지하고근거가확보된confirm/suppress만append한다. Tani Yuuki의없는KY번호나chart-only곡의명의·미수록/새ID·권리를추정하지않고,가져온시각을새차트기간/수집시각으로바꾸지않는다. 실제조회실패/판단보류는별도후보/이유로보존하며마지막정상자료를유지한다. 3317ID/URL/storage·동결staging/chart·번호문자열·unknown/미수록구분·blue#4f7cff→pink#ff7aa8/pinkmintblue·preview전체noindex/광고없음을유지한다. 독립실자료/반례와의미있는unit/type·artifact/browser·두target재현·같은SHA CI/actualpreview·실제old복구→new재적용/운영후의미있는Release만만든다. Google계정/신청/심사는실제결과전unknown이다.

### Issue35 기존 보류 사례 검증의 append 호환

실제 전체 E2E39개 중36통과/3실패는 기존 deferred-guide 검사가 ledger의 총77건을 고정하여 실제 검토7건 추가와 충돌한 결과다. root 소유범위를 `e2e/deferred-guide.spec.ts`로 확장한다. 원77항목의 전체내용·순서를 base2964f7e에서 독립 산출한 고정 SHA256으로 보존 검증하고 최소77건을 요구한다. SAM/Ado의 정확한 원번호·근거·금영보류 검사와 현재 전체ledger의 build-info SHA256 비교는 유지한다. 새항목의 적법성은 기존 전체ledger 계약과 신규 독립 실제자료 검증으로 계속 확인하며 길이 상수만84로 바꾸거나 실패 검사를 삭제하지 않는다. 품질담당은 소유 E2E의 더보기 상한을 동결자료 수와 엄격한 진행검사로 보완한다. 최초실패/수정후 실제결과를 각각 보존한다.
