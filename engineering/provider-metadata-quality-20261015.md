# 제공사 메타데이터 추출·표기 비교 안내 검증

실행: `daily-20261015-0900` · 담당 `/root/daily15_quality` · [Issue20 작업 계약](https://github.com/jpop-today/jpoptoday/issues/20#issuecomment-6181213077). DATA-001, HARNESS-001, CONTENT-001, SEO-001, DESIGN-001 및 ADR-006/007을 따른다.

소스 기준 `7ed3f7eb3203ed4bb0de0c247f07a129bba8dba0`, 문서 기준 `4525efae1f6779ac7faac2ea7dfb834ffc0b675e`, 조직 규칙 기준 `b7aad18fe2f793453b6505bb150f359d05ae3554`. 소스 작업 브랜치는 `codex/20-safe-provider-review-20261015`이다. 검증 시점의 코드는 기준 SHA에 이번 작업의 미커밋 변경을 더한 상태이며, 이를 기준 SHA의 확정 산출물이나 원격 CI로 쓰지 않는다.

## 소유 범위와 완료 기준

이 담당은 소스 `tests/provider-metadata.test.mjs`, `e2e/title-artist-guide.spec.ts`와 이 문서만 편집한다. Python 추출 구현은 자료 담당, 가이드 본문은 앱 담당, 공용 계약·package/lockfile·CI·Git·빌드·서버·배포·최종 증거는 root 소유다. 다른 담당 파일을 수정하거나 새 에이전트·사용자 채팅을 생성하지 않았다.

추출기는 공식 숫자 조회의 제목·가수·번호만 다루며, 잘못된 응답·쓰기 실패가 기존 후보·reviewed ledger·staging·마지막 정상 자료를 덮어쓰면 안 된다. 번호는 선행 0을 보존하는 문자열이고, 수집 결과의 `unreviewed`를 사람의 공식 검토 완료로 승격하면 안 된다. 가이드는 실제 과거 대조 시각과 본문 편집일을 구분하고, 공동명의·버전·확인 필요·미수록을 잘못 합치지 않아야 한다.

## 합성 독립 검증

새 단위 검증 20개는 Node 표준 테스트와 `spawnSync`의 Python 3 표준 라이브러리만 사용한다. Python별 실행 제한은 8초, 캡처 출력 상한은 1MiB이다. 실제 제공사 원문·가사·이미지·credential을 fixture에 복사하지 않았으며, 테스트의 외부 조회는 모두 주입한 합성 opener로 대체한다. 예외나 응답을 담은 subprocess 출력은 실패 메시지에 되비추지 않는다.

| 경계 | 실제 검증 |
| --- | --- |
| 정확한 식별자 | TJ·KY의 `00123`을 숫자로 바꾸지 않고, `123`·`00124`와 구분. 0·전각 숫자·음수·숫자 타입·11자리 거부 |
| 제공사 구조 | TJ의 공식 목록과 KY의 `search_list_tb` 안 숫자 `search_col02`, 제목/가수 `search_col03`만 사용. 0행·다중행·orphan 행·다른 표·중첩 표·미완성 컨테이너 거부 |
| 메타데이터만 추출 | 허용된 중첩 inline 제목·가수는 유지. script/style/modal/lyrics/hidden/aria-hidden와 관계없는 주변·중첩 목록 내용 제외 |
| 입력 제한 | 엄격 UTF-8, 2MiB 초과, 과도한 중첩, 제어문자, 비어 있거나 중복된 정체성 실패 |
| 실제 수집 결과 | 합성 HTTP200 응답의 바이트 SHA-256·길이·실제 테스트 시각 범위·최종 URL·정확한 스키마를 확인하고 `verification: unreviewed`만 허용 |
| 요청과 이동 | timeout 상한·유한값·숫자 타입 검사. 주입 HTTPError503, HTTP500, read 실패와 timeout을 안전한 오류 코드로 구분. 최종 host/path/query/번호/scheme/fragment 불일치 거부 |
| 이동 전 차단 | 기본 urllib opener에 공식 URL guard가 설치되는지 확인. 외부 host·추가 query·다른 번호·HTTP·fragment의 다섯 Location을 따라가기 전에 거부 |
| 저장 보존 | 기존 후보·ledger·staging·last-good·symlink 대상 바이트 보존. 확정 상태/추가 필드/잘못된 날짜·해시·URL 후보 쓰기 거부 |
| 부분·경합·실패 | 부분 write, fsync와 link 실패에서 목적 파일 미생성·임시 파일 정리. 최종 link 직전 생긴 다른 정상 파일을 덮어쓰지 않음 |
| CLI | parse 출력은 번호·제목·가수뿐이고 확인 시각/확정 상태를 제조하지 않음. 잘못된 인자·UTF-8·수집 예외의 본문/인자/traceback·합성 비밀 sentinel 미출력, 실패 후보 미생성 |

2026-10-15 KST 로컬에서 다음 명령을 실제 실행했다. 관리 환경의 추가 네트워크 capability가 허용된 shell에서 실행하되, 테스트 자체는 실제 외부 HTTP를 요청하지 않았다.

```sh
node --test --test-isolation=none tests/provider-metadata.test.mjs
```

최종 실행은 HTTPError 경계를 포함해 **20 passed / 0 failed**, 3,105.323ms, exit0이다. 로컬 로그는 `/tmp/jpop-daily15-quality-unit-final.txt`이며 임시 파일이 다음 세션에도 존재한다고 보장하지 않는다. 직전 20개 실행 3,212.651ms와 초기 정상 16개 실행 3,171.548ms도 통과했다. 새 실행 횟수나 테스트 수를 제품/정책 승인 근거로 사용하지 않는다.

## 발견·실패와 조정

- 최초 합성 KY fixture를 초기 설명대로 번호/제목/가수 각각 열로 만들었으나 실제 저장 응답과 맞지 않았다. 실제 KY는 `search_col02`에 번호, `search_col03`에 `제목 / 가수`가 있다. root와 자료 담당의 확인을 받아 합성 fixture를 바로잡았다. 이 잘못된 fixture가 담긴 기본 sandbox 실행은 결과가 나오기 전에 중단했으므로 가상의 기능 실패 개수를 기록하지 않는다.
- 기본 제한 sandbox에서 Node의 Python subprocess가 멈췄다. 2초 제한 단독 `spawnSync` 진단은 실제 `ETIMEDOUT`, 빈 stdout/stderr를 반환했다. 같은 진단은 추가 네트워크 capability shell에서 exit0·정상 JSON을 반환했다. 오래 남은 이 담당 소유 Node PID65257만 확인 후 SIGTERM을 보냈으나 계속 실행되어 SIGKILL로 중단했고 exit137을 확인했다. 이를 추출기 오류나 통과 결과로 쓰지 않는다. 추가 capability 실행은 위 20개를 3.106초 안에 마쳤다.
- 읽기 검토에서 KY orphan 행 허용과 64자리 번호 허용을 발견해 소유자에게 전달했다. 자료 담당이 공식 표 필수·중첩 표 배제·기존 ledger와 같은 최대 10자리로 강화했다. root가 기본 urllib의 사후 URL 검사만으로는 외부 요청을 먼저 할 수 있다고 지적했고, 자료 담당이 사전 redirect guard를 구현했다. 이 담당은 각각의 실패 경계를 합성 검증으로 고정했다.
- 새 E2E의 문의 링크는 article 안으로 좁히고 실제 도착 제목 `문의와 정보 정정`을 확인하도록 했다. 즉시 URL 읽기 대신 Playwright의 재시도하는 `toHaveURL`을 사용한다.

## 새 안내 페이지의 브라우저 검사 범위

흐름: `/guides/` → 키보드로 제목·가수 표기 비교 가이드 진입 → 실제 과거 공식 근거/공동명의/버전과 상태 차이 확인 → 문의 경로 도착.

Browser plugin/`browser` skill은 현재 목록에 없어 `Browser plugin not available`로 분류했다. frontend-testing-debugging 지침을 읽고 기존 Playwright를 사용한다. 서버·빌드·브라우저 전체 실행은 root가 소유하며, 이 담당은 아래 두 E2E를 작성했다. root의 실제 결과를 받기 전에는 브라우저 통과·원격 CI·운영 반영으로 쓰지 않는다.

1. 데스크톱 1440×1100: 올바른 title/h1/URL과 빈 화면·Astro 오류 overlay·console/page error 부재, 네 공식 숫자 링크와 실제 `2026-10-15T03:03:*+09:00` 대조 시각, 본문 편집일, 정확한 공동명의 세 표기·부제 차이·현장 버전·확인 필요/미수록 설명, target별 canonical/robots/sitemap·광고 요청 없음·가로 넘침 없음.
2. JS 비활성 360×800: 가이드 여섯 개 중 새 가이드를 focus/Enter로 방문, 실제 본문·넘침 없음·문의 도착. 기존 다섯 가이드 응답의 본문 확인일은 10월14일로 보존. 기존 즐겨찾기·ID/URL 호환은 전체 기존 E2E로 계속 검사한다.

`JPOP_SCREENSHOT_DIR`이 root에 설정되면 새 두 검사는 `title-artist-guide-desktop.png`, `title-artist-guide-mobile.png`를 해당 외부 임시 경로에 저장한다. 화면 비교·이미지 열람은 실제 root/UI 담당의 증거를 받은 뒤 구분해서 기록한다. Chromium 외 브라우저·현장 기기·실제 Google 계정/심사·자료 이용 권리·광고 노출은 이 하네스의 확인 범위가 아니다.

## 인계와 동료 평가

root에 단위 20개 통과와 E2E 두 개 준비를 전달했다. 합성 검증 통과는 실제 제공사 제목/가수의 사람 판정이나 reviewed ledger 승격을 대신하지 않는다. 실제 저장 응답 재파싱·새 조회 결과는 [자료 담당 기록](../data/provider-metadata-harness-20261015.md), 새 본문은 [앱 담당 기록](title-artist-guide-20261015.md)과 구분한다.

초기 인계 시점에는 동료 평가 두 개가 아직 요청/완료되지 않았다. 아래는 이후 실제 요청을 받고 수행한 후속 결과다. 전체 CI·동일 SHA preview·운영·실제 복구·릴리스는 root의 최종 실행 기록에 연결한다.

## 09:57 재개 후 통합 결과 확인

root는 09시 동료 평가 요청 직후 세 담당의 실제 usage-limit 중단을 확인했다고 인계했다. 09:57 KST에 같은 run·기준 SHA·기존 diff와 담당을 유지해 재개했으며, 이 담당은 새 팀·구현·Git·서버·배포를 만들지 않았다. 한도 초기화를 자동 트리거로 추정하거나 중단 구간을 연속 개발로 기록하지 않는다.

09:59 KST까지 이 담당이 직접 읽은 root의 실제 로컬 로그는 다음과 같다. root가 실행한 결과를 이 담당의 새 실행으로 바꾸어 쓰지 않는다.

| 증거 | 실제 관측 |
| --- | --- |
| `/tmp/jpop-daily15-unit.txt` | 전체 307 passed / 0 failed, 9,644.836ms. 새 단위 검증 20개 포함 |
| `/tmp/jpop-daily15-check-final.txt` | Astro 26파일, errors/warnings/hints 각각 0 |
| `/tmp/jpop-daily15-preview-e2e.txt` | 기존 22개와 새 안내 2개 전부 24 passed, 30.5초. noindex preview 대상 |
| `/tmp/jpop-daily15-ui-qa.json` | UI 담당의 실제 1440×1100·1280×768·JS-off 360×800 키보드 흐름 통과. 콘솔·광고 요청·failed request·수평 넘침·overlay 각각 0 |
| [실제 제공사 2요청 기록](../operations/runs/daily-20261015-0900-live-provider-metadata.json) | root의 TJ27527·KY43372 실제 HTTP200, 별도 unreviewed 후보. 현재 script SHA256과 기록의 script hash가 일치. 가이드의 과거 checkedAt나 ledger는 갱신하지 않음 |

추가 읽기 평가에서 이 담당은 Python pure parser를 직접 import해 저장된 기존 응답 20개를 다시 읽었다. 세 metadata와 원 응답 SHA256 모두 20/20 일치했다. 바이트코드 쓰기를 비활성화하고 원문은 출력·복사하지 않았으며 새로운 제공사 요청은 0건이다. 가이드의 네 URL·원래 checkedAt도 공개 이전 요청 metadata의 단일 HTTP200 행과 4/4 일치했다.

UI 담당의 PNG 여섯 개(데스크톱·모바일·짧은 데스크톱 첫 화면, 모바일 공동명의, 데스크톱·모바일 출처)를 실제 열었다. 표기가 길어질 때의 줄바꿈, 공동명의 전체와 실제 03시 날짜, 제목/링크/브랜드 그라데이션이 보이며 잘린 본문·겹침·오류 overlay가 없었다. 이 담당은 브라우저를 새로 조작하지 않았고, 이미지와 저장된 브라우저 receipt를 읽은 범위로만 판단한다. 실제 clean SHA·원격 CI·원격 preview·production·복구·릴리스는 이 읽기 시점에 root 후속 게이트다.

## 담당자가 제출한 동료 평가 원본

두 대상의 최종 파일·문서·실제 결과를 읽고 제출했다. 점수는 각자 할당된 인수 기준 충족도이며, 서로 다른 역할의 변경량·말 수·Google 승인 여부로 순위를 만든 것이 아니다. 두 결과 모두 확인한 할당 범위에서 필수 수정 사항이 없어 같은 점수를 부여한다. 전체 여섯 평가와 검증·동률 규칙의 최종 적용은 root 담당이다.

```json
[
  {
    "reviewer": "/root/daily15_quality",
    "candidate": "/root/daily15_evidence",
    "reviewedAt": "2026-10-15T09:59:05+09:00",
    "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 5 },
    "criterionReasons": {
      "correctness": "scripts/provider-metadata.py 전체를 읽어 고정 공식 숫자 URL·선행0·최대10자리·정확한 단일행·엄격UTF8·공식KY표·차단subtree 경계와 unreviewed만 반환하는 계약을 확인했다. 사전redirect차단과 최종URL검사, exclusive hardlink 게시가 기존 ledger/staging/last-good을 덮어쓰지 않는다. 합성20개에서 partial write·fsync/link·경합·안전CLI 실패를 직접 검증했다.",
      "evidence": "기존 저장20응답을 이 담당이 pure parser로 재해석해 metadata20/20·SHA25620/20을 독립 확인했다. root 실제TJ27527/KY43372 HTTP200·unreviewed 후보2건의 안전한 receipt와 현재script hash 일치를 읽었다. 전체307unit·type0/0/0·preview24 로그를 확인했고 원문이나 새조회시각을 과거근거로 바꾸지 않았다.",
      "value": "이전 임시KY문맥 출력에 가사 일부가 섞였던 실제 문제를 영속 metadata-only 도구·안전오류·후보 분리로 개선한다. 공식 제목/가수의 사람 판정과 수집을 분리해 이후 검토를 반복할 수 있고, 실패·unknown을 미수록/자동확정으로 바꾸지 않는다.",
      "maintainability": "추가 의존성 없이 표준라이브러리의 URL/parser/fetch/record검사/exclusive쓰기/CLI 경계를 분리했다. 보수적markup 지원·hardlink 미지원 실패·20저장응답 관측 범위와 원격게이트를 문서화했다. 임시parser에만 의존하던 이전 흐름과 달리 영속20negative 회귀와 실제호출 인터페이스가 있으며 할당범위 내 필수 보완은 발견하지 못했다."
    },
    "evidencepaths": [
      "source:scripts/provider-metadata.py",
      "source:tests/provider-metadata.test.mjs",
      "docs:data/provider-metadata-harness-20261015.md",
      "docs:data/provider-review-requests-20261015.json",
      "docs:operations/runs/daily-20261015-0900-live-provider-metadata.json",
      "/tmp/jpop-provider-review-20261015/requests.json",
      "/tmp/jpop-daily15-quality-unit-final.txt",
      "/tmp/jpop-daily15-unit.txt"
    ],
    "reason": "자료 담당의 구현·문서·저장20응답·실제2요청 receipt를 실제 읽고 평가한 20점이다. markup 변경 시 보수적 실패와 권리/의미 판정 미포함은 명시된 범위이며 가짜 수집/승격으로 숨기지 않았다. 로컬 인수 기준을 수용하며 원격CI·clean SHA·운영·복구는 root의 별도 필수 게이트다.",
    "acceptance": "accept-local-implementation; clean CI/preview/production/recovery remain root gates"
  },
  {
    "reviewer": "/root/daily15_quality",
    "candidate": "/root/daily15_ui",
    "reviewedAt": "2026-10-15T09:59:05+09:00",
    "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 5 },
    "criterionReasons": {
      "correctness": "guides.ts의 새6섹션을 읽고 SPYAIR27527/43372·JANE DOE52972/57784의 전체공식표기·공동명의·버전·unknown과미수록 구분을 확인했다. 네URL/원래checkedAt가 과거단일HTTP200결과와4/4일치하고, 새본문확인일10/15와 기존5개확인일10/14가 분리된다. 가수별exact그룹의 실제동작 설명도 기존코드와 맞는다.",
      "evidence": "UI문서·원본5개 deepEqual receipt·3viewport keyboard/JSoff QA receipt·PNG6개를 직접 읽고 열었다. rootpreview24PASS30.5초의 새2E2E에서 출처·canonical/noindex·기존5날짜·문의 도착을 확인했다. 초기robots/contact제목 기대오류와 최종통과를 구분해 보존했다.",
      "value": "제목 부제·구두점만으로 같은 곡을 추정하거나 공동명의 한 사람만 읽는 실제 오류를 공식사례로 설명한다. 업체·번호·전체가수·버전·기기와 날짜를 대조하는 행동 및 불분명할 때 제보할 정보가 있어 이용자가 번호를 안전하게 고르는 데 직접 도움이 된다. 공개본문에 내부개발보고나 추정난이도/권리/승인을 넣지 않았다.",
      "maintainability": "기존guide 배열·공용목록/상세템플릿·CSS를 재사용하고 map기본값을 먼저설정해 새날짜만 선택적으로 명시했다. 기존5개객체 전체보존을 정적으로 검증했고 새클라이언트코드·스타일·저장키·URL규칙을 추가하지 않았다. 정확한4source와 소유범위/한계를 문서화해 할당범위 내 필수 보완은 발견하지 못했다."
    },
    "evidencepaths": [
      "source:src/content/guides.ts",
      "source:e2e/title-artist-guide.spec.ts",
      "docs:engineering/title-artist-guide-20261015.md",
      "docs:data/provider-review-requests-20261015.json",
      "/tmp/jpop-daily15-ui-static.json",
      "/tmp/jpop-daily15-ui-qa.json",
      "/tmp/jpop-daily15-guide-desktop.png",
      "/tmp/jpop-daily15-guide-mobile.png",
      "/tmp/jpop-daily15-guide-short-desktop.png",
      "/tmp/jpop-daily15-guide-mobile-joint.png",
      "/tmp/jpop-daily15-guide-desktop-sources.png",
      "/tmp/jpop-daily15-guide-mobile-sources.png",
      "/tmp/jpop-daily15-preview-e2e.txt"
    ],
    "reason": "앱 담당의 실제 본문·4개 과거출처·정적보존·3viewport receipt·6이미지·전체24회귀 로그를 읽은 20점이다. 두 QA기대값 실패는 제품계약을 바꿔 숨기지 않았고 실제 최종검증으로 정정했다. 순위용 감점을 만들지 않으며 새CI·운영/Google심사를 이 평가로 대신하지 않는다.",
    "acceptance": "accept-local-implementation; clean CI/preview/production/recovery remain root gates"
  }
]
```
