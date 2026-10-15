# 공식 번호 메타데이터 추출 하네스

작업: [Issue20](https://github.com/jpop-today/jpoptoday/issues/20) / `daily-20261015-0900` / DATA-001·HARNESS-001·CONTENT-001. 담당 `daily15_evidence`, 통합 담당 root. 소스 기준 `7ed3f7eb3203ed4bb0de0c247f07a129bba8dba0`, 문서 기준 `4525efae1f6779ac7faac2ea7dfb834ffc0b675e`, 조직 규칙 `b7aad18fe2f793453b6505bb150f359d05ae3554`. 브랜치 `codex/20-safe-provider-review-20261015`. 소유 파일은 소스 `scripts/provider-metadata.py`와 이 문서 두 개다. 테스트·앱·package/lock·Git·CI·배포·ledger/staging은 다른 담당 범위이며 수정하지 않았다.

## 목적과 경계

이전 공식 대조에서 KY 응답의 인접 문맥을 검사하던 임시 도구 출력에 가사 일부가 섞인 사건을 재발시키지 않기 위해, 공식 검색의 번호·제목·가수 셀만 읽는 Python3 표준 라이브러리 도구를 추가했다. 원문 응답·주변 문맥·예외 문자열·헤더·입력 인수·시크릿을 출력하지 않는다. 실제 가사는 테스트 fixture나 문서에 복사하지 않는다.

추출 성공은 제목·가수·버전의 사람 검토, 가수 정체성·공동명의 통합, 기기/음원 제공 범위, 권리 허락, AdSense 심사 결과를 뜻하지 않는다. 후보는 항상 `verification: "unreviewed"`다. 공식 reviewed ledger와 마지막 정상 데이터는 자동으로 수정하지 않는다. unknown을 미수록으로 바꾸지도 않는다. 실제 현재 운영/릴리스 여부는 root의 별도 CI·preview·배포 기록에서 확인한다.

## 함수 계약과 사용

`official_url(provider, number)`는 `tj`와 `ky` 및 1~10자의 ASCII 숫자 문자열을 받는다. 모두 0인 값은 거부하고 선행 0을 보존한다. URL은 기존 ledger와 같은 고정 HTTPS numeric 검색 형식이다. 사용자 입력으로 호스트·추가 query·경로를 만들지 않는다.

`parse_metadata(body: bytes, provider, number)`는 최대 2MiB의 엄격 UTF-8 바이트를 받으며, 정확히 단일 결과에서 요청 번호와 일치하는 `number`, `title`, `artist` 세 문자열만 반환한다. 오프라인 파일 재해석은 source·조회시각·검토 성공을 생성하지 않는다.

- TJ: `ul.grid-container.list` 결과 행의 직접 `li.title3`, `li.singer`, 번호 `span.num2`를 사용한다.
- KY: 직접 `td.search_col02`와 `td.search_col03`를 가진 결과 `tr`을 사용한다. 닫힌 최상위 `table.search_list_tb` 안에 있는 행만 인정하며, orphan 행과 다른/중첩 table의 행은 제외한다. 정보 셀의 단일 ` / ` 구분자로 제목/가수를 나누며 구분자 중복은 추측하지 않고 실패한다.
- 중첩 span/em/p 등 표기 markup은 보존하되 script/style/template/modal/lyrics/dialog/hidden/aria-hidden와 숨김 style의 텍스트는 저장하지 않는다. identity 셀 안의 관련 없는 table/list 텍스트도 결과에 넣지 않는다.
- 필수 행·셀의 종료 태그 누락, 중복/누락 셀, 빈 제목/가수, 복수/0개 행, 번호 불일치에 실패한다. HTML 깊이 128·노드 100,000·정규화한 identity 2,000자 상한을 둔다. 공백만 정규화하고 일본어 원문·대소문자·구두점·선행 0을 임의 통합하지 않는다.

`fetch_metadata(provider, number, *, opener=None, timeout=20)`는 제한된 실제 요청에서 HTTP200·정확한 최종 공식 URL·응답 크기와 parser 결과를 확인한 뒤에만 `schemaVersion`, `provider`, 세 metadata, `url`, `finalUrl`, UTC 실제 `checkedAt`, `responseSha256`, `responseBytes`, `status`, `verification`을 반환한다. timeout은 유한한 0초 초과~60초 이하다. 테스트에서는 주입한 opener를 사용하며, 이 결과를 실제 네트워크 조회로 기록하지 않는다. 기본 opener는 `HTTPRedirectHandler`로 요청과 다른 Location을 실제 후속 요청 전에 거부한다. `build_opener`의 기본 ProxyHandler와 HTTPS CA 신뢰를 유지하며, 주입 opener에서도 최종 URL은 다시 확인한다.

`write_candidate(path, record)`는 엄격한 unreviewed 후보 schema를 확인한 뒤 같은 디렉터리의 임시 파일에 쓰고 flush/fsync 후 hardlink로 새 JSON 경로에 exclusive 게시한다. 기존 경로·symlink가 있으면 교체하지 않으며 reviewed/staging/last-good 경로와 알려진 ledger/catalog/snapshot 이름을 거부한다. 부모 symlink도 거부한다. hardlink를 지원하지 않는 파일 시스템이나 쓰기/게시 실패는 안전한 오류로 반환하고 기존 바이트를 유지한다. 새 부모 디렉터리는 자동 생성하지 않는다. 후보를 사람 검토 없이 ledger로 승격하는 기능은 없다.

```sh
# 저장된 응답의 오프라인 해석: 출력은 number/title/artist만이다.
python3 scripts/provider-metadata.py parse tj 27527 < /tmp/jpop-provider-review-20261015/tj-27527.html

# 실제 조회: 결과는 별도 새 후보에만 게시한다. 이 담당은 실행하지 않았고 root의 후속 두 실제 조회는 아래 기록과 구분한다.
python3 scripts/provider-metadata.py fetch tj 27527 --output /tmp/new-provider-candidate.json
```

잘못된 provider/번호는 stdin 읽기 전에 거부한다. 실제 urllib HTTPError는 응답/URL 없이 `http_status`로 분류한다. CLI 인수가 틀리거나 실패하면 traceback/입력값 대신 `{ "error": "<allowlisted code>" }`만 stderr에 남긴다. 오류 코드는 `invalid_provider`, `invalid_number`, `invalid_timeout`, `invalid_body`, `response_too_large`, `invalid_utf8`, `malformed_html`, `result_count`, `missing_identity`, `number_mismatch`, `http_status`, `final_url`, `network_error`, `output_exists`, `unsafe_output`, `invalid_record`, `write_failed`, `invalid_arguments`다. 내부 예외의 메시지나 URL은 공개 오류에 덧붙이지 않는다.

## 실제 검증과 한계

2026-10-15 09:06:45 KST에 이전 [03시 실제 공식 대조](provider-review-20261015.md)의 로컬 저장 응답 20개(TJ10·KY10)를 새 pure parser로 재해석했다. `/tmp/jpop-provider-review-20261015/requests.json`의 기존 `rows[0]`와 결과 세 필드를 비교하고 원 응답 SHA256을 다시 계산했다. metadata 20/20·응답 hash 20/20 일치했다. 저장된 원문은 출력·복사하지 않았으며, 이 오프라인 시각을 새로운 공식 조회시각으로 사용하지 않았다. 원 조회의 실제 03:03 KST 시각/공식 URL/메타데이터 hash 근거는 [공개 metadata 전용 기록](provider-review-requests-20261015.json)에 남아 있다. `/tmp` 원문은 다음 환경에서 존재한다고 보장하지 않는다.

실행한 명령/범위:

```sh
python3 -m py_compile scripts/provider-metadata.py
# importlib로 함수 로드 → 이전 requests.json 20개와 각 saved HTML의
# parse_metadata(number/title/artist)·원 응답 SHA256 비교 (응답 본문 비출력)
python3 scripts/provider-metadata.py parse tj 27527 < /tmp/jpop-provider-review-20261015/tj-27527.html
```

초기 독립 합성 검사에서는 선행0·중첩 em 및 script/modal/table sentinel 제외, injected opener fetch, 새 후보 exclusive 게시·재게시 거부/기존 바이트 보존, UTF-8 오류·복수행·0행의 세 실패를 확인했다. 모두 통과했고 실제 요청 수는 0이다. 합성 sentinel은 실제 가사가 아니다. 품질 담당의 영속 회귀 테스트·root의 통합 검증 결과는 별도 증거가 추가될 때 갱신한다.

현재는 이전 20응답의 알려진 markup에 대한 보수적인 parser다. 제공사가 markup/구분자/인코딩을 바꾸면 추측하지 않고 실패하며 마지막 정상자료를 유지한다. title/artist 셀에 표시된 실제 문자의 의미나 권리 상태를 자동 판정하지 않는다. 기본 redirect handler는 다른 URL을 후속 요청 전에 거부하고, 응답의 최종 URL도 정확히 비교한다. 새 실제 fetch, 원격 CI, 브라우저 실행, 배포를 이 담당이 수행했다고 주장하지 않는다. root의 실제 후속 조회와 로컬 통합 결과는 아래에서 별도로 기록한다.

### 검토 보완과 미완료 검사

root의 초기 읽기 검토에 따라 숫자 길이를 기존 ledger 계약과 같은 1~10자로 맞추고 KY orphan 행·중첩 table을 거부하며 닫힌 공식 table 관계를 검사하도록 보완했다. 보완 후 09:08:28 KST에 저장된 이전 20응답의 metadata와 원 hash를 다시 대조해 20/20 일치를 확인했다. 기본 redirect handler의 비공식/추가 query/HTTP Location 3종을 직접 호출해 후속 요청 전에 `final_url`로 거부함을 확인했다. 11자리·0·전부0 3종 및 KY orphan 행도 거부했다. 이 검증의 실제 네트워크 요청 수는 0이다.

`node --test --test-isolation=none tests/provider-metadata.test.mjs`를 기본 sandbox에서 실행했으나 약 2분 동안 관측 가능한 출력이 없어 이 담당의 도구 session13654만 중단했다(종료130). 테스트 assertion 결과는 미관측이며 통과나 제품 실패로 계산하지 않는다. Python 직접 검사는 즉시 완료했다. 품질 담당의 확인에서 Python program의 newline escape는 정상으로 판명되어 그 진단 의심으로 코드를 변경하지 않았다. 품질 담당이 추가 network capability 환경에서 실행한 초기 영속 회귀 로그 `/tmp/jpop-daily15-quality-unit-first.txt`를 이 담당도 읽어 16/16 통과·실패0·3.17초를 확인했다. 제한 sandbox Node child 시간초과와 이 결과를 분리한다. 이후 최종 로그 `/tmp/jpop-daily15-quality-unit-final.txt`를 읽어 redirect·부분 쓰기·경합·malformed·실제형 HTTPError를 포함한 20/20 통과·실패0·3.105초를 확인했다. 이는 로그 읽기이며 이 담당의 새 회귀 실행으로 계산하지 않는다.

## root 후속 실제 조회·통합 근거

root가 승인된 안전 CLI로 TJ27527·KY43372 두 번을 실제 조회했다. [원격 원문을 보존하지 않은 영수증](../operations/runs/daily-20261015-0900-live-provider-metadata.json)을 이 담당도 읽었다. 실행 script SHA256은 `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243`이며, HTTP200·정확한 공식 URL/최종 URL·메타데이터·응답 hash/길이·실제 시각을 담은 새 unreviewed 후보 두 개만 생성했다. 실제 checkedAt는 TJ `2026-10-15T00:11:59.598676Z`(09:11:59 KST), KY `2026-10-15T00:12:03.356362Z`(09:12:03 KST)다. ledger나 가이드의 기존 03시 checkedAt는 바꾸지 않았다. 이 담당의 오프라인20재해석과 root의 새 실제2조회는 서로 다른 증거다.

root의 `/tmp/jpop-daily15-unit.txt`와 `/tmp/jpop-daily15-preview-e2e.txt`도 읽어 full unit **307/307·9.645초**, 로컬 preview **24/24·30.5초**를 확인했다. 이 담당이 같은 검사를 새로 돌렸다는 뜻은 아니다. dirty local 검증을 clean SHA CI·원격 preview·운영·복구·릴리스의 증거로 바꾸지 않는다. root가 수행할 후속 통합 게이트는 별도다.

## 실제 동료 읽기 평가

09시 요청 뒤 실제 usage-limit으로 중단된 동일 run을 root가 cloud178/current/running·미커밋 diff 보존 확인 후 재개했다. 새 팀/worker/Git/배포를 만들지 않았다. 아래 평가는 다른 두 산출물의 실제 코드·문서·기존 검증 로그와 이미 생성된 UI PNG를 읽은 결과다. 이 담당은 새 브라우저나 외부 조회를 실행하지 않았다.

UI 평가에서는 Node24로 `guides.ts`와 공개된 이전 요청 metadata JSON을 읽기 전용 import하여, 09:58:22 KST에 source4개의 exact URL/checkedAt/HTTP200/단일 행 및 각 행의 전체 number/title/artist가 실제 본문에 포함됨을 다시 확인했다. 기존5가이드 본문 확인일10월14일도 확인했다. 이전5객체 전체 deep equality는 UI의 static receipt를 읽은 범위이고 이번 독립 명령에서는 날짜만 비교했다. desktop/mobile/mobile-joint/mobile-sources PNG 네 개를 실제 열어 읽어 가로 넘침·겹침·오버레이가 없는 것을 확인했다. UI의 3viewport/키보드 흐름은 기존 receipt를 읽어 수용했으며 독립 재실행으로 쓰지 않는다.

### 앱·디자인 담당

```json
[
  {
    "reviewer": "/root/daily15_evidence",
    "candidate": "/root/daily15_ui",
    "scores": {
      "correctness": 5,
      "evidence": 5,
      "value": 5,
      "maintainability": 4
    },
    "criterionReasons": {
      "correctness": "실제 guides.ts 여섯 섹션과 이전 공개 요청 4건을 독립 import로 대조했다. 전체 번호·원문 제목·가수/공동명의·exact URL·원래 03시 checkedAt가 일치하고, 글 편집일과 조회일·unknown/미수록·현장 버전을 구분한다. 기존 5개 날짜도 보존한다.",
      "evidence": "UI static receipt와 3 viewport QA JSON의 실제 시각/정확한 URL/키보드/무광고/noindex/콘솔0을 읽었다. 실제 PNG 4개를 열어 화면을 확인했다. 두 잘못된 QA 기대값 실패도 기록하고 dirty 로컬과 미실행 원격을 구분한다.",
      "value": "SPYAIR의 부제/구두점과 JANE DOE의 공동명의를 실제 공식 기록으로 설명하여 한글 표기나 제목만으로 잘못 예약할 가능성을 줄인다. 업체·버전·근거 확인·제보로 이어지는 유용한 한국어 자체 콘텐츠다.",
      "maintainability": "기존 공용 템플릿/CSS를 사용하고 기존 5객체·source 날짜를 보존했다. 다만 전체 기존 객체 동등성과 4사례 전체 제목/가수의 엄격 대조는 임시 static assertion에 있고 영속 E2E는 주로 날짜·링크·대표 문구를 고정하므로 향후 편집의 전체 근거 일치를 같은 범위로 재현하는 검사가 추가되면 좋다."
    },
    "evidencepaths": [
      "jpoptoday/src/content/guides.ts",
      "docs/engineering/title-artist-guide-20261015.md",
      "docs/data/provider-review-requests-20261015.json",
      "/tmp/jpop-daily15-ui-static.json",
      "/tmp/jpop-daily15-ui-qa.json",
      "/tmp/jpop-daily15-guide-desktop.png",
      "/tmp/jpop-daily15-guide-mobile.png",
      "/tmp/jpop-daily15-guide-mobile-joint.png",
      "/tmp/jpop-daily15-guide-mobile-sources.png",
      "/tmp/jpop-daily15-preview-e2e.txt"
    ],
    "reason": "실제 과거 근거의 원문 전체/날짜를 독립 확인하고 렌더 증거를 읽어 수용한다. 새 공식 조회·원격 CI·운영 배포·Google 승인을 UI 담당의 성과로 가정하지 않는다. 유지보수 보완은 전체 근거 회귀의 영속성에 관한 비차단 의견이다."
  }
]
```

### 하네스·품질 담당

```json
[
  {
    "reviewer": "/root/daily15_evidence",
    "candidate": "/root/daily15_quality",
    "scores": {
      "correctness": 5,
      "evidence": 5,
      "value": 5,
      "maintainability": 4
    },
    "criterionReasons": {
      "correctness": "20 단위 테스트와 2 E2E를 실제 읽었다. 문자열 선행0/행수/공식 표/금지 텍스트/UTF8/크기/날짜/HTTPError/이동 전 차단/부분 쓰기/경합/기존 바이트/CLI 비출력을 검사하며 가이드의 원래 4시각·target별 index/sitemap·JS-off 문의 흐름을 고정한다.",
      "evidence": "최종 20 PASS 0 FAIL 3.105초 로그와 root 307 PASS/preview 24 PASS 30.5초 로그를 읽었다. 잘못된 초기 KY fixture와 sandbox ETIMEDOUT/exit137을 기능 실패나 통과로 꾸미지 않고, 새 20 검사는 합성 opener이며 실제 조회가 아님을 명시한다.",
      "value": "과거 원문 누출 사건의 재발과 잘못된 번호 확정·실패 시 정상 자료 손상을 실제 negative 경계로 막는다. 제품 가이드에서는 공동명의·업체·현장 버전·확인 필요/미수록 구분과 키보드 도착을 회귀로 남긴다.",
      "maintainability": "합성 최소 fixture/재사용 row builder/제한된 subprocess/캡처 출력 비반사 방식은 안전하다. 다만 긴 Python 프로그램을 JS template string에 여러 줄 escape로 삽입한 테스트가 많아 fixture나 쓰기 경계 변경 시 읽기/부분 수정 난도가 높고, 후속에 안전한 공용 Python 회귀 helper로 구조를 명료화할 여지가 있다."
    },
    "evidencepaths": [
      "jpoptoday/tests/provider-metadata.test.mjs",
      "jpoptoday/e2e/title-artist-guide.spec.ts",
      "docs/engineering/provider-metadata-quality-20261015.md",
      "/tmp/jpop-daily15-quality-unit-final.txt",
      "/tmp/jpop-daily15-unit.txt",
      "/tmp/jpop-daily15-preview-e2e.txt"
    ],
    "reason": "검사 대상과 실제 검증 범위를 코드/로그로 대조해 수용한다. 새 단위 20개는 실제 제공사의 사람 검토를 대체하지 않고 root의 로컬 24 E2E를 원격 CI/운영 통과로 해석하지 않는다. 유지보수 의견은 cross-language 검사 문자열 가독성에 관한 비차단 보완이다."
  }
]
```
