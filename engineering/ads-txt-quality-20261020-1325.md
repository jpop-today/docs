# Issue47 r2 · 원본 ads.txt 복원 품질 검증

같은 run `manual-20261020-1325`의 운영 후속이며 신규 worker가 아니다. 담당 `/root/run47_replacement`, source base `7ced2f6b2d072db75c82d48d2196577bb04b3342`, branch `codex/issue47-ads-txt`. [추가 스펙](../product/spec.md#실제-운영-후속--원본-adstxt-복원), HARNESS-001/RELEASE-001/ADS-001을 따른다. 최초 Issue47의 68감사·6+2평가·M1의 실제 CI/배포/복구와 이번 r2 검증은 구분하며 기존 증거를 덮지 않았다.

수정 소유는 새 `tests/ads-txt-artifact.test.mjs`, 새 `e2e/ads-txt.spec.ts`, 이 문서와 root가 후속 추가 배정한 읽기평가 `ads-txt-peer-quality-20261020-1325.json`이다. root가 `public/ads.txt`·finalize/verify-dist/verify-deployment와 Git/package/lockfile/CI/build/단일서버/배포를 소유한다. 이 담당은 해당 파일이나 제품 UI/data를 수정하지 않았고 추가 agent를 만들지 않았다.

## 원본과 목표

root의 [실제404 재확인](../operations/runs/manual-20261020-1325-ads-txt-recheck.json)과 [원본 출처](../operations/runs/manual-20261020-1325-original-ads-provenance.json)를 구분한다. 원본 공개 commit `744b90ba1c99b60134dcc57e347b408f61557cd0`, blob `aeee54215b01df4256bd69a3eaec9c95226d0850`의59bytes를 root가 복원했다. 본 역할은 public 파일을 메모리에서 읽어59bytes/SHA256 `485311a4f1407193caa0a8a7314c316e2f37aaae73986b7c7f935033fb44db80`/Google seller 형식1행을 실제 검사했다. 게시자 값이나 원문은 이 문서·assertion·로그에 복사하지 않았다. 새 publisher 생성이나 Google 계정 일치·소유권·권리·심사 승인으로 해석하지 않는다.

production artifact와 실제 `/ads.txt`는 원본의 정확한 bytes와 text/plain을 제공해야 한다. preview는 파일을 포함하지 않고404/noindex이며 seller행이나 광고코드를 제공하지 않아야 한다. 기존 metadata/동결자료/ID/ledger/URL/storage·palette와 기존27 HTTP는 별도 기존 회귀로 유지한다. 과거 해당 파일이 없던 artifact의 복구에 새 파일 요구를 소급하지 않는다.

## source-only artifact 검증

root의 실제 `verifyAdsTxt({target, bytes, expectedBytes})` export를 읽고 테스트했다. 대상은 공개 artifact 계약이고 제공사·Google 계정 API나 실제 광고 요청이 아니다.

- production 원본59bytes/hash/형식1행·무광고/HTML아님, 함수 입력과 기대 Buffer 무변경.
- production 누락·null·빈파일·잘림·1byte변조·추가공백/개행·중복행·HTML wrapping을 거부.
- 기대 reference 자체가 누락·null·빈값·비seller·HTML인 경우도 거부.
- preview는 `undefined` 파일부재만 수용하며 null·빈파일·올바른 원본이나 HTML 파일을 거부.

첫 테스트는 preview null도 부재로 허용할 것이라 가정해19노드 중18pass/1fail(exit1)이었다. root 함수와 실제 fs의 부재 계약은 undefined만 허용하므로 구현 결함이 아니라 테스트 가정 오류다. root를 느슨하게 바꾸지 않고 테스트에서 null 거부로 바로잡았다. `/tmp/jpop47-r2-replacement-ads-artifact.log` SHA256 `edad5cebf65d08110c13ed436ebf19405c4bfe62ae23c5ca4834b8bfe649097f`를 보존했다.

수정 후 같은 범위 실행은23노드/23pass/fail0/skip0(exit0)이다. 20leaf+3그룹 부모이며23개의 독립 leaf라고 과장하지 않는다. `/tmp/jpop47-r2-replacement-ads-artifact-final.log` SHA256 `9a2f9e5c969b65aab8b1ee365509cf372b99b45410d23a4af2276716538c4ded`다. 2026-10-20T07:38:40.754594+00:00 실제 검사에서 두 로그에 원문 seller body와 실제 `pub-` 식별자가 모두 없음을 boolean으로 확인했다.

| 명령 | 실제 결과 |
| --- | --- |
| `node --test --test-isolation=none tests/ads-txt-artifact.test.mjs` 최초 | 18pass/1fail, 테스트 null 가정 오류 보존 |
| 같은 명령 수정 후 | 23pass/fail0/skip0, exit0 |
| `node_modules/.bin/tsc --noEmit --skipLibCheck --module preserve --moduleResolution bundler --target ES2023 e2e/ads-txt.spec.ts` | exit0, 출력없음 |
| `git diff --check` | 통과 |

## 실제 HTTP E2E와 경계

새 단일 E2E는 `/build-info.json`의 실제 target을 읽고 `/ads.txt`를 redirect 없이 요청한다. production은200·text/plain·59bytes·원본SHA·seller1행·HTML/광고코드없음을 요구한다. preview는404/noindex·seller0행·원본파일 hash와다름을 요구한다. response body나 publisher 문자열은 assertion의 actual/expected로 넘기지 않고 상태/boolean/길이/hash/행수만 비교한다. 실제 광고를 표시하거나 클릭하지 않는다.

root의 새 r2 preview build/verify·기존 단일4323서버 제공 통지 뒤 실제 preview E2E를 수행했다. root가 통지한 작업 artifact는 source M1 `7ced2f6b…`/dirty:true/preview3347파일·3336HTML·index0·ads파일없음이며 clean 확정SHA 배포와 구분한다. 신규 `e2e/ads-txt.spec.ts`와 기존 `e2e/chart-both.spec.ts`를 함께 실행해 실제5/5 pass·20.6초·exit0, `.last-run.json` passed/failedTests[]를 확인했다.

preview 명령: `JPOP_TEST_BASE_URL=http://127.0.0.1:4323 PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium node_modules/.bin/playwright test e2e/ads-txt.spec.ts e2e/chart-both.spec.ts --output=/tmp/jpop47-r2-preview-browser-results`. 실제 파일404/noindex/seller0/광고없음과 기존4개 차트·원100행/업체별·교집합·문자열번호/URL/JS-off360/keyboard/storage/가이드동선을 통과했다. 로그 `/tmp/jpop47-r2-preview-browser.log`에 실제 publisher 식별자가 없음을 boolean으로 확인했다. root가 서버를 production artifact로 변경하기 전 완료를 통지했고 추가 서버/build는 실행하지 않았다.

root의 production build/verify 통지 뒤 같은 단일서버에서 같은 신규ads1+기존chart4를 `/tmp/jpop47-r2-production-browser-results`에 실행해 실제5/5 pass·19.5초·exit0, `.last-run.json` passed/failedTests[]를 확인했다. 실제 `/build-info.json`과 `/ads.txt`를 별도 읽어 source M1 `7ced2f6b…`/dirty:true/targetproduction 및200·text/plain; charset=utf-8·59bytes·원본SHA·Google1행도 확인했다. 별도 읽기 시각은 `2026-10-20T07:45:04.290Z`, 값미포함 증거는 `/tmp/jpop47-r2-production-route-proof.json`이다. root가 통지한 production artifact는3348파일/3336HTML/57index다. 로컬 target 검증이며 외부운영 완료나 clean 확정SHA 배포로 확대하지 않는다.

새ads E2E만 `trace:'off'`로 지정해 실패trace에도seller본문을 남기지 않도록 했다. preview5를 수행한 초기 E2E hash76c53bc6…와 production5를 수행한 최종hashb472430d…의 차이는 이 trace 설정뿐이며 HTTP assertions는 같다. 기존chart4의 trace/검사·코드는 바꾸지 않았다. 최종코드의 두target 전체58 및 같은SHA CI/외부검증은 root가 수행한다. 두 E2E 로그에 실제 publisher 식별자와 원본문이 없음을 boolean으로 확인했다.

root의 첫 production artifact 검사에서는 기존 walk allowlist가 새루트ads.txt를 허용하지 않아 exit1이 발생했다. root가 [원실패로그](../operations/runs/manual-20261020-1325-r2-initial-artifact-failure.log)를 보존하고 루트 `ads.txt` 한경로만 허용한 뒤 기존 strict 원본bytes/prod-only 검증을 통과했다. 이 실제 통합 누락과 앞선 테스트 null가정 오류는 서로 다른 실패다. 임의txt/하위ads파일을 허용하는 확장은 아니며, 신규unit은 verifyAdsTxt 함수 대상이고 전체파일 allowlist 통합은 root의 실제artifact 실행과 코드읽기로 확인했다.

현재 원본59bytes로 Git blob SHA1을 별도로 계산해 `aeee54215b01df4256bd69a3eaec9c95226d0850`도 일치함을 확인했다. 이는 원문을 출력하지 않는 읽기 검사이며 원본 API를 새로 조회했다는 주장이 아니다. 자료 담당의 [복원 근거](../data/ads-txt-restoration-20261020-1325.md) 최종본도 읽었다.

이 역할의 Node 런타임은24이며 전체 unit/type/build/artifact·같은 SHA CI·실제외부preview/production·복구·Release 및 Google계정/심사는 root의 별도 게이트다. 초기404의 발생 원인·시각·Google봇 접근성·계정 일치는 추정하지 않는다. 원래6평가와후속2평가는 원기록 그대로이며 이번 r2의 새읽기평가는 별도로 기록한다.

## 현재 테스트 파일

| 파일 | SHA256 |
| --- | --- |
| `tests/ads-txt-artifact.test.mjs` | `49c9893e93fafbaf59031912108615f7753c3a3dc461850406487c45ff721e07` |
| `e2e/ads-txt.spec.ts` | `b472430d614f7d38254b1a769fe28b1cbaafa9e6493bda508dce357ef76c707a` |


최종 root verifier 읽기 SHA256 `558e68ac9a836e6e5c4c3710267f891dcba0e7e1a5a79bbd58bdd52d79b0d2c9`. [자료 산출물 후속 읽기평가](ads-txt-peer-quality-20261020-1325.json)는 실제1건20/20이며 원6+후속2평가와 별도다. source/tests2개 및 이 품질문서를 동결하고 root가 최종같은SHA 검증·통합한다.
