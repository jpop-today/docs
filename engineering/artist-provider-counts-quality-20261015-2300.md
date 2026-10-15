# 가수별 업체 확인곡 수 · 품질 검증

작업 `resume-20261015-2300` · [Issue24](https://github.com/jpop-today/jpoptoday/issues/24), 상위 [Issue6](https://github.com/jpop-today/jpoptoday/issues/6). DATA-001, CONTENT-001, SEO-001, DESIGN-001, HARNESS-001.

담당 `resume23_quality`, Git·공용 파일·서버·CI·배포는 root만 수행한다. 소스 base `dffa93bc6982c9f186a269da577e3dbc108ad189`, docs base `daa79c0828ad6906461f88a6dc5edef36842fd73`, 조직 base `b7aad18fe2f793453b6505bb150f359d05ae3554`. branch `codex/24-provider-counts-20261015`.

소유 파일은 소스 `tests/reviewed-cohort-2300.test.mjs`, `e2e/artist-provider-counts.spec.ts`와 이 문서다. 세 저장소 AGENTS·제품 스펙·status·roadmap·daily-development·continuous-development를 읽고 시작 시 source clean 상태를 확인했다. 다른 담당 파일은 읽기만 하며 Git/서버 작업을 수행하지 않는다.

## 검증 설계

- 기존 63개 ledger의 전체 객체·배열 순서를 canonical JSON SHA256 `83698b0b9d3406e768fac9fe5dfa2b3caa804477e8c06acd494d77cef32802aa`로 고정한다. frozen staging·chart 입력 hash와 3317 ID를 함께 검증한다.
- 자료 담당의 실제 조회·편집 대조 결과를 받은 뒤 4 ID/8개 provider의 번호·제목·가수·공식 URL·실제 checkedAt·response hash를 고정했다. 조사 전 성공을 가정하지 않았다. 각 source 값은 [실제 조회 기록](../data/provider-review-requests-20261015-2300.json)과 대조한다.
- 새 cohort 적용 전후 public projection에서 ID 순서·기존 URL·메타데이터와 모든 비대상 곡 전체 객체를 비교한다. 특히 같은 제목 `ドライフラワー`의 Ado ID에 優里 근거가 전파되지 않으며 TJ 관측/금영 unknown이 유지되는지 검사한다.
- E2E는 production의 `confirmedNumbers`/group helper를 가져오지 않는다. 실제 served catalog의 current playlist·provider-confirmed·충돌 없음·문자열 번호·공식 근거로 song ID Set을 계산하고 global/각 index/각 group의 TJ·금영 수, row별 실제 provider 포함 여부를 대조한다. 업체 합과 전체 ID 합집합을 구분하며 실제 0곡 그룹과 미수록 아님 설명을 확인한다.
- `/artists/` → 가수 anchor → 기존 곡 상세를 360px·JavaScript off·키보드로 이동하고 제목/공식 근거를 확인한다. canonical·대상별 robots·무광고·콘솔·overlay·가로 넘침을 검사한다. 기존 전체 E2E의 저장 목록·legacy URL 회귀를 대체하지 않는다.

렌더 검증에는 `frontend-testing-debugging` skill을 적용했다. 현재 Browser plugin/skill이 제공되지 않아 기존 Playwright를 사용했다. 브라우저/의존성 설치는 하지 않았다. 서버와 target artifact는 root가 준비했다.

## 실제 실행

2026-10-15 23:06~23:09 KST, Node `v24.19.0`, 기존 system Chromium `/usr/bin/chromium`, root의 dirty preview `http://127.0.0.1:4321`에서 실행했다. clean commit/CI artifact 검증과 구분한다.

| 검사 | 실제 결과 | 명령·범위 |
| --- | --- | --- |
| cohort unit | 3 PASS, 실패·skip 0, 214.918209ms | `node --test --test-isolation=none tests/reviewed-cohort-2300.test.mjs` |
| 새 E2E 첫 실행 | 2 실패 | 아래 테스트 단위 표기 가정 오류. 숫자 자체 mismatch가 아님 |
| 새 E2E 정정 후 | 2 PASS, 11.5초 | `JPOP_TEST_BASE_URL=http://127.0.0.1:4321 PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium npx playwright test e2e/artist-provider-counts.spec.ts` |

첫 E2E에서 테스트 담당이 표시 단위를 `24곡`으로 가정해 실제 `24개 ID`와 달랐다. 앱과 공유한 DOM count 계약은 만족했고 `data-count="24"`는 맞았다. 실제 global/index/group의 단위와 screen reader 문구를 읽어 테스트를 수정했으며 앱 코드를 바꾸거나 숫자/공식근거 조건을 완화하지 않았다. 원래 두 실패를 통과로 기록하지 않는다. 정정 때 설명의 `id="provider-count-note"`와 global·각 index·각 group의 `aria-describedby` 연결도 추가 검증했다.

독립 계산 결과는 **TJ 24곡 ID, 금영 22곡 ID, 전체 합집합 24곡 ID**다. `provider-confirmed`라도 충돌이면 제외하며 실제 TJ·금영이 겹쳐 단순합 46을 전체곡으로 표시하면 실패한다. 모든 가수 index/group의 수치·표시문구·포함된 ID Set이 일치했다. 0곡 업체는 공식 미수록이 아니라 확인근거 없음으로 설명되고, 같은 제목 Ado ID는 표시되지 않았다.

이 초기 E2E artifact는 group count의 `p`→`div role="group"` 접근성 마무리 이전 빌드였다. 기존 검사에는 설명 연결은 있었으나 role 조건은 없었다. 이를 확인한 뒤 최종 DOM의 `role="group"` 검사도 추가했다. 초기 PASS를 최종 source/clean artifact 검증으로 확대하지 않으며 root의 재빌드·전체 회귀에서 추가 조건의 실제 통과를 확인한다.

| 렌더/상호작용 | 실제 결과 |
| --- | --- |
| 페이지 URL·title·h1·본문 | PASS `/artists/`, 가수별 공식 확인곡 |
| 빈 페이지·framework overlay | PASS, overlay 0 / Internal Server Error 없음 |
| 콘솔·pageerror·광고요청 | PASS, relevant warning/error 및 광고요청 0 |
| global/index/group count·0 설명 | PASS, 독립 set와 모든 수치 일치 / 설명 aria 연결 |
| canonical·preview robots | PASS, `https://jpop.today/artists/`, `noindex, follow` |
| 1440×1100 / 360×800 가로 넘침 | PASS |
| JS off·키보드 이동 | PASS 가수 바로가기 focus→Enter→해당 anchor→곡 link focus→Enter→기존 `/songs/<stable-id>/`·제목·공식근거 |
| screenshot | 이번 E2E 명령에는 screenshot 환경변수를 지정하지 않았다. 앱 담당의 별도 시각 QA 결과를 읽기 전 이 검사에서 캡처했다고 주장하지 않는다 |

첫/정정 실행의 Node `NO_COLOR`/`FORCE_COLOR` 경고는 브라우저 콘솔 경고와 구분한다. 앱·브라우저 오류는 관측하지 않았다. unit/E2E는 환경 capability를 지정해 기존 proxy·CA를 유지했고 시크릿·전체 환경·원문 응답을 출력하지 않았다. CI·clean artifact·원격 preview/production·실제 복구·릴리스는 root의 후속 증거를 확인하기 전 완료로 쓰지 않는다. 기존 저장 목록/legacy URL 검사는 `e2e/artists.spec.ts` 등 전체 회귀에서 root가 실행한다.

## 최종 소스와 통합 결과의 읽기 확인

2026-10-15 23:16 KST에 root의 실제 로그 `/tmp/jpop-2300-unit.txt`(310 PASS·실패/skip0·11.481초), `/tmp/jpop-2300-check-final.txt`(0 errors/warnings/hints), `/tmp/jpop-2300-preview-e2e-final.txt`(26 PASS·44.6초)를 읽었다. 최종 preview 전체 회귀에는 추가한 `role="group"` 조건과 기존 저장/URL 회귀도 포함된다. root가 전한 초기 전체 coverage 24 PASS/2 실패는 공용 기존 검사의 이전 count 고정값 오류로 별도 기록하며 이 담당이나 앱의 기능 오류로 만들지 않는다.

앱 담당의 최종 3viewport 결과인 `operations/runs/resume-20261015-2300-ui-qa.json`과 6개 PNG를 실제 읽고 열었다. root가 복사한 영속 PNG와 처음 열람한 `/tmp/jpop-resume23-ui-final/`의 PNG 6개가 byte 단위로 같고 영속 JSON도 원 결과와 같음을 직접 확인했다. desktop summary/index, 360px summary/index/실제 금영0 그룹, 1280×768 마지막 그룹 화면이 각각 수치/정체성/줄바꿈/브랜드와 일치했다. 이 독립 시각 증거 역시 dirty preview이며 clean CI/원격 운영 증거로 올리지 않는다.

자료 결과도 요청8개의 원 candidate(verification unreviewed, HTTP200, 동일 final URL), 편집 decision8개, 실제 ledger append8개를 staging의 정확 ID/전체 제목/가수/관측 번호와 별도 읽기 스크립트로 일대일 대조했다. 8/8 일치, 보존 input hash4개와 결과 ledger hash도 일치했다. 실제 raw 응답은 저장하지 않았으므로 원문 재파싱이나 response hash 독립 재계산·새 공식 조회를 했다고 주장하지 않는다.

## 동료 읽기 평가

아래 두 평가는 2026-10-15 23:16 KST의 실제 소유 파일·자료·로컬 검증과 화면에 근거한다. clean CI·원격 preview/production·복구·Release는 이 성분별 수용과 별개인 root 게이트이며 이 시점에는 미완료다. 변경량·말 수로 점수를 차등하지 않았고 필수 보완 결함은 발견하지 않았다.

```json
{
  "reviews": [
    {
      "reviewer": "/root/resume23_quality",
      "candidate": "/root/resume23_evidence",
      "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 5 },
      "criterionReasons": {
        "correctness": "frozen chart의 정확 canonical 4ID와 staging의 provider 번호를 actual candidate/decision/ledger8개에 독립 대조해 전부 일치했다. 優里 근거가 같은 제목 Ado에 전파되지 않고, 원63 wholeobjects/order·3317ID와 비대상 public song은 unit3/3 및 전체310에서 보존된다.",
        "evidence": "requests JSON의 실제 HTTP200/finalUrl/checkedAt/responseBytes/hash와 원 unreviewed 후보를 보존하고 별도 reviewer/reviewedAt/이유8개로 편집 판정을 구분했다. 본인이 candidate↔decision↔ledger evidence8/8 및 보존 inputhash4개와 결과ledgerhash를 직접 대조했다. 원 응답 비저장에 따른 재파싱 불가와 새조회/심사/권리 미확인을 명시했다.",
        "value": "실제 사용자가 찾는 동결차트4곡8번호를 관측 상태에서 정확한 제공사 제목·가수·버전 근거가 있는 확인 상태로 보강한다. 원래 번호/ID를 보존하고 실제 가수 탐색의 안전 확인곡 합집합을24곡으로 넓히며, 가수전체목록·미수록·권리승인으로 과장하지 않는다.",
        "maintainability": "기존 metadata-only helper와 ADR006 ledger 형식을 재사용하고 8개 끝 append만 수행했다. 기존63 metadata/객체/순서/prefix를 보존하며 원문·가사·시크릿을 남기지 않고 요청/편집/검증/한계/다음담당을 영속 JSON과 문서로 연결했다. Git/공용파일/서버/CI/배포 소유권을 지켰다."
      },
      "reason": "할당된 실조회·정체성 대조·append/보존·안전 영속 기록 수용 기준을 실제 읽기/회귀 범위에서 만족한다. 필수 수정 없음.",
      "evidence": [
        "jpoptoday/data/reviewed/provider-corrections.json",
        "docs/data/provider-review-20261015-2300.md",
        "docs/data/provider-review-requests-20261015-2300.json",
        "jpoptoday/data/staging/legacy-catalog.json",
        "jpoptoday/data/chart-snapshot.json",
        "jpoptoday/tests/reviewed-cohort-2300.test.mjs",
        "/tmp/jpop-2300-unit.txt"
      ],
      "acceptance": "accepted_data_component; actual fetch receipt와 편집 근거/불변입력을 수용. raw response 독립재파싱·오디오·권리·Google심사는 미실행/미확인. 배포·복구·Release는 root 별도 게이트."
    },
    {
      "reviewer": "/root/resume23_quality",
      "candidate": "/root/resume23_app",
      "scores": { "correctness": 5, "evidence": 5, "value": 5, "maintainability": 5 },
      "criterionReasons": {
        "correctness": "기존 confirmedNumbers 자격을 유지하며 provider별 ID Set으로 global/index/group을 계산한다. production helper를 가져오지 않는 본인 E2E가 실제 served catalog에서 TJ24/KY22/union24와 모든 가수 수치·포함 ID를 대조했다. 0의 미수록 아님과 aria 설명 연결, 최종 div role=group, 기존 anchor/공동명의 분리/URL 보존을 확인했다.",
        "evidence": "최종 dirty preview 3viewport QA JSON과 영속 PNG6개를 실제 읽고 열었고 원 임시 증거와 바이트 동일도 확인했다. 82수치/20그룹·실제0군·정확gradient/palette·콘솔/광고/넘침0 및 키보드 이동과 화면이 일치했다. root의 finalpreview26PASS/type0/unit310 로그를 읽었고 초기 p artifact·진단 URL2/brand선택자1 오류와 최종 source 증거를 분리했다.",
        "value": "사용자가 방문할 업체를 기준으로 확인곡 범위를 가수 바로가기와 그룹에서 바로 판단하고 실제 번호/근거/기존 상세로 이동할 수 있게 한다. 두업체중복을 전체46곡으로 오해하지 않도록 union24를 설명하며 금영0을 미수록으로 단정하지 않는다. 360px JSoff에서도 본문과 키보드 이동을 제공한다.",
        "maintainability": "artists.astro 한 소유파일에서 기존 trusted projection을 재사용하며 데이터·공용CSS·저장키·공용계약을 변경하지 않는다. shared DOM count 규약과 소유doc이 다음 회귀에 연결되고, 새 clientscript/dependency 없이 SSR로 구현했다. 길어진 index의 sticky를 해제해 낮은 viewport 마지막링크 접근을 유지하고 브랜드computed값을 검증했다."
      },
      "reason": "할당된 업체별 수·의미·정적 탐색·접근성·브랜드/호환성 수용 기준을 실제 파일·독립 집계/회귀·최종 화면에서 만족한다. 필수 수정 없음.",
      "evidence": [
        "jpoptoday/src/pages/artists.astro",
        "docs/engineering/artist-provider-counts-20261015-2300.md",
        "docs/operations/runs/resume-20261015-2300-ui-qa.json",
        "docs/engineering/design/artist-provider-counts-20261015-2300-desktop-summary.png",
        "docs/engineering/design/artist-provider-counts-20261015-2300-desktop-index.png",
        "docs/engineering/design/artist-provider-counts-20261015-2300-mobile-summary.png",
        "docs/engineering/design/artist-provider-counts-20261015-2300-mobile-index.png",
        "docs/engineering/design/artist-provider-counts-20261015-2300-mobile-zero-group.png",
        "docs/engineering/design/artist-provider-counts-20261015-2300-short-last-group.png",
        "/tmp/jpop-2300-preview-e2e-final.txt",
        "/tmp/jpop-2300-check-final.txt"
      ],
      "acceptance": "accepted_app_component; final dirtypreview SSR/독립count/360JSoff·키보드·시각QA 수용. WebKit/Firefox/실제스크린리더·필드성능은 미실행. cleanCI·실제운영·복구·Release는 root 별도 게이트."
    }
  ]
}
```
