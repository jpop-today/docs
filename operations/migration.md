# 개인 저장소에서 jpop-today로 이관

상태: 이관 절차 설계. 원본 조사일 2026-10-13, 한국 시간. 실제 코드 이관·운영 전환·배포 완료는 각각의 실행 기록과 SHA로 확인한다.

관련 [제품 스펙](../product/spec.md): SRC-001, DATA-001, HARNESS-001, SEARCH-001, SONG-001, FAVORITES-001, CONTENT-001, SEO-001, PRIVACY-001, RELEASE-001, ADS-001. 구조·ID·공개 범위의 결정은 [ADR 목록](../adr/README.md), 검증은 [하네스](../engineering/harness.md), 실제 릴리스 절차는 [배포·복구](deployment.md)를 따른다.

사용자는 `jpop.today`의 재설계, 여러 스레드 작업, 코드·문서 변경과 배포를 위임했다. 일상적인 구현·검증·미리보기·운영 전환에 같은 허락을 반복해서 묻지 않는다. 이 절차의 검증 조건은 위임된 전환을 정확하게 수행하기 위한 품질 기준이다. 계정 접근·도메인 제어·권리 증거 같은 실제 누락 항목은 독립 작업을 진행한 뒤 필요한 항목만 요청한다.

## 저장소별 책임

| 저장소 | 책임 |
| --- | --- |
| `jpop-today/.github` | 조직 프로필, 작업 규칙, Issue/PR 양식, 공통 검증·리뷰 기준 |
| `jpop-today/docs` | 제품 스펙, ADR, 데이터 기준, 조사, 검증 하네스, 이관·배포·운영 기록 |
| `jpop-today/jpoptoday` | 사이트 코드, 콘텐츠·데이터 원본, 수집·검증·빌드 도구, 앱 CI와 배포 설정 |
| `kim75503/todayjpop` | 출처와 이전 운영 구현·이력; 전환 완료까지 기존 서비스 복구 기준 |

세 새 저장소의 기존 commit 이력을 보존한다. 원본 기본 브랜치가 `dev`라는 이유만으로 새 조직의 기본 브랜치를 바꾸지 않는다. 원본 checkout을 읽기 전용 조사 위치로 유지하고 원본에 bot push·강제 push·코드 변경을 하지 않는다. 이관 작업은 기존 클라우드 checkout과 작업 브랜치를 사용한다. 매 작업마다 별도 Git worktree를 만들 필요는 없다.

## 확인된 baseline과 아직 필요한 정보

| 구분 | 값·상태 |
| --- | --- |
| 앱 코드 기준 | 원본 `dev` SHA `6dbe5a65d1bd729c09393f746f715f4363089a43`; `main`과 앱 로직 동일 |
| 이전 `main` 기준 | `c57a7ae1be0c6cff06c847bb7dbc5c572e631edd` |
| 등록곡 | 3,299행, 업체별 확인일·검증 증거 없음 |
| 최소 호환 ID | 위 두 SHA의 공개 곡/차트 합집합 3,317개 |
| 공개 저장 키 | `jpop_today_favorites`, `jpop_today_recent_filters` |
| 기존 공개 도메인 | `https://jpop.today` |
| 실제 운영 배포 | todayjpop, kim75503/todayjpop dev, SHA6dbe5a65d1bd729c09393f746f715f4363089a43, deployment5072e2ec-6858-4c44-b34f-8c27c3a016e8, npm run build→dist |
| 기존 Cloudflare 복구 | 직전 성공 deployment394fc369-7263-403a-a72a-dfebc9889760 조회; 실제 rollback 실행/권한은 미검증 |
| GitHub 제어면 | Git/Issue/PR/merge/CI 조회 성공; deployments API는 integration scope403 |
| 외부 자료 권리 | 가사 7곡·외부 차트 이미지의 개별 사용 허락 증거 미확인 |

[현재 구조 조사](../discovery/current-state.md)의 수치는 저장소 관찰이다. 운영 SHA를 확인하면 그 SHA의 ID·URL·설정도 baseline에 추가한다. `main`·`dev` 두 HEAD의 합집합만으로 과거 모든 주소를 보존했다고 주장하지 않는다.

## 1. 원본과 운영 증거 고정

1. 원본 SHA별 파일 manifest와 주요 입력·생성 데이터의 SHA-256을 기록한다. source repo, ref, SHA, 경로와 import 시각을 함께 보존한다. import 시각을 번호 확인일로 쓰지 않는다.
2. `main`과 `dev`의 `js/songs.js`, `js/chart.js`를 각각 읽어 ID, 원제, 가수, 업체 번호, 기존 곡 URL을 추출한다. 최소 3,317 ID의 fixture 또는 mapping을 새 소스에서 관리한다. 가사 본문은 이 호환 fixture에 넣지 않는다.
3. 이전 운영 deployment의 Cloudflare 프로젝트·배포 ID·SHA·빌드 설정을 확인한다. GitHub 기본 브랜치와 Cloudflare 운영 브랜치를 각각 기록한다.
4. 실제 운영 사이트에서 주요 URL, `.html` 주소, 404, public data 경계, canonical, sitemap, ads.txt, 이미지·가사 요청, 광고 태그와 헤더를 읽기 전용으로 확인한다. 접근 실패는 검증 성공으로 대체하지 않는다.
5. 전환 전 정상 배포와 되돌릴 수 있는 방법을 기록한다. 복구 대상은 추정한 최신 Git SHA가 아니라 실제 서비스 중인 정상 artifact다.

현재 네트워크에서 운영 사이트 또는 제어 API가 막히면 앱·데이터 이관과 로컬 검증은 계속 진행한다. 실제 접근이 필요한 운영 SHA 확인·전환·운영 smoke는 미확인 상태로 남긴다. 네트워크 허용 또는 계정 binding이 준비되면 해당 작업을 다시 실행한다.

## 2. 코드·데이터를 새 소스에 이관

원본과 새 저장소는 독립 이력이므로 새 저장소의 이력을 지우는 덮어쓰기나 강제 push를 하지 않는다. 앱의 필요한 원본을 일반 commit으로 이관하고 고정 source SHA와 필요한 라이선스 고지를 provenance 기록에 남긴다. 전체 Git 이력이 제품·감사에 필요하면 별도 ADR로 원본 이력을 합치는 방법을 선택한다. 원본 이력 링크는 항상 남긴다.

원본 `.idea`, 개인 컴퓨터 경로 의존 설정, `.DS_Store`, MCP 로컬 설정, cache·`node_modules`·개발 산출물은 제품 원본으로 이관하지 않는다. 기존 새 소스 하네스·테스트·`AGENTS.md`는 역할에 맞게 통합한다. 원본 앱을 가져오기 위해 이미 있는 독립 계약 파일을 임의로 삭제하지 않는다.

등록 번호는 문자열로 보존한다. 빈 번호는 `unknown`이며 공식 확인이 없는 상태에서 `unlisted`로 변환하지 않는다. 기존 imported 번호가 있다고 새 하네스의 `verified`를 충족하는 것으로 만들지 않는다. 출처·확인일 없는 legacy 자료는 별도의 imported/미확인 상태와 provenance로 표현하거나 증거 보강 전 공개 검증 카탈로그에서 분리한다. 새 스키마가 사실을 표현하지 못하면 스키마를 명시적으로 개선한다.

등록곡 3,299행과 차트 snapshot은 의미를 구분한다. 차트 출처는 TJ이며 KY 순위로 표시하지 않는다. 차트의 요청 기간, 실제 응답에서 확인한 기간, 수집 시각을 별도 필드로 다루고 근거가 없는 확인 상태를 생성하지 않는다.

가사·외부 이미지의 소스 provenance와 공개 허락은 별도로 관리한다. 허락 범위가 확인되지 않은 가사 본문을 공개 카탈로그나 검색 payload에 복제하지 않는다. 검증된 허락이 없는 외부 앨범 이미지는 자체 placeholder 등 대체 표시를 사용한다. 원본 아이콘을 유지하면 Lucide/Feather 라이선스 고지도 보존한다. 원본 Git 자료 보존과 새로운 공개 artifact 범위를 따로 기록한다.

완료 기준은 실제 입력과 source manifest, 기존 ID mapping, 공개 경계 검증이 review 가능한 commit으로 존재하는 것이다. 합성 샘플만 있는 상태는 이관 완료가 아니다.

## 3. ID·URL·저장 호환성 구현

기존 해시 ID는 mutable 메타데이터에서 재생성하지 않는다. 이관 시 그대로 보존하고 내부 안정 ID가 필요하면 `legacyId → canonicalId` mapping을 추가한다. 번호·제목·가수 정정은 mapping을 함께 갱신한다. 이름을 고친 뒤 기존 곡 주소가 사라지면 실패다.

| 대상 | 새 구조가 만족해야 할 동작 |
| --- | --- |
| `/song?id=<legacyId>` | 같은 곡으로 연결하거나 근거 있는 이전 주소 안내; silent 다른 곡 연결 금지 |
| `/filter?q=...` | 한글·일본어·업체 번호·복합 검색 유지 |
| 반복 `number`·`tag`, `anime`, `artist`, `page` | 기존 조건 해석 또는 의미가 보존된 redirect |
| `/#favorites-view` | 기존 즐겨찾기 화면으로 연결 |
| `.html` 주소 | 기존 query를 유지하며 정규 주소로 redirect |
| 문자열·`{ id }` 저장 목록 | 모두 읽고 검증된 alias로 연결 |
| 미해결 ID | 원본 저장값 보존; 사용자가 알 수 있는 복구·미해결 상태 제공 |
| 최근 필터 | 최대 5개와 예전 snapshot을 읽는 계약 보존 또는 버전 이관 |
| HTTPS `jpop.today` | 같은 origin과 기존 저장 키 유지 |
| preview 도메인 | 테스트 fixture로 저장 이관 확인; 운영 사용자 데이터가 자동 공유된다고 설명하지 않음 |

로컬 저장 migration은 원본 값을 남겨 복구할 수 있게 하고 실패·quota·잘못된 JSON에서도 검색·저장 UI를 복구한다. 중복 곡 통합은 제목 유사도나 번호 단독 매칭으로 자동 확정하지 않는다. KY 충돌 7개와 TSV 인용 오류 3곡은 독립 correction Issue로 관리하고 공식 근거·변경 전후·ID mapping을 남긴다.

## 4. 구현과 하네스 검증

기존 앱의 검증을 새 계약과 통합한다. 테스트 fixture는 직접 작성한 합성 가사·허락된 데이터만 사용한다. 실제 legacy 카탈로그와 합성 계약은 결과를 별도로 보고한다.

| 검증 | 완료 증거 |
| --- | --- |
| 데이터 계약 | 실제 imported 카탈로그 count·번호 문자열·상태·provenance 검사; 확인일 생성 없음 |
| 매칭 | 버전·가수·언어가 다른 번호 충돌에서 잘못된 자동 병합 방지 |
| ID 보존 | 최소 3,317 legacy ID 및 운영 SHA 추가 ID mapping 검증 |
| 검색 | 한글·원제·가수·번호·복합 검색; 빈 결과 복구 |
| 저장 | 문자열·객체 이전 형식, 미해결 ID, 저장 차단, 손상 JSON, alias 이관 |
| URL | 기존 주소·필터·query·hash와 뒤로가기 복원 |
| 공개 artifact | raw·도구·비공개 가사 제외; 허락 없는 본문·이미지 URL 제거 |
| 정적 내용 | JavaScript 없이 차트·가이드·정책 읽기; 페이지별 SEO·canonical·sitemap |
| 접근성·반응형 | 키보드, focus, 복사 상태, 320px 이상 모바일, desktop·WebKit 검사 |
| 배포 | Pages preview artifact에서 실제 redirect·404·헤더·캐시·source 비공개 확인 |
| 신선도·실패 | 재수집 실패가 정상 snapshot·서비스를 지우지 않음 |

빌드 성공, 테스트 성공, 미리보기 성공, 운영 배포 성공, AdSense 승인 상태는 서로 다른 결과다. 각 실행에 명령·날짜·SHA·대상 URL·실행 여부·실패 원인을 남긴다. 원본 테스트에서 광고 스크립트를 대체한 것은 실제 광고·동의 동작의 검증이 아니다.

원본 `npm run build`가 tracked 생성물을 수정하는 점을 고려한다. 새 구조는 generated 산출물을 재현 가능하게 만들고, CI가 새 source commit과 다른 코드·데이터를 배포하지 못하도록 manifest와 source SHA를 연결한다. Node 및 package manager 버전은 lockfile과 CI에 맞춰 고정한다.

## 5. Cloudflare preview와 운영 전환

새 소스 저장소 연결, build command, output directory, Node 버전, 환경 변수·secret binding을 실제 계정에서 확인한다. 기존 README의 `dist` 권장값은 현재 Cloudflare 설정의 관찰 결과가 아니다. 자격 증명은 환경의 기존 binding을 먼저 사용하고 값은 문서·commit·로그에 넣지 않는다.

preview 배포에는 Git SHA, artifact hash, 빌드 로그, preview URL과 smoke 결과를 기록한다. 새로운 구조가 만드는 모든 공개 route와 기존 redirect mapping을 preview에서 검사한다. 존재하지 않는 route와 `/data/jpop-playlist.raw.json`, 개발 파일 경로는 실제 404여야 한다. 저장소 루트를 직접 배포하면 실패다.

preview가 통과하고 이전 정상 배포의 rollback 방법을 확보하면 위임 범위에서 `jpop.today`를 새 artifact로 전환한다. 기존 서비스 도메인과 저장 키는 유지한다. 실제 계정 권한·네트워크·배포 target이 없으면 전환만 차단 상태로 남기고 완료된 이관·검증 결과를 제공한다.

전환 직후 홈, 검색, 원본 곡 주소, 즐겨찾기, 차트, 가이드, 문의, 개인정보, robots, sitemap, ads.txt, 404를 운영 도메인에서 확인한다. HTTPS apex와 `www` 이동, query 보존, 노출 데이터, 광고 설정을 검사한다. 캐시 때문에 이전 HTML/JS가 섞이지 않는지도 확인한다.

## 6. rollback과 이전 수집 중단

다음 현상이 나타나면 새 artifact의 운영 확대를 멈추고 확인된 이전 정상 배포로 복구한다: 주요 검색·번호 기능 불능, legacy 주소·저장 대량 유실, 잘못된 곡 연결, raw/허락 없는 본문 공개, 404 동작 오류, 배포 데이터 무결성 실패.

복구는 이전 Cloudflare deployment/artifact 선택을 우선한다. 이전 배포가 재선택 가능한지 실제 제어면에서 미리 확인한다. Git revert만 했다고 CDN과 운영 사이트가 복구되었다고 기록하지 않는다. 운영 smoke를 다시 실행하고 복구 배포 ID·SHA·시각·원인·영향을 기록한다.

전환 안정성을 확인한 뒤 원본 예약 수집 workflow와 새 workflow가 동시에 운영 데이터를 갱신하지 않도록 소유권을 정리한다. 원본 job 비활성화는 실제 새 job·배포 연결이 정상임을 확인한 후 대상과 변경 내용을 기록하여 수행한다. 원본 이력과 복구 snapshot은 삭제하지 않는다.

## 7. 운영 기록과 AdSense 재검토 연결

이관 완료 기록에는 source SHA, 새 소스 SHA, docs/spec version, 호환 ID 개수, dataset manifest, 권리 제외·포함 자료, preview·운영 deployment ID, 검증 결과, rollback 대상, 남은 데이터 정정·접근 차단 사항을 넣는다.

새 GitHub·Cloudflare 배포 성공은 Google AdSense 승인과 별개다. 기존 publisher 정보의 소유 계정과 실제 사이트 상태를 확인하고, 검색에서 확인 가능한 고유 콘텐츠·정확한 운영 안내·자료 권리·광고 설정을 검증한 뒤 재검토 단계로 연결한다. 승인 여부는 Google의 실제 상태와 결과 시각을 기록한다. 임의의 글 수·배포 횟수·방문자 수를 승인 보장 조건으로 만들지 않는다.

## 실행 상태 체크리스트

- [x] 원본 `main`·`dev` SHA와 구조 조사
- [x] 3,299 등록곡, 출처·확인일 부재, KY 충돌, 가사·이미지 상태 확인
- [x] 두 SHA의 최소 legacy ID 3,317개 집계
- [x] 실제 Cloudflare 프로젝트·운영 branch/SHA/deployment와 직전 성공 복구 후보 조회(rollback 실행은 미검증)
- [x] 두 조사 SHA의 원본 파일 hash·refs와 legacy ID 보존 기준선을 새 소스에 저장(실제 운영 SHA 추가 확인과 canonical alias 결정은 미완료)
- [ ] 실제 앱·카탈로그·허락 범위에 맞춘 공개 자료 이관
- [ ] 새 계약·기존 URL·저장 호환성을 실제 자료로 검증
- [ ] Cloudflare preview 배포와 smoke
- [ ] `jpop.today` 운영 전환과 smoke
- [ ] 새 수집·배포 소유권 확인 후 원본 예약 갱신 정리
- [ ] Search Console·AdSense 실제 계정 상태와 다음 심사 단계 기록

이 목록은 문서 작성 시점의 조사 상태다. 작업이 진행되면 근거가 있는 실행 기록을 연결해 갱신한다.

## 구현된 첫 이관 범위

[staging 계약](../data/legacy-staging.md)과 [ID/URL/storage 계약](../engineering/legacy-compatibility.md)은 실제 코드와3,317 ID로 검증했다. 현재 라이브러리 결과를 새 앱에 연결하기 전이며 shared-number 후보를 canonical alias로 확정하지 않았다. 실제 자료 승격·공개 경계·preview/운영 검사는 해당 단계에서 계속 수행한다.
