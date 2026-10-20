# v0.4.10 콘텐츠·정책 게이트

[Issue51](https://github.com/jpop-today/jpoptoday/issues/51) · [ADR008](../adr/008-favorites-file-backup.md) · [실행](runs/resume-20261020-2204.json).

## 제품 내용과 자료 경계

번호 메모로 복원할 수 없던 stable ID·미해결 ID·기존 부가 정보를 수동 JSON 파일로 보관하고, 로컬에서 개수를 검토한 뒤 현재 목록에 추가한다. 선택/해석은 읽기 전용이고 합치기는 기존 항목·순서·중복을 유지하며 새 ID만 추가한다. 임의 제목/번호로 identity를 합치거나 파일의 metadata를 공식 번호·권리 확인 근거로 사용하지 않는다. 잘못된 형식/version/ID·256KiB 초과·동시 변경·저장 거절·복구 미확인을 구분한다.

root는 구현·개인정보 안내·가이드를 읽고 실제 1440px/360px 화면과 단위·브라우저 증거를 확인했다. build-your-setlist만 실제 최종 검토일21일로 보강했고 다른5가이드의 전체 객체/날짜/출처는 원문 bytes와 같다. 현재 곡 자료에서 찾은 ID 수와 기존 즐겨찾기에 겹친 수를 구분하며 번호 메모는 별도 용도로 남긴다. 파일은 기기에 내려받고 선택한 파일을 브라우저에서 처리한다. 파일에는 기존 부가 정보가 포함될 수 있고 사용자 보관·공유·별도 파일 삭제가 필요함을 개인정보 안내에 반영했다.

공식 원장99·안전한40ID/74번호·동결 raw/chart·3317ID·번호 string·URL/저장 키·브랜드는 변경하지 않았다. 직전 v0.4.9 공개 catalog의 모든 bytes와 같음을 별도로 대조했다. 새 공식 조회/확인일·권리·추천·Google 결과를 생성하지 않았다. unknown은 미수록이 아니다.

## 실제 검증과 후속 보완

초기 단위568 중1실패는 의도한 setlist 변경에 대한 이전 객체 hash 요구였고, 기존4개 비관련 객체와 chart 날짜/자료·브랜드 hash 검사는 유지하면서 새 guide 사실을 별도로 검사했다. root의 영어 ID 문구 assertion 실패·네트워크 격리 테스트 중단·JS-off 이전 exact 안내 실패·실제 사용량 중단은 원로그/영수증으로 보존한다. 최종 로컬 단위573/타입26파일 오류0, 전체74 브라우저와 별도 복구6, 최종 파일/복구22를 확인했다. 이 로컬 기록을 확정 SHA CI/운영 검증으로 확대하지 않는다.

실제6peer40/40/39·원파일 hash 대조 후 품질active를 제외·interrupt하고 신규교체1명을 영입했다. 복구 실패5조건과 정상 대조를 실제 브라우저에서 주입해 원문 bytes/후속작성자/화면 경고/실제 파일 구출을 검사했다. 처음6개는 좁은 범위의 통과였으며, 별도 고정 설명의 저장값 보존 오해는 실제 화면 읽기로 발견해 root가 한 구절만 보완했다. 강화6개·후속 두 읽기평가20/20을 확인했다. 원6평가/소스 snapshot/처음 통과와 발견·수정 이력은 보존했고 물리 삭제를 주장하지 않는다.

원격에서 기존 통계를 파일 전송으로 오인하지 않도록 test-only request 검사를 보완했다. 실제 public Cloudflare beacon의 GET200/31853bytes와 hash/endpoint literal을 읽었으며 실제 POST 전달 관측은 아니다. 정확한 endpoint·POST·JSON shape만 구분하고 모든 요청의 지정된 합성 file marker를 검사하며 QA 통계 endpoint를204로 가로챈다. 동료가 실제 재현한 mixed-percent/JSON-unicode 검사 우회2건과 첫 확장3/4실패를 보존하고 root 보완 뒤 단위4/브라우저22·두 최종 읽기평가20/20을 확인했다. 이는 지정 fixture/encoding의 회귀검사이며 모든 개인정보·암호화·압축 전송 부재를 입증하는 범용 방화벽이 아니다.

## 운영자와 배포 경계

Google 정책의 과거 실제 읽기와 운영자 계정 단계는 별도다. 권리·소유권·문의 실수신·기존 ads.txt의 계정 일치·본인정보/약관/법적 동의·실제 Google 신청/심사 결과는 운영자 단계이며 unknown이다. 글 수·트래픽·확인곡 수·새 기능이나 자동 CI를 보편적인 승인 조건으로 만들지 않는다. 광고를 요청하거나 클릭/노출 테스트를 하지 않는다.

기존 원본 ads.txt 59bytes/hash는 production에서만 유지하고 모든 preview는 noindex·광고 없음이다. 개인 Git source metadata/dev·자동 false/none·도메인·이력을 보존한다. 현재 확인한 관리형 환경과 Actions의 binding 상태를 구분하며 credential을 복사하지 않는다. 기존09시/hourly 예약을 변경하지 않고 무기한 세션·정시 성공을 보장하지 않는다.

확정 main SHA의 CI/두 target 전체 bytes 재현·실제 preview/운영·실제 이전v0.4.9 복구→새 결과 재적용·최종 사이트와 Release는 실행의 실제 후속 영수증으로 수용한다. 이 문단 작성 시 해당 배포는 아직 실행하지 않았다.

## 첫 운영 검증 실패와 보완 · 2026-10-21 KST

M1 `74e36979981a3f5d0f7d855fe81e2c9731f2351c`의 main CI80+80·실제 preview80·운영 strict28/1은 통과했으나 production workflow39514342581의 실제 origin 브라우저는73pass/7fail이었다. 모두 새 백업/복구 검사의 uploads 배열에 `unexpected-write` 한 건이 남은 실패다. 해당 로그에는 요청 주소/본문이 없으므로 그7건의 실제 요청을 소급 확정하지 않는다. v0.4.10 발행은 보류하고 이전 정상 v0.4.9 b2b87099/c6903c8로 실제 복구해 strict28/1을 통과했다. M1 배포와 실패 영수증은 보존했다.

별도 읽기 전용 운영 브라우저에서 query 없는 `https://jpop.today/cdn-cgi/rum` POST·eventType1·pageloadId/location/siteToken 문자열 형식을 관측했다. 현재 관리형 proxy 공개 CA를 Chromium 신뢰 저장소에 등록해 검증했으며 전체 TLS 검증을 끄지 않았다. 본문·토큰·query 값은 보존하지 않고 QA 통계는204로 차단했다. 앞선 root HTTP에서는 beacon script가 없었고 첫 브라우저는 인증서 신뢰 오류였다는 실패/클라이언트 차이도 보존한다.

PR53은 정확한 위 운영 주소만 기존 외부 주소에 더하고 모든 지정 file canary 검사를 먼저 수행한다. 임의 origin/path/query/port/method·잘못된 JSON은 계속 거부하며 두 통계 주소를204로 가로챈다. 독립 단위5와 파일/복구 브라우저22 통과, push CI 단위574·타입26파일0오류·preview80/production80을 확인했다. 제품 앱·자료·version·workflow는 이 보완에서 바꾸지 않았다. 새 main SHA의 실제 preview/운영80·복구/복원·Release는 후속 영수증으로만 확정한다.
