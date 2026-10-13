# 실행 상태

갱신일: 2026-10-13 KST. 문서·검증·원격 전달·병합·운영 배포·AdSense는 각각 기록한다.

## 위임과 현재 단계

사용자는 스펙·전체 재설계·수정·검증·배포와 여러 스레드 협업을 위임했다. 일상 작업의 반복 확인 없이 진행한다. 현재 FOUNDATION의 제품 기준·원본 조사·데이터/호환성 하네스를 작성·검증하고 저장소별 PR로 통합한다. 조직 앱 이관·새 UI·운영 전환은 다음 단계다.

## 소스와 조사 기준

| 저장소 | 작업 시작 SHA | 작업 branch |
| --- | --- | --- |
| jpop-today/docs | f3dbac224b92593751a56b51c6f6f07a1228d879 | codex/project-foundation-20261013 |
| jpop-today/.github | 091580ca462e51e86a3b68a782fcd15759a1e216 | codex/project-foundation-20261013 |
| jpop-today/jpoptoday | 397fd3a90eae2224b4e918e649652b72752ed415 | codex/project-foundation-20261013 |
| kim75503/todayjpop dev | 6dbe5a65d1bd729c09393f746f715f4363089a43 | 읽기 전용 원본 조사 |
| kim75503/todayjpop main | c57a7ae1be0c6cff06c847bb7dbc5c572e631edd | 읽기 전용 원본 조사 |

## 작업 계약과 결과

| 작업 ID | 담당 범위 | 결과 |
| --- | --- | --- |
| [docs#2](https://github.com/jpop-today/docs/issues/2) / LOCAL-FOUNDATION | 조정자: product/architecture/ADR/content/배포·심사 기준·통합 | 스펙 11개 요구와 ADR 3개, 작업 큐·실행 기록 작성 |
| LOCAL-SOURCE | 원본 조사: discovery/current-state.md, operations/migration.md | 등록 3,299곡, 최소 legacy 3,317 ID, KY7번호 충돌·TSV3표기 문제·권리/출처 누락 확인 |
| LOCAL-HARNESS | 계약·scripts·tests·CI, data/catalog-contract.md, engineering/harness.md | 안전 snapshot 승격 구현·파일 I/O 실패 보존 검증; 원본 legacy 3,317 ID 기준선 및 저장 무결성 검사 구현 |
| LOCAL-GOVERNANCE | 조직 규칙·템플릿, operations/collaboration.md | 스레드/file/base SHA 계약, 단일 Git 조정자, handoff와 상태 구분 |

공유 checkout의 Git 조작은 조정자만 수행했다. 기존 사용자 변경은 작업 시작 시 없었고 원본 조사 checkout은 변경하지 않았다.

## 확인한 기능

- 조직 하네스: 최종 55개 전부 통과(Node24.19), synthetic CLI 검증·승격·재검증 통과, 실제 legacy 3,317 ID 기준선 --check 일치. Node22 GitHub CI push/PR 모두 success: https://github.com/jpop-today/jpoptoday/actions/runs/38339116656.
- 개인 원본 복사본: 빌드 16페이지/12sitemap, 문법25파일, Node12테스트 통과.
- 개인 원본 Chromium: 33통과/3의도적skip/0실패, 운영 대표 흐름2개 추가 통과. WebKit·실제 광고는 미실행([상세](../engineering/baseline-qa.md)).
- 세 선택 repo 및 개인 원본 Git read 성공. API 접근·Git push·배포 권한과 구분한다.

## 원격 전달과 외부 차단

조직 규칙 foundation 커밋 49d279dd24ddb7ecdbaf46409d0d4ebc9f69e11d와 소스 커밋 7233949ca0a5ef7e7af9d3c6944e5a743715c53d를 push하고 각 PR #2를 생성했다: https://github.com/jpop-today/.github/pull/2 · https://github.com/jpop-today/jpoptoday/pull/2. 두 PR은 검증 후 병합했다. 조직 merge SHA=e3588f6ce4c72270b6aab3f0ad1a8dc382c72a00, 소스 merge SHA=46604141c9afa6a57215c6cbfb58f0dbceaafab6. Docs 설계 기준은 [PR#3](https://github.com/jpop-today/docs/pull/3)로 연결한다. 이 문서 자체의 최종 병합 상태·merge SHA는 해당 PR의 실제 상태가 기준이다. 현재 초기 GitHub API/운영 사이트는 CONNECT403이었으나 후속 요청에서 API·운영 HTTP 접근이 성공했다. GitHub 배포 목록 API는 integration scope 403으로 아직 확인할 수 없다. 운영 /, 공개 JS·robots·sitemap은200, 원본 data 경로와 없는 URL은404이며 공개 JS 3개는 조사 dev 바이트와 같다. 실제 Cloudflare deployment SHA를 이 바이트 비교로 확정하지 않는다. 계정 본인정보·AdSense 심사 화면·실제 Cloudflare 프로젝트/운영 branch/SHA는 미확인이다.

환경 설정 draft에 api.github.com, jpop.today, support.google.com, developers.google.com, api.cloudflare.com, developers.cloudflare.com, www.tjmedia.com을 추가했다. 기존 package-manager preset은 유지했다. draft 저장은 runtime 적용/publish가 아니므로 사용자 환경 설정의 저장·발행 후 해당 요청을 다시 시험해야 한다. CF accounts API는 정상 도달했으나 인증 헤더 부재 오류9106/9107을 반환했다. 환경 draft에 CLOUDFLARE_API_TOKEN proxy-secret requirement(api.cloudflare.com)와 CLOUDFLARE_ACCOUNT_ID 변수 requirement를 저장했다. 현재 CLOUDFLARE_API_TOKEN/CLOUDFLARE_ACCOUNT_ID/CF_API_TOKEN/CF_ACCOUNT_ID는 없다. 새로운 비밀 값을 채팅에 요청하거나 만들어 기록하지 않았다.

## 다음 실행

1. Foundation의 세 PR과 실제 CI 결과를 기준으로 다음 구현을 진행한다. source main의 현재 protected=false는 확인했으며 정책 문서 작성이 보호 설정 적용을 뜻하지 않는다.
2. 실제 CF 프로젝트·운영 SHA를 고정하고 최소 identity baseline에 운영 ID를 추가한다.
3. [소스 Issue#3](https://github.com/jpop-today/jpoptoday/issues/3)에서 raw staging·증거 보강과 KY충돌 대조, ID/URL/storage 어댑터를 구현한다.
4. 최소 Astro 정적 앱→회귀 하네스→Cloudflare preview/복구→같은 SHA 운영으로 진행한다.
5. 콘텐츠·권리·개인정보·크롤링 준비 후 실제 AdSense 계정/심사를 추적한다. 현재 승인 상태는 unknown이다.
