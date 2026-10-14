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
| RELEASE-001 | 동일 SHA의 CI→미리보기→운영과 복구 증거 | dist 전용 배포, 비공개 원본 404, 호환성 스모크, 이전 정상 deployment 복구 확인 |
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
