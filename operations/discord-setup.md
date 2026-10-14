# Discord 보고 설정과 전송 확인

[jpoptoday#9](https://github.com/jpop-today/jpoptoday/issues/9) · [제품 스펙 REPORT-001](../product/spec.md) · [ADR-005](../adr/005-daily-agent-management.md).

이 문서는 Discord 보고의 안전한 설정 경로와 실제 송신 결과 확인 절차다. **현재 관리자 메시지는 Actions에서 실제 `sent`가 확인됐으므로 기존 연동을 유지한다.** 아래 설정 절차는 미설정 또는 웹후크 교체가 필요한 경우에 사용한다. 개발팀 실행·동료 평가·완료 기록은 [일일 운영](daily-development.md), CI·미리보기·운영·릴리스의 실제 상태는 [실행 상태](status.md)와 [일일 기록](daily/2026-10-13.md)을 따른다. 사용자가 위임한 Discord 보고는 관리자가 반복 승인 없이 수행한다. 웹후크 값 입력은 아래 보안 설정 화면에서 처리한다.

## 확인한 기준과 실행 상태

2026-10-14 00:03 UTC에 로컬 파일을 읽었다. docs 브랜치는 `codex/daily-development-20261013`, base SHA는 `5c3faaca5a77d3fd16cef865db98c68a7c204eed`다. 소스 검토 SHA는 `2beb1bf02ec2aa67161cd16e77ed96b18bbf580e`이며 아래 링크는 이 SHA의 구현에 고정한다.

- [송신기](https://github.com/jpop-today/jpoptoday/blob/2beb1bf02ec2aa67161cd16e77ed96b18bbf580e/scripts/discord-notify.mjs): URL 검증, 비밀값·멘션 보호, proxy/CA/TLS 유지, `sent`·`skipped`·`failed` 출력.
- [보고 생성기](https://github.com/jpop-today/jpoptoday/blob/2beb1bf02ec2aa67161cd16e77ed96b18bbf580e/scripts/build-report.mjs): `morning`·`completed`·`message`·이벤트 보고 준비. 준비 성공은 전송 성공이 아니다.
- [Discord workflow](https://github.com/jpop-today/jpoptoday/blob/2beb1bf02ec2aa67161cd16e77ed96b18bbf580e/.github/workflows/discord.yml), [릴리스 workflow](https://github.com/jpop-today/jpoptoday/blob/2beb1bf02ec2aa67161cd16e77ed96b18bbf580e/.github/workflows/release.yml), [README](https://github.com/jpop-today/jpoptoday/blob/2beb1bf02ec2aa67161cd16e77ed96b18bbf580e/README.md).

관리자가 전달한 실제 관측은 다음과 같다. 문서 담당은 해당 원격 실행을 직접 재조회하거나 송신하지 않았으며, 관리자 인계 증거와 로컬 계약을 대조했다.

| 관측 | 실제 결과와 해석 |
| --- | --- |
| Actions 관리자 메시지 [run 38487575652](https://github.com/jpop-today/jpoptoday/actions/runs/38487575652) | `2026-10-14T00:00:04.2355691Z`에 `Report prepared: message (2026-10-14); delivery not yet confirmed.`, `2026-10-14T00:00:04.5134419Z`에 `Discord notification sent (1 attempt).`가 출력됐다. 이 Actions 실행에 필요한 웹후크가 전달되어 HTTP 성공이 확인됐다. 채널 확인 여부는 별도로 기록한다. |
| 관리자 클라우드 런타임의 송신 CLI | `webhook_not_configured`로 `skipped`. 이 로컬 런타임의 binding이 missing인 결과이며 Actions 설정 상태를 뜻하지 않는다. |
| Actions Secret 목록 API | root의 이전 관측인 2026-10-13 약 06:31 UTC에 integration 권한 HTTP 403. 목록을 확인하지 못했다. |

**Secret 목록 403으로 저장소 Secret의 존재·부재를 판단하지 않는다.** 현재 전송 성공은 목록 조회가 아니라 위 actual job 로그를 근거로 한다. 누가 언제 Secret을 설정했는지는 추정하지 않는다. 이 관리자 메시지의 `sent`를 다른 CI 보고·cron 보고·완료 보고·릴리스의 발송 성공으로 확대하지 않으며, 새 CI·미리보기·릴리스 완료도 추정하지 않는다.

## 저장소 Actions Secret 설정

1. 기존 연동이 정상이면 보존한다. 미설정 또는 교체가 필요할 때 보고를 받을 Discord 채널에서 **채널 편집 → 연동 → 웹후크**를 열고 해당 채널에 연결된 웹후크를 만들거나 선택한다. 웹후크 관리 권한이 있는 운영자가 처리한다. 웹후크 URL을 복사한다.
2. [jpop-today/jpoptoday Actions Secret 입력](https://github.com/jpop-today/jpoptoday/settings/secrets/actions/new)을 연다. 조직 docs 저장소가 아니라 실제 송신 workflow가 있는 `jpop-today/jpoptoday` 저장소다.
3. **Name**에 `DISCORD_WEBHOOK_URL`, **Secret**에 복사한 채널 웹후크 URL을 넣고 **Add secret**을 누른다. 같은 이름이 이미 있다면 저장소 Settings → Secrets and variables → Actions에서 갱신한다.
4. 값은 채팅·Issue·PR·소스·보고 JSON·문서·명령 인자·스크린샷에 붙이지 않는다. 이 문서에는 웹후크 값이나 token 예제를 두지 않는다.

송신기가 허용하는 목적지는 HTTPS의 `discord.com` 또는 `discordapp.com` 웹후크 경로다. 복사한 원본 URL의 query·fragment·사용자 정보·명시적 port를 추가하지 않는다. 송신기가 요청할 때 `wait=true`를 붙인다.

현재 workflow는 `${{ secrets.DISCORD_WEBHOOK_URL }}`을 **송신 step의 환경 변수**로만 넘긴다. 단순 Actions Variable, docs 저장소의 Secret, 로컬 셸에 넣은 값은 이 Secret을 대신하지 않는다. 설정 저장 성공만으로 `sent`라고 기록하지 않는다.

### 권한 403일 때

Actions Secret API의 403은 해당 integration의 요청이 권한으로 거절됐다는 실제 호출 결과다. 목록 조회 403만으로 설정 권한이나 Secret 존재 여부까지 확정하지 않는다. 설정이 필요한 경우 권한이 있는 저장소 운영자가 위 GitHub 설정 화면에서 직접 등록·갱신하고 아래 `message` 테스트를 수행한다. 설정 화면 자체에 접근할 수 없다면 저장소 관리자에게 해당 설정 작업을 요청한다. URL이나 token을 채팅으로 전달받아 우회하지 않는다.

수동 workflow 실행 또는 로그 조회도 403으로 막히면 그 호출·시각·권한 차단을 기록하고 권한이 있는 관리자가 Actions 화면에서 실행·송신 로그를 확인한다. Discord 송신 로그의 `HTTP 403`은 별개의 목적지 HTTP 실패이며 `failed`로 기록한다. GitHub 목록 API 403과 혼동하지 않는다.

## 환경 binding 대안

관리자가 승인된 관리형 환경 또는 secret manager에서 `DISCORD_WEBHOOK_URL`을 런타임 환경 변수로 안전하게 주입할 수 있다. 설정 화면에서 secret을 binding하고 값·전체 환경·credential 파일을 출력하지 않는다. managed 환경의 준비 관측과 실제 송신 결과를 각각 확인한다. 이 binding은 해당 런타임의 CLI용이며 저장소 Actions Secret으로 자동 복사되지 않는다.

GitHub Environment Secret을 사용할 수도 있지만, 현재 `discord.yml`의 `notify` job과 `release.yml`의 `publish` job에는 `environment` 지정이 없다. 따라서 Environment에 값만 넣으면 현재 workflow가 사용할 수 없다. 이 대안을 채택하면 관리자가 두 송신 job의 Environment 연결·접근 규칙을 별도 변경·검증해야 한다. 현재 구현에 바로 적용하는 기본 경로는 저장소 Actions Secret이다. 이 문서는 workflow나 환경 설정을 변경하지 않는다.

실제 송신은 **Node.js 24.5 이상**을 사용한다. 기존 `HTTPS_PROXY`·`https_proxy`·`HTTP_PROXY`·`http_proxy`와 `NO_PROXY`·`no_proxy`, CA trust 설정을 유지한다. 송신기는 HTTPS proxy를 우선하고 HTTP proxy를 fallback으로 사용하며 Node 표준 HTTPS agent의 CONNECT 지원을 이용한다. proxy를 지우거나 `NODE_TLS_REJECT_UNAUTHORIZED=0`·`rejectUnauthorized:false`로 TLS 검증을 끄지 않는다. 기존 추가 CA trust도 제거하지 않는다.

Secret이 준비되지 않아도 개발·문서·계약 검증·CI·미리보기 작업을 계속한다. 해당 환경의 송신기는 명시적으로 `skipped`를 남긴다. 개발 진행 상태와 Discord 설정 차단을 따로 기록한다.

## 어떤 보고가 언제 준비되는가

| 경로 | 현재 구현 계약 | 확인할 실행 |
| --- | --- | --- |
| 오전 | `discord-report`의 cron `0 0 * * *`: UTC 00:00, Asia/Seoul 09:00. `morning` 모드는 전날 `reports/daily/YYYY-MM-DD.json`의 저장된 완료 항목·다음 계획을 읽는다. 전날 파일이 없으면 기록 부재를 알린다. | Actions `discord-report`의 `notify` job. GitHub 예약은 지연될 수 있으므로 실제 실행 시각을 남긴다. AI 개발 예약과 별개다. |
| CI 완료 | `app`·`contracts`·`deployed-preview` workflow의 `completed` 이벤트에서 conclusion·브랜치·소스 SHA·run URL을 보고한다. 성공 외 conclusion도 실제 값대로 보고한다. 동일 저장소의 run만 송신 대상으로 받으며 송신기는 검토된 default branch에서 checkout한다. | 원본 CI run과 후속 `discord-report` run을 각각 확인한다. CI 성공을 운영 배포나 AdSense 승인으로 집계하지 않는다. |
| 저장된 완료 보고 | 관리자가 `workflow_dispatch`에서 `mode=completed`를 선택한다. 대상 KST 날짜의 파일이 있고 `schemaVersion:1`, 동일 date, `timezone:Asia/Seoul`, `phase:complete` 등 검사에 통과해야 한다. | 보고 준비 실패이면 송신 step까지 도달하지 않는다. CLI mode는 `completed`, JSON phase는 `complete`다. |
| 관리자 메시지 | 관리자가 `workflow_dispatch`에서 `mode=message`와 비밀 없는 title·message를 입력한다. 저장된 완료 파일과 무관하게 관리자 메시지를 준비한다. | 설정 테스트·추가 차단·완료 알림에 사용할 수 있다. 완료 개선·검증 결과는 실제 일일 기록을 근거로 작성한다. |
| 릴리스 | 외부 게시 이벤트의 `release:published`는 `discord-report`로 보고한다. `version-release`는 검증 후 GitHub Release를 만든 뒤 자체 `publish` job에서 직접 송신한다. `GITHUB_TOKEN`으로 만든 Release 이벤트가 다른 workflow를 시작하지 않는 점을 보완한다. | 직접 게시 경로는 `version-release`의 `Deliver release notice or skip missing secret` step을 확인한다. 릴리스 게시 성공과 알림 성공을 분리한다. |

보고 생성기의 `Report prepared: ...; delivery not yet confirmed.`는 JSON 준비 로그다. `completed` 준비 실패나 상위 job 차단으로 송신이 실행되지 않으면 `not_attempted`로 남기고 준비·차단 이유를 적는다. 새 CI·preview·release 완료 여부는 이 계약 설명으로 추정하지 않는다.

## 설정 후 관리자 `message` 테스트

아래 명령은 **관리자가 새 설정·교체 뒤 실행할 실제 송신 테스트 절차**다. 문서 담당은 실행하지 않았다. 현재 확인된 위 Actions `sent` 이력은 보존하며 설정 변경이 없다면 같은 테스트를 반복할 필요가 없다. 사용자 승인 범위의 보고이므로 반복 승인 요청은 필요 없다. 중복 테스트를 피하려면 기존 성공 이력을 먼저 확인하고 title·message에 고유한 비밀 없는 확인 표식을 넣는다.

```sh
gh workflow run discord.yml \
  --repo jpop-today/jpoptoday \
  --ref main \
  -f mode=message \
  -f title='Discord 설정 확인' \
  -f message='관리자 설정 확인 2026-10-14-test-01. 이 메시지는 웹후크 연결 테스트입니다.'

gh run list \
  --repo jpop-today/jpoptoday \
  --workflow discord.yml \
  --event workflow_dispatch \
  --limit 5 \
  --json databaseId,createdAt,status,conclusion,headSha,url

gh run view RUN_ID --repo jpop-today/jpoptoday --log
```

`RUN_ID`는 방금 요청한 dispatch의 시각·이벤트·main 기준으로 확인한 실제 ID로 바꾼다. 다른 실행의 최신 결과를 테스트 결과로 복사하지 않는다. [Actions](https://github.com/jpop-today/jpoptoday/actions/workflows/discord.yml) 화면에서 **Run workflow → main → mode: message**로 같은 테스트를 실행할 수도 있다. 웹후크 값은 입력 항목에 넣지 않는다.

1. 해당 run의 `Build factual report`가 성공했는지 확인한다.
2. `Deliver or explicitly skip missing secret` step의 아래 실제 출력을 확인한다. workflow 전체의 녹색 성공만으로 `sent`를 판단하지 않는다.
3. 지정한 Discord 채널에 동일 title·확인 표식이 도착했는지 확인한다. 실제 시각·run URL/ID·검토한 소스 SHA·송신 상태·비밀 없는 이유를 일일 기록에 남긴다. 송신 로그가 `sent`인데 채널 확인이 되지 않으면 두 관측을 별도로 남긴다.

## 송신 출력과 실패 처리

| 실제 송신 CLI 출력 | 종료 코드 | 기록 의미 |
| --- | --- | --- |
| `Discord notification sent (1 attempt).` 등 | `0` | 목적지가 HTTP 200 또는 204를 반환했다. 로그의 실제 attempt 수를 기록한다. 채널 확인은 별도로 수행한다. |
| `Discord notification skipped: webhook_not_configured.` | `0` | 그 실행에 전달된 `DISCORD_WEBHOOK_URL`이 undefined 또는 빈 문자열이었다. 요청하지 않았다. 저장소 전체의 설정 여부를 추정하지 않는다. |
| `Discord notification failed: HTTP_ERROR; HTTP 403.` 등 | `1` | 송신기 검증·transport·timeout·목적지 HTTP 실패. 실제 안전한 code와 HTTP status만 기록한다. |
| 송신 step 미실행 | 해당 없음 | `not_attempted`. 보고 생성 실패·상위 job 차단 등 실제 원인을 기록한다. |

`failed` 출력은 목적지·응답 body·보고 내용·내부 오류를 노출하지 않는다. 디버깅을 위해 URL·token·전체 환경을 출력하지 않는다. Secret에 공백만 있거나 잘못된 URL이 있으면 missing skip이 아니라 검증 실패다. 429만 유효한 retry 정보를 기준으로 최대 3회, 회당 최대 10초 대기하여 재시도한다. timeout·5xx 등은 중복 방지를 위해 자동 재송신하지 않는다. 관리자는 재시도 전에 job·채널·기존 성공 기록을 확인한다.

릴리스 `publish` job은 GitHub Release를 만든 뒤 송신한다. 이후 송신 실패로 job이 실패해도 Release가 이미 게시되었을 수 있다. 릴리스 URL·게시 상태를 먼저 확인하고, 알림 재시도 때문에 릴리스 게시 job 전체를 무작정 다시 실행하지 않는다. 필요한 알림은 실제 게시 근거를 담은 관리자 `message`로 별도 처리한다.

## 문서 검증과 인계

이 변경은 `operations/discord-setup.md`만 소유한다. Git·package·workflow·소스·보고 공용 파일은 수정하지 않았다. 검증은 상대 문서 링크의 파일 존재, 고정 SHA 소스 링크의 로컬 git 객체, workflow inputs·송신 step·CLI 출력·종료 코드·보고 phase 계약과의 대조다. 네트워크 호출·실제 송신·새 CI·미리보기·릴리스 확인은 관리자의 후속 실행으로 남긴다.

2026-10-14 00:05:55 UTC 로컬 검증: `python3` inline 링크·계약 검사에서 상대 링크 5개, 고정 SHA 소스 링크 5개, 소스 계약 16개와 웹후크 URL 예제 부재가 PASS였다. `git diff --check`는 기존 tracked diff에서 오류가 없었으며, 이어 수행한 새 문서의 Python 공백·최종 newline 검사도 PASS였다. 계약 확인 대상 소스 SHA는 위 `2beb1bf0…`다. 문서 작성·로컬 대조 PASS와 관리자가 전달한 Actions `sent` 관측을 분리하여 인계한다.
