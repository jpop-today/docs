# ADR-006 · 근거 있는 번호 정정과 실제 운영 전환

2026-10-14 KST · accepted · DATA-001/SEARCH-001/SONG-001/SEO-001/RELEASE-001/ADS-001 · source Issue#13(계속 작업), Issue#6.

## 목적과 결정

미리보기만 배포한 상태에서 종료하지 않고 실제 jpop.today에 새 앱을 반영한다. 원본 staging과 최소3317 ID를 보존한 채 공식 대조 결과를 별도 reviewed correction ledger로 적용한다. 틀린 것으로 확인한 번호는 복사를 막고 unknown으로 표시하며, 새 번호나 확인된 번호는 정확한 제공사 근거·실제 확인일을 함께 표시한다. 전체 카탈로그가 공식 확인됐다고 설명하지 않는다. 날짜·권리·가수/버전 정체성·canonical alias를 만들어내지 않는다.

## 내부 ledger 계약

data/reviewed/provider-corrections.json은 {ledgerVersion:1,purpose:"reviewed-provider-corrections",legacyRevision:<staging.revision>,entries:[...]}다. entry는 {songId,provider:"tj"|"ky",action:"confirm"|"suppress",expectedObservedNumber:string|null,number:string|null,evidence:{url,checkedAt,title,artist,responseSha256},reason:string}다. 공식 number 검색 HTTPS URL·실제 확인 날짜(YYYY-MM-DD 또는 ISO 시각)·제목/가수·응답SHA256을 보존한다. suppress는 number:null이고 기존관측번호를반증한근거다. confirm은실제공식번호string이며원본unknown에서추가할수도있다. 같은 ID/provider 중복·없는ID·원본관측값불일치·잘못된출처/날짜·추측날짜/미수록은 거부한다. 원본/staging/저장키는 수정하지 않는다.

공개 read model은 corrections가 있을 때 version2다. 번호의 기존키 observedNumber는 표시/검색/복사할유효값으로 유지한다. provider에 evidence:null|{url,checkedAt,title,artist}, correction:null|{reason,previousNumber}를추가하며 상태는 observed-unverified/provider-confirmed/unknown이다. 원문응답·내부hash/refs·가사·외부이미지는 공개하지 않는다. 공식일치선언은검토한행에만적용한다. suppress/정체성보류에서null을미수록으로표시하지않고인접근거/이유를설명한다.

## 차트와 색인

차트는 snapshot rawalias와처음원본observation tuple로seed한canonicalId로연결하고원본배열index에의존하지않는다. 최신raw/제공사snapshot에서source/실제fetchedAt/요청기간/확인기간구분과100개고유ID·정수순위·기존ID/번호/제목가수대응을미래fetchedAt도거부해원자적으로승격한다. 새ID·번호만같은다른곡·부분/쓰기실패는마지막정상snapshot을보존하고자료검토로돌린다. live수집성공 없이현재빌드시각으로수집일을바꾸지않는다.

JPOP_BUILD_TARGET=preview|production으로동일소스의검증대상을명시한다. preview는전체noindex·차단robots·광고없음. production은홈/검토가이드/정책/FAQ·유용한범위/차트와실제공식근거가있는정적상세만색인/sitemap에포함한다. 검색/개인저장/호환어댑터/보관·미확인상세/404는noindex다. 수천개관측상세를고유편집콘텐츠로포장하지않는다. production의광고없음과AdSense실제신청/승인은별도상태다. build-info는target와catalog/ledger/chart hash·SHA·dirty를연결하고artifact검사는mode별HTTP/metadata/public필드를검증한다.

## 운영 전환과 배포 소유권

기존todayjpop·jpop.today·HTTPS origin을유지하여localStorage를읽는다. Cloudflare공식문서가허용하는기존Git연결project에Wrangler검증artifact를올리는경로를사용한다. 전환시개인repo의자동production/preview배포를중단해새앱덮어쓰기를막고Git코드·수집이력·기존배포를보존한다. 도메인/DNS이동이나프로젝트삭제를기본경로로삼지않는다. 소스는org main·검증동일SHA이며Cloudflare의legacyGitsource metadata와실제artifact소유권을분리기록한다.

org CI가실제credentials로자동운영배포할수있는지검증한다. 로컬managedbinding을GitHubSecret으로복사하지않는다(placeholder일수있고대상도메인경계가있음). RepoActionssecret이준비되지않아도관리자cloudbinding으로검증된배포를진행하고자동CI부분의실제차단만기록한다. 전환전직전정상배포/자동배포설정·복구경로를확보하고실제운영HTTP/브라우저로같은SHA·검색/복사/기존주소/저장/404/robots/sitemap/자료경계를검증한다.

권한이나지원API문제는실제응답과영향을기록하며임의삭제·TLS무시·무근거우회·forcepush를하지않는다. 이미위임된루틴수정/PR/병합/배포에추가허가를요구하지않는다. Google본인정보·약관/법적동의와실제승인통지는운영자가정확히입력할별도단계다.

## 검증

정정계약/반증·ID/raw불변·날짜/출처·unknown복사없음·검색상태/근거표시, 차트순서변경/부분/쓰기실패/원본ID호환, preview/prod메타/robots/sitemap·raw/ops/가사/이미지제외, 같은SHA CI/preview/production·복구시험·자동덮어쓰기방지와실제사이트스모크를 Issue#13에연결한다.
