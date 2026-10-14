# 2026-10-14 보고·릴리스 계약 감사

Issue [jpoptoday#9](https://github.com/jpop-today/jpoptoday/issues/9) · REPORT-001, DAILY-001, TEAM-001, RELEASE-001 · 담당 `/root/daily_20261014_harness` · 단독 소유: 이 새 문서. 코드·Git·package·workflow·다른 문서는 수정하지 않았다.

2026-10-14 09:06 KST 최초 감사, 09:08 수정 재검증. 시작 source `main`의 `2beb1bf02ec2aa67161cd16e77ed96b18bbf580e`, docs base `5c3faaca5a77d3fd16cef865db98c68a7c204eed` / `codex/daily-development-20261013`. 공유 docs의 기존 변경은 보존했다. 직접 읽은 기준은 두 저장소와 조직의 AGENTS, [스펙](../product/spec.md), [ADR-005](../adr/005-daily-agent-management.md), [일일 계약](daily-development.md), 소스 `scripts/build-report.mjs`, `scripts/discord-notify.mjs`, `tests/report.test.mjs`, `tests/discord-notify.test.mjs`, `.github/workflows/discord.yml`, `.github/workflows/release.yml`이며, 수정 후 `tests/report-cli.test.mjs`도 읽었다.

## 직접 실행한 결과

Node `v24.19.0`에서 `node /tmp/jpop-report-audit-20261014.mjs`를 실행해 **24개 확인 PASS**, 아래 관측 2개를 별도로 남겼다. 모든 입력·출력은 합성 자료와 `/tmp/jpop-report-audit-20261014-ltbyK3`에만 두었다. 프로세스의 stdout/stderr는 권한 `0600`의 임시 파일로 받았다. 이 환경에서 `spawnSync` 기본 pipe 캡처는 `EPERM`을 반환했으므로 파일 descriptor 캡처로 다시 확인했다. `/tmp`의 다음 실행 보존을 가정하지 않는다.

| 확인 | 실제 결과 |
| --- | --- |
| KST 날짜 | UTC `2026-10-13T14:59:59Z` → `2026-10-13`, `15:00:00Z` → `2026-10-14`; 연도·윤년 전날 계산 PASS. 불가능한 날짜는 exit 1·새 출력 없음. |
| 오전 CLI | 전날 파일 없음은 exit 0·`전날 완료 보고 없음`, JSON `phase: plan`; 전날 `complete`의 완료/다음 계획만 사용. 다른 날짜·`plan`·깨진 JSON은 exit 1·새 출력 없음. |
| 완료 CLI | `completed`는 해당 KST 날짜 파일의 JSON `phase: complete`를 사용. 파일 없음·`plan`은 exit 1·새 출력 없음. JSON `plan/complete`와 CLI `morning/completed`는 다른 이름이며 CLI `plan`은 거부. |
| 평가의 형태 | 합성 3명×2개인 6개만 계산. 자기/중복 리뷰·리뷰 누락·빈 근거·0..5 밖 점수는 거부. 실제 동료 점수로 사용하지 않았다. |
| 관리자 메시지·송신 mock | 실제 CLI `message` 산출물에 합성 webhook/token/opaque secret/멘션을 넣고 `runCli`의 주입 `fetchImpl`만 호출. 본문에서 비밀값 제거·`allowed_mentions.parse: []`·POST/`wait=true`/redirect 거부 확인. mock 204는 `sent`, mock transport 실패는 비밀 없는 `TRANSPORT_ERROR`. |
| 실제 송신 CLI의 누락 분기 | 최소 env의 빈 `DISCORD_WEBHOOK_URL`로 실행해 exit 0·`skipped: webhook_not_configured`; 네트워크 요청 없음. 송신기의 손상 UTF-8 파일 거부도 확인. |

이번 담당자가 실제 Discord endpoint에 POST한 횟수는 **0**이다. mock의 `sent`는 라이브 전송 증거가 아니다. 환경 변수 전체·실제 시크릿 값·응답 본문은 출력하거나 기록하지 않았다.

## 발견과 수용 기준

**P2 AUDIT-01 · 오전 보고가 잘못된 완료기록 형태를 소비함 → 로컬 수정 해결.** 시작 SHA의 `build-report.mjs:23`은 전일 `date/phase`만 확인했다. 다음 합성 전일 JSON을 실제 `morning` CLI에 넣었을 때 exit 0이며 `unvalidated synthetic result`를 전일 완료로 보고했다. `validateCompleteReport`는 같은 입력을 거부했다.

```json
{"date":"2026-10-13","phase":"complete","timezone":"UTC","schemaVersion":99,"completed":["unvalidated synthetic result"],"nextPlan":["synthetic plan"]}
```

재현은 새 `/tmp` 디렉터리에 `reports/daily/2026-10-13.json`으로 위 JSON을 저장한 뒤, 그 디렉터리에서 `REPORT_DATE=2026-10-14 node /workspace/jpoptoday/scripts/build-report.mjs morning /tmp/audit-result.json`이다. REPORT-001의 전일 실제 완료기록 조건에 영향을 준다. root에 전달한 수용 기준은 잘못된 schemaVersion/timezone·필수 필드/배열 형태의 실패와 새 출력 없음, 정상 전일 완료·누락 fallback 유지다.

root가 `codex/11-daily-report-validation`에서 `morningReport`에 `validateCompleteReport(previous, yesterday)`를 적용하고 완료 본문의 문자열/필드/배열 검증을 강화했다. 이 담당자는 09:08 KST에 `node /tmp/jpop-report-audit-20261014-resolved.mjs`를 실행해 **39개 확인 PASS**를 직접 확인했다(`/tmp/jpop-report-audit-20261014-DQCUhQ`). 정확한 원 재현과 13개 독립 손상 입력(schema/timezone/title/summary/fields 형태·name/value/inline/25개 제한/completed·nextPlan 타입과 값)은 모두 exit 1·새 출력 없음·전일 원본 바이트 보존이었다. 정상 입력·누락·송신 mock 계약도 유지됐다. 당시 대상은 base `2beb1bf`의 root 미커밋 diff, `build-report.mjs` SHA256 `ebeafa5a122e63e2c81b15d57109758c70e48105f2fbd8a0205229c8e51bed81`이다. 코드 변경 소유자는 root이며, `0.2.1` 후보의 commit/CI/tag/배포는 이 재검증으로 완료되지 않는다.

**AUDIT-02 · 리뷰의 의미 검증은 root 게이트다.** `evaluateTeam`은 합성 리뷰의 전 항목 5점과 `수용 기준 전부 미충족, 파일 미검토` 이유·존재하지 않는 근거 문자열도 계산했다. 이 함수의 형태 검증을 실제 근거 확인으로 해석하지 않는다. [일일 계약](daily-development.md)의 관리자 규칙대로 실제 파일·검증 결과와 4항목별 근거를 읽고, 모순 점수·읽지 않은 파일·근거 없는 주장은 거부해 재검토를 요청해야 한다. 실제 6개 리뷰가 없으면 `pending_review`; 합성 점수나 이 함수만으로 교체를 완료하지 않는다. 이 감사 시점에는 실제 동료 평가를 요청받지 않았고 점수를 생성하지 않았다.

## workflow 경계와 외부 상태

직접 읽고 문자열 assertion으로 확인한 Discord workflow는 UTC00:00(KST09시) cron, 같은 저장소 `workflow_run`만 허용, **default branch checkout**이다. event의 head SHA는 보고할 식별자이며 실행 코드를 checkout하지 않는다. Node24와 `contents: read`를 사용하고 `DISCORD_WEBHOOK_URL`은 마지막 송신 step에만 주입한다. Actions 보고가 AI 개발팀 실행을 수행하는 코드는 아니다. concurrency는 동일 event run을 직렬화하며, 서로 다른 수동 run의 동일 알림까지 중복 제거하지는 않는다. 재전송 전 root가 runId·이전 성공 기록을 확인한다.

릴리스 workflow는 tag SHA checkout → main 이력 포함 확인 → tag/package 버전·노트 확인 → unit/type/build/dist/Chromium → artifact → `needs: verify` 게시 순서다. 게시 단계의 `GH_TOKEN`과 Discord secret도 해당 step에만 주입한다. `gh release create --verify-tag` 성공 후 별도 릴리스 알림을 준비하며, GITHUB_TOKEN으로 만든 release event가 새 workflow를 시작하지 않는 경우를 직접 송신 step으로 보완한다. 이는 정적 코드 감사이며 실제 게시·배포 성공 증거가 아니다.

관리형 환경 revision49/current/running에서 Cloudflare binding ready, Discord binding 없음은 직접 조회했다. **이 환경의 누락은 저장소 Actions Secret 없음이라는 뜻이 아니다.** root가 전달한 라이브 관측은 [관리자 알림 run38487575652](https://github.com/jpop-today/jpoptoday/actions/runs/38487575652)의 `2026-10-14T00:00:04.5134419Z` `Discord notification sent (1 attempt)`와 same-SHA [app38487532294](https://github.com/jpop-today/jpoptoday/actions/runs/38487532294)/[contracts38487532195](https://github.com/jpop-today/jpoptoday/actions/runs/38487532195) success, [별도 preview](https://92a25483.jpoptoday-preview.pages.dev) 배포 success다. 담당자의 직접 원격 로그/HTTP 검증과는 구분한다. root의 Secret 목록 API403은 목록 미확인이며 라이브 송신 성공과 모순되지 않는다.

최초에는 `v0.2.0` release 검증 실행 중이었고, 이후 root는 [release38487974900](https://github.com/jpop-today/jpoptoday/actions/runs/38487974900)/[deployed-preview38488015707](https://github.com/jpop-today/jpoptoday/actions/runs/38488015707) success와 `00:05:03.930Z` 릴리스 Discord `sent`를 전달했다. 이는 root의 실제 관측 인계다. `v0.2.1` 후보·jpop.today 운영 전환·AdSense 승인·cron 정시 실행은 이 문서에서 완료로 확정하지 않는다. 다음 실행은 최신 완료 JSON/대응 일일 문서, 실제 리뷰 6개와 root 확인, AUDIT-01 통합 SHA, 라이브 알림 run 및 최종 release/preview/production SHA를 읽는다. 원격 배포·복구·예약 조회는 root 통합 담당이 기록하며 이 담당자는 실행하지 않았다.
