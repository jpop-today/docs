# 제목·가수 표기 비교 가이드 · 2026-10-15

`CONTENT-001` · `SEO-001` · `DESIGN-001` · [Issue #20 작업 계약](https://github.com/jpop-today/jpoptoday/issues/20#issuecomment-6181213077) · [제품 스펙](../product/spec.md).

## 범위와 인수 기준

| 항목 | 실제 배정 |
| --- | --- |
| 실행 / 담당 | `daily-20261015-0900` / `/root/daily15_ui` |
| source base / branch | `7ed3f7eb3203ed4bb0de0c247f07a129bba8dba0` / `codex/20-safe-provider-review-20261015` |
| docs / 조직 base | `4525efae1f6779ac7faac2ea7dfb834ffc0b675e` / `b7aad18fe2f793453b6505bb150f359d05ae3554` |
| 단독 변경 범위 | source `src/content/guides.ts`, docs 이 파일 |
| 읽은 기준 | 세 저장소의 AGENTS, Issue20 스펙, 기존 5개 가이드·공용 가이드 페이지·가수별 확인곡, [이전 실제 대조 문서](../data/provider-review-20261015.md)와 [20개 요청 메타데이터](../data/provider-review-requests-20261015.json) |
| 수용 기준 | 실제 사례와 공식 링크·원래 checkedAt가 일치하는 한국어 자체 가이드 1개, 기존 5개 본문·URL·sources·editor·본문 확인일 보존, 표기/버전/업체/확인 필요와 미수록 구분, JS-off·모바일·키보드로 읽고 이동 가능 |
| 통합 경계 | Git·공용 계약·package/lockfile·CI·수량 기준·build·서버·preview·운영 배포·릴리스는 root 담당. 담당자의 로컬 편집·검증을 원격 CI/배포 또는 Google 승인으로 표현하지 않음 |

## 제품 변경

새 경로는 `/guides/title-artist-variants/`, 제목은 **제목·가수 표기가 다를 때 같은 곡인지 확인하는 법**이다. 기존 공용 가이드 목록과 상세 템플릿을 사용하고 별도 레이아웃·CSS·클라이언트 스크립트는 추가하지 않았다. 공식 표기 차이를 읽는 데 필요한 여섯 단계와 실제 사례를 썼다.

1. 사용할 업체와 번호를 먼저 짝짓고 같은 결과의 제목·가수·부제·버전을 읽는다.
2. SPYAIR의 `サムライハート` 부제에 있는 `it`/`It`, 느낌표·괄호·공백·작품명 차이는 기록된 두 번호의 전체 맥락으로 비교한다. 구두점만으로 같은 곡/다른 곡을 판정하는 규칙을 만들지 않는다.
3. `JANE DOE`의 저장된 공동명의 `米津玄師，宇多田ヒカル`와 TJ의 `米津玄師 & 宇多田ヒカル`, 금영의 `米津玄師、宇多田ヒカル`를 양쪽 이름 전체로 비교한다. 연결 기호 차이를 다른 항목을 합치는 허가로 일반화하지 않는다.
4. 가수별 목록은 정확한 기존 원문 표기로 묶는다는 실제 동작을 설명한다. 한글 표기가 공동명의 전체를 담지 않을 수 있다는 한계를 안내하며 새 번역·별칭·참여자 정보를 추정하지 않는다.
5. 글의 편집일·이전 번호 조회 시각·방문 기기의 현재 표시를 구분하고 업체·가수·언어·버전이 불분명하면 예약을 보류하도록 안내한다.
6. 확인 필요는 미수록 확정이 아니며, 공식 검색 실패나 빈 값을 다른 업체 번호로 채우지 않는다. 곡 주소·업체·번호·차이·확인 날짜·공식 링크로 제보하도록 안내한다.

가사·음원·이미지·난이도·권리 허가·Google 심사 결과를 추가하지 않았다. 공개 본문에 내부 개발·추출 하네스 작업을 설명하지 않았다. blue `#4f7cff`→pink `#ff7aa8` 브랜드와 pink/mint/blue 배경은 기존 공용 스타일을 그대로 사용한다.

## 실제 출처와 날짜의 구분

이 작업은 **2026-10-15에 작성·검토한 자체 안내**다. 이 담당자는 공식 검색을 새로 요청하지 않았다. 아래는 이전 실행에서 실제 조회·사람 검토를 완료한 메타데이터를 읽어 재사용한 것이며, 작성 시각으로 `checkedAt`를 갱신하지 않았다. 전체 제목/가수는 앞서 링크한 요청 메타데이터의 단일 행과 대조했다.

| 사례 / 업체·번호 | 실제 이전 checkedAt, 한국 시간 | 공개 가이드의 source |
| --- | --- | --- |
| SPYAIR / TJ 27527 | `2026-10-15T03:03:41.493546+09:00` | [공식 numeric 검색](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27527&strWord=Y) |
| SPYAIR / 금영 43372 | `2026-10-15T03:03:44.173265+09:00` | [공식 numeric 검색](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43372) |
| JANE DOE / TJ 52972 | `2026-10-15T03:03:47.678068+09:00` | [공식 numeric 검색](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52972&strWord=Y) |
| JANE DOE / 금영 57784 | `2026-10-15T03:03:50.023395+09:00` | [공식 numeric 검색](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57784) |

`guides.ts`의 map은 기본값 `editor/reviewedAt`를 먼저 쓰고 각 가이드를 펼치도록 순서를 바꿨다. 이로써 새 가이드만 `reviewedAt: '2026-10-15'`를 가질 수 있다. 기존 5개는 원래 데이터 객체 전체와 동일한 `editor: 'jpop.today 편집'`, `reviewedAt: '2026-10-14'`로 남는다. 기존 source의 URL/조회 시각도 보존한다.

## 실제 검증과 현재 한계

### 정적 검증

2026-10-15T00:06:16.707Z에 Node24의 실제 모듈 import 및 strict assertion으로 다음을 확인했다. 임시 receipt는 `/tmp/jpop-daily15-ui-static.json`이다.

- base SHA의 `guides.ts`를 `git show`로 읽은 뒤 기존 5개 객체 전체와 현재 첫 5개를 `assert.deepEqual`로 비교: **5개 전부 동일**.
- 가이드 6개·고유 slug 6개, 새 `title-artist-variants`의 본문 확인일 2026-10-15·편집자·여섯 섹션·공식 sources 4개 확인.
- 네 source URL을 실제 이전 요청 JSON에서 각각 1건 찾고 checkedAt 문자열·HTTP200·결과 1행과 대조: **4/4 일치**. 신규 제공사 요청은 **0건**.
- 후속 strict assertion에서 네 공식 행의 전체 제목·가수·번호 문자열이 새 가이드 본문에 실제 포함되고, 현재 ledger의 네 `confirm` 근거 title/artist/URL/checkedAt와도 일치함을 확인했다. 짧게 줄인 출처 링크 label만으로 본문 표기를 검증한 것이 아니다.
- 첫 검증 시도에서 Node `execFileSync('git', ...)`가 sandbox `spawnSync git EPERM`으로 실패했다. 가이드나 근거 판정 실패가 아니며, 그 시도를 통과로 기록하지 않았다. top-level `git show`를 `/tmp`에 읽고 Node는 파일만 읽도록 분리한 뒤 동일 비교를 실제 통과했다.

### 실제 렌더와 상호작용 검증

`frontend-testing-debugging` 스킬을 읽었다. 현재 도구/스킬 목록에 Browser plugin과 browser skill이 없어 **Browser plugin not available**로 분류했다. root가 실제 시작하고 허용한 `http://127.0.0.1:4321` 서버에서 기존 Playwright와 `/usr/bin/chromium`을 사용했다. 이 담당자는 서버·포트·Git을 별도로 변경하지 않았다. 스크린샷·임시 스크립트는 `/tmp`에 있다.

실제 흐름은 **가이드 목록 → 새 가이드 링크 focus·Enter → 정확한 상세 URL·제목·원래 날짜의 네 공식 근거를 읽음 → 문의와 정정 링크 focus·Enter → `/contact/`의 문의와 정보 정정 제목 확인**이다. 최종 실행 시각은 **2026-10-15T00:11:42.387Z–00:11:47.154Z (한국 시간 09:11:42–09:11:47)**이다. 원문 가이드의 본문·문의 안내만 렌더했으며, 공식 외부 링크를 클릭하거나 새 제공사 조회는 하지 않았다.

| 검사 | 실제 결과 |
| --- | --- |
| 화면 | 1440×1100 JS-on, 360×800 JS-off, 1280×768 JS-on 모두 통과 |
| 페이지 식별·본문 | exact URL/title/h1, 본문 확인일 10/15, 공동명의 세 표기·본문 2,698자 확인 |
| 공식 근거 | 네 링크의 exact URL·원래 checkedAt·새 탭·noopener/noreferrer 확인 |
| preview 정책 | 계약의 `noindex, follow`, canonical `https://jpop.today/guides/title-artist-variants/` 확인 |
| 빈 화면·오버레이 | 의미 있는 본문, framework overlay 0개 |
| 콘솔·네트워크 | 세 화면 모두 error/warning/failed request/광고 요청 각각 0개 |
| 레이아웃 | document scrollWidth가 각각 1440/360/1280으로 viewport와 같고, 검사한 본문 h1/h2/p/li의 수평 넘침 0개 |
| 기존 색감 | computed brand `linear-gradient(135deg, rgb(79, 124, 255), rgb(255, 122, 168))`와 pink/mint/blue 배경 확인 |
| 키보드 이동 | 세 화면 모두 가이드 목록 → 새 가이드 → 문의 흐름을 실제 Enter로 실행 후 exact URL/h1 확인 |
| 시각 검토 | 데스크톱·모바일·짧은 데스크톱 첫 화면과 모바일 공동명의, 두 화면의 공식 출처 PNG 6개를 직접 열어 읽고 넘침·겹침·잘린 컨트롤·오버레이가 없는 것을 확인 |

명령은 임시 스크립트 `node /tmp/jpop-daily15-ui-qa.mjs`이며 최종 receipt는 `/tmp/jpop-daily15-ui-qa.json`이다. Playwright의 focus/Enter 이후 URL을 즉시 읽지 않고 `expect(page).toHaveURL`로 완료를 기다렸다. 임시 QA 스크립트의 첫 두 실패도 숨기지 않고 각각 보존했다.

- 00:11:04Z 시작 첫 시도는 preview를 `noindex, nofollow`로 잘못 기대해서 실패했다. 실제 계약 `contracts/publication.mjs`의 `noindex, follow`가 렌더와 일치함을 읽고 QA 기대값만 고쳤다. 원본은 `/tmp/jpop-daily15-ui-qa-robots-expectation-failed.json`이다.
- 00:11:24Z 시작 두 번째 시도는 문의 페이지 h1을 링크 문구 `문의와 정정`과 같다고 잘못 기대해서 실패했다. 실제 기존 `src/pages/contact.astro`의 제목 `문의와 정보 정정`을 읽고 기대값만 고쳤다. 원본은 `/tmp/jpop-daily15-ui-qa-contact-expectation-failed.json`이다.

두 실패에서 앱 소스나 색인·제목 계약을 완화/변경하지 않았다. 세 번째 동일 흐름의 세 viewport 검증이 실제 통과했다. root가 별도로 알려 준 type 0error/0warning/0hint·preview artifact 3,336HTML/3,346files/0index 결과는 담당자의 독립 실행으로 바꾸어 주장하지 않는다. 이 로컬 dirty 렌더는 clean 동일 SHA CI/원격 preview/운영 배포의 대체 증거가 아니며 그 후속 게이트는 root 담당이다.

스크린샷은 `/tmp/jpop-daily15-guide-desktop.png`, `/tmp/jpop-daily15-guide-mobile.png`, `/tmp/jpop-daily15-guide-short-desktop.png`, `/tmp/jpop-daily15-guide-mobile-joint.png`, `/tmp/jpop-daily15-guide-desktop-sources.png`, `/tmp/jpop-daily15-guide-mobile-sources.png`이다. root는 검토 가능한 공개 증거가 필요하면 시크릿/원문이 없는 실제 파일만 자신의 통합 범위에서 보존한다.

공식 메타데이터의 사용권 전체, 실제 기기 수록·오디오·난이도, Google 계정의 실제 심사 상태와 Chromium 외 브라우저·실측 필드 성능은 이번 확인 범위가 아니다. 링크를 누르면 제공사의 현재 결과가 달라질 수 있다.

## 실제 읽기 전용 동료 평가 원본

root의 같은 실행 재개 지시에 따라 2026-10-15 한국 시간 09:58경 두 산출물의 실제 파일·문서·로그를 읽었다. 아래 평가는 미커밋 로컬 산출물과 그때까지 존재하는 검증만 대상으로 하며 원격 CI·새 clean SHA·배포·복구·릴리스의 평가로 바꾸지 않는다. 두 후보 파일이나 테스트를 수정하지 않았다.

자료 담당 검토에서는 `sys.dont_write_bytecode=True`로 Python 함수를 읽어 이전 저장 응답 20개를 직접 재해석했다. 공개 metadata JSON의 각 세 필드·원 응답 hash가 **20/20** 일치함을 독립 확인하고, root의 실제 새 HTTP 두 건 receipt에서 현재 script hash·exact URL·metadata·HTTP200·unreviewed를 확인했다. 응답 본문을 출력하지 않았으며 이 평가자의 새 외부 요청은 **0건**이다. 품질 담당의 독립 20unit 로그(20 passed, 3,105.323ms), root 전체 307unit(실패0, 9,644.836ms)와 preview 24browser(30.5s)의 실제 로그를 읽었다. sandbox 중단·미관측 assertion과 정상 완료 로그를 구분했다.

```json
[
  {
    "reviewer": "/root/daily15_ui",
    "candidate": "/root/daily15_evidence",
    "scores": {"correctness": 5, "evidence": 5, "value": 5, "maintainability": 4},
    "criterionReasons": {
      "correctness": "실제 Python 구현은 엄격 UTF-8/2MiB/1~10 ASCII 문자열 번호·단일 공식 행·정확한 title/artist/number를 검사한다. KY 공식 최상위 table 관계·TJ 직접 identity 셀, 숨김/가사/modal/script 제외, 사전 redirect guard와 사후 exact URL·HTTP200 검사, 실제 fetch만 unreviewed/UTC/hash를 만드는 경계가 코드와 일치한다. 별도 후보의 exclusive hardlink 게시·기존 경로/보호 경로 거부로 reviewed ledger나 마지막 정상 바이트를 덮어쓰지 않는다.",
      "evidence": "실제 script/소유 문서를 전부 읽고 저장 원문 20개를 이 평가자가 stdout 비출력으로 pure parser 재해석해 기존 metadata20/20·원 SHA25620/20을 독립 확인했다. root live2 receipt의 현재 scriptHash·exact URL·HTTP200·원래 metadata·새 실제 checkedAt·unreviewed도 읽어 확인했다. 새 실제 fetch2와 과거 저장20의 재해석, 담당자 미실행/중단과 품질20unit/root307·preview24 정상 로그를 구분하며 Google/권리를 만들어내지 않는다.",
      "value": "이전 인접 문맥 출력에 가사가 섞인 실제 사건에 대해 제목·가수·번호 셀만 취하는 재사용 도구와 안전한 오류 코드 경계를 제공한다. 자동 사람 판정이나 현재 기기 보장 없이 새 후보만 남기므로 근거 수집을 반복하면서도 기존 사용자 번호·정체성·확인 필요 상태를 보존할 수 있다.",
      "maintainability": "Python 표준 라이브러리만 사용하고 pure parse/fetch/exclusive write/CLI를 나눠 주입 가능한 opener와 고정 오류 코드를 제공한다. 제공사별 클래스·KY의 단일 구분자와 수동 HTML 구조 검사는 외부 markup 변경 때 유지 작업이 필요하며 현재20저장응답에 대한 보수적 지원이라는 한계를 문서화했다. 변경 시 추측하지 않고 실패하는 설계와 독립 회귀가 있어 현재 수용을 막는 결함은 발견하지 못했다."
    },
    "evidence": ["/workspace/jpoptoday/scripts/provider-metadata.py", "/workspace/docs/data/provider-metadata-harness-20261015.md", "/workspace/docs/data/provider-review-requests-20261015.json", "/tmp/jpop-provider-review-20261015/requests.json", "/workspace/docs/operations/runs/daily-20261015-0900-live-provider-metadata.json", "/workspace/jpoptoday/tests/provider-metadata.test.mjs", "/tmp/jpop-daily15-quality-unit-final.txt", "/tmp/jpop-daily15-unit.txt", "/tmp/jpop-daily15-preview-e2e.txt"],
    "reason": "실제 코드·원 응답의 비출력 재검증·실제 요청 receipt·독립 회귀 근거가 데이터/콘텐츠 안전 경계와 일치한다.",
    "acceptance": "accepted-local; 사람 검토/권리/실제기기/Google심사와 이후 clean 동일SHA CI·원격preview·운영/복구는 별도 root 게이트"
  },
  {
    "reviewer": "/root/daily15_ui",
    "candidate": "/root/daily15_quality",
    "scores": {"correctness": 5, "evidence": 5, "value": 5, "maintainability": 4},
    "criterionReasons": {
      "correctness": "20개 실제 테스트가 합성 fixture로 선행0·다중/누락/잘못된 결과 구조·UTF-8/크기/제어문자·숨김/가사 sentinel·HTTP/timeout/read/redirect·unreviewed/hash/시간·기존 파일/부분쓰기/fsync/link/경합/CLI 비반사 경계를 독립 기대값으로 검사한다. 두 E2E는 실제 새 가이드의 네 원래 source/checkedAt·공동명의·버전/미수록 설명과 target별 robots/sitemap, JS-off360 키보드 이동·기존 다섯 날짜를 검증하고 문의를 실제 h1로 확인한다.",
      "evidence": "20unit/두E2E/소유 문서 전체와 actual20PASS 로그, root307PASS·preview24PASS 로그를 직접 읽었다. 문서가 sandbox child ETIMEDOUT/중단과 합성 KY fixture 보정을 숨기지 않고 최종 정상 실행과 구분한다. 새 E2E의 exact source/time 기대값이 제가 비교한 기존metadata4개 및 실제UI receipt와 일치하고 UI 담당의 독립 세 viewport 렌더에서도 같은 핵심 흐름이 통과했다. 원격 CI/운영을 완료했다고 주장하지 않는다.",
      "value": "자료 추출이 자동 확정/불필요 원문 출력/기존 정상자료 손실로 이어지는 핵심 위험과 새 콘텐츠의 잘못된 확인일·공동명의·색인을 모두 회귀 검사한다. 새 guide만 추가한 수량 변경과 기존 URL/저장 호환을 기존 전체E2E와 함께 검증해 단순 본문 생성보다 실제 사용 경로를 보호한다.",
      "maintainability": "새 의존성 없이 Node test/기존 Playwright에 통합하고 Python subprocess timeout8초·buffer1MiB·고정 실패문구로 비밀/원문 재출력을 막는다. JS 문자열 속 Python 프로그램이 길어 문법/fixture 수정 때 읽기 비용이 있고 E2E의 몇 본문 문구는 편집과 함께 관리해야 한다. 이 중복은 현재 독립 기대값과 안전 경계 검증의 의미가 있으며 현재20unit·전체24browser가 통과했다."
    },
    "evidence": ["/workspace/jpoptoday/tests/provider-metadata.test.mjs", "/workspace/jpoptoday/e2e/title-artist-guide.spec.ts", "/workspace/docs/engineering/provider-metadata-quality-20261015.md", "/tmp/jpop-daily15-quality-unit-final.txt", "/tmp/jpop-daily15-unit.txt", "/tmp/jpop-daily15-preview-e2e.txt", "/workspace/docs/data/provider-review-requests-20261015.json", "/tmp/jpop-daily15-ui-qa.json", "/tmp/jpop-daily15-guide-mobile.png", "/tmp/jpop-daily15-guide-desktop-sources.png"],
    "reason": "독립 합성 경계·실제 렌더와 원래 공식 근거를 읽고 검증한 결과가 하네스 계약에 부합하며 중단과 통과 이력이 추적 가능하다.",
    "acceptance": "accepted-local; clean 동일SHA CI·원격preview·실제운영/복구/Release는 root 후속이며 Chromium외·권리·Google실제심사는 검사 범위밖"
  }
]
```
