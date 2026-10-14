# Cloudflare 배포·복구

RELEASE-001. 운영 todayjpop의 npm run build/dist/source dev 설정을 Pages API로 확인했다([기준선](cloudflare-baseline.md)). 개인 원본 README의 Node24 요구와 실제 runtime 설정 확인은 구분한다.

## 연결 조사

프로젝트명·계정·현재 연결 repo/production branch·build command/output·custom domain·현재 운영 deployment ID/SHA·직전 정상 deployment·예약 수집 branch를 실제 Cloudflare/GitHub에서 확인해 기록한다. 비밀 값은 기록하지 않는다. 신규 org Git 연결이 자동 운영 배포를 유발하는지 확인하고 먼저 preview branch를 사용한다.

수정·미리보기·배포는 사용자가 위임했다. 정상 게이트를 통과한 릴리스에 반복 확인을 요구하지 않는다. 계정 본인 정보·약관 동의·사용자 데이터 삭제 같은 입력은 사용자에게 맡긴다.

## 게이트

1. 관련 스펙·ADR·변경 데이터·권리/호환성 검토, 소스 SHA 고정.
2. lockfile 설치, 테스트·typecheck·고정 스냅샷 빌드. org 앱은 npm ci/test/check/build/verify:dist/test:e2e로 검증한다. 실제 command/SHA 결과를 남긴다.
3. 실제 dist에서 원본 TSV/JSON/가사·scripts·tests·환경 파일 비공개 검사. 설정만 믿지 않는다.
4. CI와 같은 SHA/artifact로 preview 배포. 검색·번호 복사·즐겨찾기·기존 URL·404·본문·sitemap·광고/동의를 브라우저·HTTP로 확인.
5. 이전 정상 deployment로 복구하는 방법과 데이터 snapshot을 확보.
6. 위임 범위에서 production 배포, 실제 jpop.today에서 핵심 스모크·artifact SHA 연결 확인.
7. 릴리스 기록에 source SHA, catalog hash, content version, CI run, preview/production deployment ID, 검사 결과, rollback target 기록.

/songs 정적 상세뿐 아니라 기존 /song?id=, /filter?q=유이카, /chart, /coverage, /favorites, /guides/*, .html 리다이렉트와 /#favorites-view를 확인한다. /data/jpop-playlist.raw.json·/scripts/*·존재하지 않는 경로는 실제 404여야 한다. 운영 HTTPS origin을 유지해야 기존 localStorage를 읽을 수 있다.

## 실패와 복구

수집 오류는 신규 데이터 승격과 배포를 중단하고 마지막 정상 snapshot을 유지한다. UI/URL 회귀는 이전 정상 artifact로 복구한다. 데이터와 코드 버전을 함께 고정하고 smoke를 다시 실행한다. 권리 문제 자료를 내릴 때는 새 artifact에서 제외 후 운영 반영·캐시 상태를 확인한다. 이전 이력을 지우는 강제 push나 운영 프로젝트 삭제는 하지 않는다.

## 초기 조사 제한과 현재 기준

초기 운영 GET·GitHub API는 CONNECT403이었으나 후속 HTTP·PR·CI 확인은 성공했다. GitHub 배포 목록 API는 integration scope403이다. 사용자가 CF binding을 입력한 뒤 특정 Pages 프로젝트/domain/deployment 읽기는 성공했다. 당시 새 앱 artifact/preview/rollback/운영 전환은 미실행이었다. 이후실제preview·같은SHA CI·원격브라우저는통과했으며, 최신운영전환/복구상태는status와해당version릴리스에서다시확인한다. 인증·프로젝트 연결을 확인할 때까지 배포 완료를 주장하지 않는다. 네트워크 설정 반영만으로 인증·push·배포 권한이 생겼다고 가정하지 않는다.

## 조직 앱 미리보기 실행 경로

전용 `jpoptoday-preview`는 direct upload이며 모든 page가 noindex·광고 없음이다. 소스 sourceSha·catalog hash·clean 상태를 확인한 dist만 npm run deploy:preview로 배포한다. existing todayjpop custom domain/source는 이 명령에서 변경하지 않는다. Cloudflare의 environment=production은 별도 미리보기 프로젝트 내부 branchmain의 label이다. 이를 jpop.today 운영 전환으로 쓰지 않는다.

실제 build/type/contract/browser/HTTP·CI·배포 ID는 [Issue#5 실행 기록](status.md)의 최신 항목을 따른다. 원격 browser는 deployed-preview workflow로 정상 HTTPS에서 source를 고정해 검증한다. 복구 시험은 전용 프로젝트에서 성공한 실제 이전 artifact로 수행하고, 기존 jpop.today rollback 권한/실행을 완료로 확대하지 않는다.

## ADR-006 기존 origin의 조직 artifact 전환

기존 todayjpop Git 프로젝트를 삭제하거나 직접-upload 타입으로 바꾸지 않는다. Cloudflare 공식 [Git integration](https://developers.cloudflare.com/pages/configuration/git-integration/)은 자동 production/preview 빌드를 중지하고 Wrangler artifact를 기존 Git 프로젝트에 배포하는 경로를 지원한다(2026-10-14 본문 확인). jpop.today/HTTPS origin·domain·dev production branch는 유지한다.

1. org main의 정확한 SHA에서 두 target을 고정자료로 빌드/검증한다. preview와production metadata의catalog/ledger/chart hash가같아야한다. mainCI와실제noindexpreview/원격브라우저가통과해야한다.
2. `python3 scripts/manage-production.py inspect`로현재project/domain/branch/source·성공canonical을재확인한다. 이전정상artifact와설정의복구json을확보한다.
3. `python3 scripts/manage-production.py pause <새backup.json> <현재원본SHA>`는backup을exclusive로생성하고automatic production=false/preview=none으로가역변경한다. 현재배포가바뀌지않은것을확인한다.
4. Actions `production-deploy`를검증SHA/전용preview URL로dispatch한다. main이력·sameSHA appCI·동일reviewedhash·정적/브라우저게이트를거쳐ActualActions CFbinding이있을때만배포한다. bindings없음은explicit skip이며job success를실배포로쓰지않는다. cloudbinding은Actionssecret으로복사하지않는다.
5. Actionsbinding이없는경우cloud관리자가cleanHEAD의production dist를`npm run deploy:production`으로기존todayjpop/dev에직접올린다. actualjpop.todayHTTP/metadata/sourcehash/crawler/권리경계와브라우저를검증한다. workflow `verify_only=true`는배포없이실제origin을같은SHA로검증한다.
6. `python3 scripts/manage-production.py rollback <성공productionID>`는target성공과production환경을읽고APIrollback/canonical일치를확인한다. root는원본복구·새버전복원후HTTP도확인하고증거를남긴다. 필요할때`restore <backup.json>`으로이전Git자동배포설정도복구할수있다. 수집실패는정상snapshot으로돌아가며원본이력을지우지않는다.

[실제 v0.3.0 기록](releases/v0.3.0-20261014.md)의 sourceSha/deployment ID/CI·HTTP·remote·rollback이실행증거다. `_headers`·robots·sitemap은target별로검증하고모든preview는noindex/광고없음이다. production이정상배포되더라도Google계정/법적동의·실제심사통지는별도운영자단계다.

## Issue#15 응답 검증과 Actions 바인딩 인계

[ADR-007](../adr/007-deployment-response-verification.md)에 따라 다음 helper를 같은 target의 **검증된 고정 dist**와 실행한다. `EXPECTED_SOURCE_SHA`는 전체 40자리 SHA, `EXPECTED_TARGET`은 `preview` 또는 `production`이다. 인자는 안전한 HTTPS origin·dist 경로·새 결과 파일이며, 기존 정상 결과를 덮어쓰지 않는다.

```sh
npm run verify:deployment -- https://jpop.today dist /tmp/jpop-production-healthcheck-new.json
```

nonce는 진단 metadata에만 붙이고 실제 홈·카탈로그·crawler·대표 상세는 일반 URL에서 검사한다. 실패 시도·최종 상태·시간/크기 상한을 기록하며, 성공한 API rollback 후에도 실제 HTTP를 다시 확인한다. 부분 HTTP 결과를 브라우저·콘텐츠/권리 심사·모든 CDN 위치의 동시 전환 증거로 확대하지 않는다. [하네스 기준](../engineering/deployment-healthcheck-20261014.md)과 [실제 실행 인계](runs/resume-20261014-2000.json)가 상세 결과다.

현재 workflow는 저장소 Actions secrets를 읽는다. 안전한 [GitHub 설정 페이지](https://github.com/jpop-today/jpoptoday/settings/secrets/actions)에서 `CLOUDFLARE_API_TOKEN`과 `CLOUDFLARE_ACCOUNT_ID`를 구성한다. 토큰은 기존 todayjpop이 있는 Cloudflare 계정의 Pages 배포에 필요한 권한으로 발급하고, 값·계정 ID는 채팅·Git·로그에 넣지 않는다. 관리형 클라우드의 readiness/프록시 placeholder를 실제 토큰으로 복사하지 않는다. Environment secrets를 사용하려면 해당 job에 실제 environment를 연결하는 후속 변경과 권한 검증이 필요하며, 설정만 존재한다고 현재 job이 읽는다고 가정하지 않는다.

실제 `production-deploy`의 presence-check 결과와 deploy step을 확인한다. 두 값이 준비되지 않으면 `skipped`이며 전체 job의 success를 운영 배포로 기록하지 않는다. 이 설정이 없어도 관리자는 클라우드 바인딩으로 검증된 artifact를 배포하고 `verify_only=true`로 실제 운영 HTTP·브라우저를 같은 SHA에서 검증할 수 있다. Discord는 기존 notification workflow의 `DISCORD_WEBHOOK_URL` 경로를 유지한다.
