# 차트 두 업체 탐색 · 독립 교체 감사

Issue47 / run `manual-20261020-1325` · 담당 `/root/run47_replacement` · 실제 완료 2026-10-20T07:05:52.440678+00:00. source base `4d4c021e668d49c39057e76558c21b30735b8d98`, docs base `1291c45fded5f9c22ec28b5bf78fa62d37b1d751`, 기존 `codex/issue47-chart-both` 작업 사본을 이어받았다.

초기 세 담당의 실제 여섯 읽기평가가 자료40/앱40/품질38이며, 품질의 독립 모델 중복으로 인한 유지보수 부담이 두 평가의 각1점 근거였다. root가 active roster에서 해당 담당을 제외·interrupt한 뒤 이 교체 담당을 실제 영입했다. 원래 작업/점수/소스는 보존했고 물리 삭제를 주장하지 않는다. 사용량 오류 뒤 같은 교체 ID의 턴을 재개한 것은 새로운 worker 생성이 아니다.

## 배정 범위와 독립성

[스펙 Issue47](../product/spec.md#차트의-두-업체-확인곡-탐색--issue47-2026-10-20), DATA-001/CONTENT-001/DESIGN-001/HARNESS-001. 수정은 신규 `tests/chart-both-artifact.test.mjs`, 신규 test 전용 `e2e/helpers/chart-confirmed-oracle.ts`, 기존 품질 담당이 인계한 두 테스트의 중복 모델 부분, 이 감사 문서에 한정했다. root의 `scripts/verify-dist.mjs`는 읽기·테스트 대상으로만 다뤘고 수정은 root가 수행했다. Git/package/lockfile/CI/build/dist/공유 서버/배포/추가 agent 생성은 하지 않았다.

유지보수 보완은 unit의 `numericEvidence`·보정 후 중복도와 E2E의 `evidenceMatches`/`rawProviderIds`/공개 predicate를 test 전용 한 파일로 추출한 것이다. 이 모델은 앱·계약의 count/eligibility helper를 import하지 않는다. raw/ledger의 전체 보정 후 중복도 집합과 공개 투영 predicate는 여전히 별도 함수로 교차 대조한다. 번호의 선행0, 전체 metadata/strict URL, current playlist·confirm, 전체 카탈로그의 중복도와 suppress 선적용 의미는 보존했다. 두 실행면이 같은 reference model을 쓰므로 독립성은 앱 구현과의 경계에 있으며 unit과E2E가 서로 독립된 두 reference model이라는 주장은 하지 않는다.

원래 네 unit의 고정 raw/chart/identity/CSS bytes·원92개 객체/순서/상위 metadata·새7쌍/전체 제목가수·KY null·명의보류·타5가이드 검사는 그대로다. 원래 네 E2E의 HTTP hash/원100행·업체별/union/both·원문 번호·기존URL/원행→상세·JS-off360/keyboard/44px/focus/storage bytes/가이드/robots/noads 검사도 그대로 유지했다. helper의 변경 안정성은 synthetic 선행0/엄격 URL 부정, 목록 밖 archived/chart-only 중복도와 suppress 선적용 사례를 신규 테스트에서 직접 검증한다.

## 실제 최초 실패와 root 보완

신규 source-only synthetic100행 fixture의 기대 교집합은 명시한2개 ID `001`/`1`이다. 앱 helper로 기대값을 계산하지 않았고, `00123`/`00987` 문자열과0개 교집합·다른ID의 한 업체씩·archived/chart-only/conflict/unreviewed/불명URL 제외를 대조했다. 가짜 provider 확인일은 실제자료로 사용하지 않으며 fixture의2001년 날짜·제목/가수는 합성 예다.

최초 `node --test --test-isolation=none tests/chart-both-artifact.test.mjs`는 exit1, 총68노드 중62pass/6fail이었다. 실제 누락은 아래5개 leaf이고 실패 그룹 부모1개가 집계에 포함된다. 총68은61leaf+7그룹 부모이며68개의 독립 leaf 사례로 과장하지 않는다.

| 실제 허용된 수용기준 위반 | 최소 변형 | root가 적용한 검증 |
| --- | --- | --- |
| 기본펼침 | `<details open>` | open boolean 속성 없음 |
| 중복 summary | 올바른 summary 뒤 두 번째 summary | 해당 native panel 내 summary 정확히1개 |
| 키보드 Tab 인입 제거 | summary `tabindex="-1"` | tabindex 없거나0 |
| 항상 보이는 의미 안내 누락 | panel 바깥 `#chart-both-note` 제거 | exposed HTML의 panel 밖 note 정확히1개, 같은곡/미수록 경계 |
| toggle의 안내 연결 누락 | summary `aria-describedby` 제거 | note ID 토큰 포함 |

원래 verifier SHA256은 `1f45ed3a496a892658d7464251c9fffebd4d0e98d4cc178320b7c610153447b5`, 첫 실패 테스트 SHA256은 `d45f4a3232acef57acc0a6bde1bd6aff59669fe5da686d2d38070ea27bcac148`이다. 최초 로그 `/tmp/jpop47-replacement-artifact-initial.log` SHA256 `3d3c29e4d846a51ce1c39fd9e9993ff87893ae6d69dbd57661892e081452fb75`를 덮지 않았다. root가 영속 실행 증거로 복사한다. 로그의 assertion은 모두 `Missing expected exception`으로 실제 거부 누락을 뜻한다.

root는 공유 verifier만 보완했고 교체 담당은 같은 파일의 같은68노드를 재실행해68pass/fail0/skip0(exit0)를 확인했다. 잘못된 union/분모/visible count/summary 숫자prefix, ID/순위/순서/anchor/전체제목가수·양쪽 문자열/선행0, duplicate/outside shortcut, 원100행, hidden/inert/aria-hidden/comment/template/inline CSS 위장도 기존 기대대로 대조했다. root가 이미 보완했던 summary 숫자prefix를 새 발견으로 세지 않았다. 모든HTML/CSS 공격을 포괄한다고 주장하지 않는다.

## 실제 검증 결과

| 수행 명령/검사 | 실제 결과 | 범위 |
| --- | --- | --- |
| `node --test --test-isolation=none tests/chart-both-evidence.test.mjs` | leaf4/4, exit0 | oracle 추출 뒤 원래 네 unit 수용 |
| `node_modules/.bin/tsc --noEmit --skipLibCheck --module preserve --moduleResolution bundler --target ES2023 --allowImportingTsExtensions --allowJs e2e/helpers/chart-confirmed-oracle.ts e2e/chart-both.spec.ts` | exit0, 출력없음 | 지정 helper/E2E의 타입·구문, 앱 전체 check와 별도 |
| `node --test --test-isolation=none tests/chart-both-artifact.test.mjs` 최초 | 62pass/6fail, exit1 | 실제5우회+실패 부모1 보존 |
| 같은 artifact 명령, root 보완 후 | 68pass/fail0/skip0, exit0 | 같은 테스트·합성입력·검증 대상만 보완 |
| `JPOP_TEST_BASE_URL=http://127.0.0.1:4323 PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium node_modules/.bin/playwright test e2e/chart-both.spec.ts --output=/tmp/jpop47-replacement-browser-results` | 실제4/4,19.9초, exit0; `.last-run.json` passed/failedTests[] | 원래 네 rendered assertions를 추출 뒤 재실행 |
| `git diff --check` 지정 테스트 | 통과 | 새 파일 포함 최종 상태는 root가 재확인 |

런타임은 실제 Node `v24.19.0`, 기존 Playwright·시스템 Chromium이다. Node22 로컬 실행 파일은 확인하지 못했으며22호환은 실제 contracts CI 결과로 root가 확인한다. helper는 type alias와 erasable annotation만 사용하고 enum/namespace/parameter property가 없다. source-only 회귀는 docs checkout·Git history·네트워크·build 산출물에 의존하지 않는다.

frontend-testing-debugging 및 Browser 스킬을 읽고 실제 도구 inventory를 조회했으나 Browser js/CUA/tab 제어 도구는 노출되지 않았다. 기존 지시에 명시 허용된 Playwright fallback을 사용했고 새 서버·의존성은 만들지 않았다. 검증 흐름은 `/chart/`→native 목록 열기→동결 원행→기존 상세, 그리고 `/guides/read-tj-chart/`→같은 summary anchor다. 360px JS-off/키보드/fragment reload·storage 원문/console/noads/robots를 실제 검사했다. CLI의 NO_COLOR/FORCE_COLOR 경고는 런너 설정 관측이며 앱 console 오류로 기록하지 않았다.

이 보완은 UI source를 변경하지 않았다. 기존 앱 담당의 [desktop closed](design/chart-both-20261020-1325-desktop-closed.png)와 [360px JS-off open](design/chart-both-20261020-1325-mobile-js-off-open.png) PNG를 직접 읽어 네 번째 집계/2열·native 열기/원제·가수·양쪽 번호·blue focus/mint패널을 확인했다. 새 스크린샷이나 보조기술/다른 엔진/모든 viewport 검증으로 확대하지 않는다. 이전 root의 전체57/428/type0과 이번 교체 담당의4/68은 서로 다른 실행이다.

## 원래 품질 산출물과 평가의 보존

refactor 직전 별도 backup을 만들지는 않았다. 실제 읽었던 원래 함수·호출을 후속 테스트에서 역추출해 원품질 전체파일의 기존 SHA256과 정확히 일치하는 원문을 복원했다. root가 전체hash를 다시 검증해 [원unit 인계본](audit-evidence/chart-both-20261020-1325-original-evidence-test.txt)과 [원E2E 인계본](audit-evidence/chart-both-20261020-1325-original-browser-test.txt)을 영속 복사했다. 각각 기존 `7ce6bdc86b89dbbb242f3a3ee31c1626af49c2cc4064ecc76b0da2d691a4502f` / `c4cde5a1af4a73ed23a22b2a7acfd9fa7f38eb3f18f52a7dea6357aef0a3f294`와 일치한다. [실제 보존 방법/경로](../operations/runs/manual-20261020-1325-original-quality-source-preservation.json)를 따른다. refactor 전에 backup을 만들었다거나 Git에 없던 원본 commit을 복구했다고 표현하지 않는다.

첫 [실패로그](../operations/runs/manual-20261020-1325-artifact-initial-failure.log)와 [판단 JSON](../operations/runs/manual-20261020-1325-artifact-initial-failure.json)도 root가 실제 보존했다. 원래 세 peer JSON의 여섯 평가·소스 hash는 수정하지 않았고, 위 인계 원문과 연결한다. 후속 원본이 정리된 새 테스트/hash·감사 결과의 두 읽기평가는 원래 여섯 평가와 구분한다.

root가 인계한 head contracts Node22 push39384283480/PR39384334259 success는 이 역할의 로컬 Node22 실행과 구분하며, 최종 CI·source SHA 연결은 root의 실행 기록을 따른다.

## 최종 파일과 한계

| 파일 | SHA256 |
| --- | --- |
| `e2e/helpers/chart-confirmed-oracle.ts` | `095af32bb702af8042fe7d9307861d4bf1e5390eee8e659a9991e4609501d4d3` |
| `tests/chart-both-evidence.test.mjs` | `e033b7623ca07d66b0219dc4a09ec26c12ad1e13eadb151744a3487ab9cb3ae4` |
| `e2e/chart-both.spec.ts` | `12f82a0d199af26e62f04521617ae8d42c303382d0ee897003f6a5f92c55ffc5` |
| `tests/chart-both-artifact.test.mjs` | `d45f4a3232acef57acc0a6bde1bd6aff59669fe5da686d2d38070ea27bcac148` |
| `scripts/verify-dist.mjs` | `f5cefb53e93ccabb02a5631778541a9c3f213c2708fd1d75eb898d73e2856436` |

final artifact 로그 SHA256 `24680d6304e1366d72aa00c2880debb1826293e436dbd5b2066b0c23849277d2`, unit 로그 `07d6190946a00b87c542530804a4f48dae1a5be3bc80205388f22dbc92aa7895`, browser 로그 `51b44ddcfe472392fd3142e0fba5e81cc0756ac1d230a75bbd30259de13d160a`다. 임시 실행 파일/로그는 CI의 입력이 아니고 검증 코드와 영속 판단 근거를 저장소에 남긴다.

이 역할은 full suite/앱전체 type/build/artifact, clean SHA CI·외부 preview·실제복구·production·Release·Google 계정/권리/승인 검증을 수행하지 않았다. 원본 staged 데이터·ledger99·가이드·ID/URL/storage·palette를 편집하지 않았다. 이 산출물의 후속 두 읽기평가는 다른 자료/앱 담당이 실제 최종 파일과 실행 증거를 읽은 뒤 root가 수용한다.
