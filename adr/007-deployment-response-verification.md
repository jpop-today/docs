# ADR-007 · 배포 응답의 같은 버전 검증

상태: 구현 기준 · 2026-10-14 KST · [Issue#15](https://github.com/jpop-today/jpoptoday/issues/15) · RELEASE-001/HARNESS-001 · [ADR-006](006-reviewed-corrections-and-production.md).

## 문제와 실제 근거

v0.3.0의 원본 rollback에서 API canonical 변경 뒤 기본 metadata 응답이 이전 버전으로 남는 사례를 관측했다. 실패한 probe와 실제 원본문/복원 응답은 [릴리스 기록](../operations/releases/v0.3.0-20261014.md)에 보존했다. API 성공이나 한 JSON의 sourceSha만으로 실제 페이지가 같은 배포라고 판단할 수 없다. 잔류 응답의 구체적인 캐시 계층은 확인되지 않았다.

## 결정

관리자는 검증된 고정 dist와 실제 HTTPS 응답을 비교한다. 진단 metadata는 매 요청 새로운 query를 사용하고, 일반 방문자가 사용하는 query 없는 홈·카탈로그·robots·sitemap·대표 상세의 바이트 해시를 비교한다. metadata의 sourceSha/target/dirty와 catalog/ledger/chart digest는 같은 target의 고정 artifact와 일치해야 한다. 검사 전후 metadata도 일치해야 하며, 실제 응답을 비교하지 않은 API pointer 변경은 별도 증거다.

목적지 hostname·HTTPS·포트·URL 자격증명·경로/query/hash를 엄격히 제한한다. redirect를 따르지 않고 ambient credential을 전송하지 않는다. 기존 proxy/CA와 정상 TLS 검증을 유지한다. 요청·전체 실행·재시도·응답 크기의 상한을 두고, 과거 정상 receipt를 덮어쓰지 않는 새 파일에 실패 시도와 최종 결과를 함께 남긴다. 보고에는 안전한 검사명·상태·해시만 보존하며 응답 원문·헤더·자격증명·진단 nonce URL·원래 예외 문자열을 기록하지 않는다.

실제 v0.3.0 대조에서 `/contact/`의 Cloudflare 이메일 보호가 매 응답 다른 HEX와 decoder script를 추가해 원문 바이트가 달라지는 것을 확인했다. [첫 실패](../operations/runs/resume-20261014-2000-healthcheck-failed-email-transform.json)와 [query mailto 후속 실패](../operations/runs/resume-20261014-2000-healthcheck-failed-detail-transform.json)를 보존한다. 고정 artifact에 있는 정확한 mailto anchor만 대상으로, href와 span의 두 HEX를 독립적으로 strict UTF-8 XOR 복호화해 같은 기대주소임을 확인한 보호 형태를 되돌릴 수 있다. 제보 링크는 전체 raw HTML mailto payload(제목·본문·곡 URL·entity 포함)와 원래 label이 정확히 같아야 하며 query를 해석하거나 지우지 않는다. 지정 decoder script 1개를 제거한 뒤 전체 바이트가 같아야 하며 다른 주소·attribute·script·본문 변화는 거부한다. 원문 hash와 되돌린 hash·적용 개수를 모두 기록한다. 카탈로그/metadata/crawler에는 이 HTML 변환을 적용하지 않는다. [보완 뒤 실제 전체 HTTP](../operations/runs/resume-20261014-2000-healthcheck-baseline-complete.json)는 별도 성공 receipt다.

CI의 배포 미리보기와 운영 검증은 같은 source의 같은 target dist를 빌드해 이 helper를 사용한다. 운영 workflow의 CF bindings 누락은 실제 deploy skipped로 구분하며 관리형 클라우드 배포와 verify-only 경로를 유지한다. 비밀 값은 Actions/environment 안전한 설정에서 운영자가 입력하고, 클라우드 proxy placeholder를 복사하지 않는다.

## 대안과 한계

고정 metadata URL만 확인하면 과거 응답을 오판할 수 있다. metadata에만 nonce를 쓰면 실제 본문과 혼합된 버전을 놓칠 수 있다. 모든 URL에 nonce를 붙이면 일반 방문자가 받는 응답을 검증하지 못한다. 전체 CDN purge는 이번 작업의 근거와 필요한 권한이 확보되지 않았으며 사용하지 않는다.

한 검증 요청 묶음의 일치가 모든 CDN 위치의 동시 전환을 보장하지는 않는다. 모든 3,317 상세를 매 원격 검사에 요청하지 않고, 전체 파일은 artifact gate로 확인하고 실제 원격 대표 경로와 브라우저 회귀를 함께 사용한다. Google 계정·심사 상태는 이 기술 검증과 별개다.

## 검증과 인계

stale/mixed metadata, 잘못된 SHA/target/digest, 404/redirect/timeout/과대응답, 잘못된 URL과 비밀 로그, 기존 receipt 보존을 자동 검증한다. 실제 명령·SHA·CI·HTTP·복구 결과는 [실행 인계](../operations/runs/resume-20261014-2000.json)와 [하네스 설명](../engineering/deployment-healthcheck-20261014.md)에 남긴다. 이 ADR 작성 자체를 실행 성공으로 사용하지 않는다.

## 2026-10-16 보완 · JavaScript 없는 공개 이메일 링크

Issue30 실제 운영 브라우저에서 JS-off 제보 링크가 보호 href로 바뀌어 사용 불가능한 결함을 발견했다. 공식 Cloudflare `email_off` 범위로 공개 anchor 세 곳만 제외해 직접 mailto를 유지했다. Cloudflare가 이 개폐 주석을 소비해 `/contact/`의 기존 엄격 비교는 BODY_MISMATCH로 실패했다. [실제 변환 분류](../operations/runs/resume-20261016-1400-contact-fix-marker-transform.json)는 동일 normal Node fetch에서 contact/privacy 전체 바이트가 expected의 정확한 paired 주석만 제거했을 때 일치함을 확인했다. [일반 HTTP 이메일](../operations/runs/resume-20261016-1400-contact-fix-production-email-observed.json)은 세 공개 anchor의 원래 전체 raw payload 보존을 확인했다.

새 정규화는 신뢰한 artifact의 정확한 paired email_off mailto anchor에 한해, actual의 동일 원시 주소·query/entity·label·attribute·개수인 직접 mailto에 주석만 복원한다. 그 뒤 전체 바이트 비교와 raw/normalized/expected hash를 유지한다. 다른 주석·본문·스크립트·수신자·query·label·개수 변화를 제거하거나 의미상 동일하게 만들지 않는다. 불완전한 marker 쌍도 허용하지 않는다. 이 범위의 링크가 다시 CF 보호 href로 바뀌면 기존 XOR 복호화로 수용하지 않는다. 같은 identity의 marked/unmarked anchor가 섞여 위치 예산이 모호하면 안전하게 실패한다. 이전 unmarked CF 이메일 계약은 유지한다.

실제 JS-off E2E는 직접 mailto와 원래 곡 subject/body·문의/개인정보의 동일 수신 주소·키보드 초점을 계속 요구한다. 정규화의 성공을 링크 사용성으로 대신하지 않는다. 최초 운영29/2와 후속marker 비교17attempt 실패, 이후 새 SHA 검증을 각각 보존한다. 공식 근거·최종 검증은 [Issue30 스펙](../product/spec.md)과 [실행 기록](../operations/runs/resume-20261016-1400.json)을 따른다.
