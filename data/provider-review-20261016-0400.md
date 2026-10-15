# 동결 차트 15–18위의 공식 번호 검토 · 2026-10-16 04시

`DATA-001` · `CONTENT-001` · `SEO-001` · `DESIGN-001` · `HARNESS-001` · [Issue27](https://github.com/jpop-today/jpoptoday/issues/27) · [상위 Issue6](https://github.com/jpop-today/jpoptoday/issues/6) · [제품 스펙](../product/spec.md) · [ADR-006](../adr/006-reviewed-corrections-and-production.md).

## 작업 계약

| 항목 | 실제 범위 |
| --- | --- |
| run / 담당 / 통합 | `resume-20261016-0400` / `/root/resume04_evidence` / root 단독 Git·CI·배포 |
| source base / branch | `ce1b49f1ab6cedb64dca7cd2b215e72aafc76e50` / `codex/27-chart-confirmed-20261016` |
| docs base | `5d1964dfeacdad5c6b2c81d2522ce3aadb83de05` |
| 조직 규칙 base | `b7aad18fe2f793453b6505bb150f359d05ae3554` |
| 단독 편집 | source `data/reviewed/provider-corrections.json`, docs의 이 문서와 `data/provider-review-requests-20261016-0400.json` |
| 후보 / 수용 기준 | 동결 차트15–18위 current/playlist4ID의 TJ4·KY4 관측 문자열 번호를 실제 공식 조회하고 전체 표시 제목·가수·부제·명의·정확한 번호/URL/실제 시각으로 개별 판단 |
| 결과 | 실제8요청 모두 HTTP200·단일 일치번호. confirm6만 기존71항목 뒤 append, 불명 KY2는 defer. 총77항목(confirm64/suppress13) |
| 담당 검증 경계 | 실제8요청·편집판단·원본보존·기존 validator/projection·기존8계약 회귀 완료. 전체 검사·브라우저·CI·같은 SHA 배포·복구·Release는 root의 별도 실제 결과 필요 |

세 저장소의 AGENTS·제품의 Issue27 스펙·실행 계약을 읽고 담당 파일만 편집했다. root가 확인한 환경 revision263/current/running·네트워크 unrestricted/enforced·Cloudflare binding 준비 상태는 [실행 기록](../operations/runs/resume-20261016-0400.json)에 있다. 이 담당은 실제 공식 요청에 추가 network capability와 기존 proxy·CA/TLS 경로를 사용했다. 새 하위 agent·사용자 채팅·Git 변경·서버·공용 package/lockfile·CI·배포 조작은 하지 않았다. 다른 담당의 작업 파일은 변경하지 않았다.

차트는 기존 `2026-10-13T10:26:45.525Z` 스냅샷이며 요청 기간 `2026-09-29–2026-10-13`, legacy revision `94d615f5e355f4b00ebbb824547ac6dfa034ad18`을 보존했다. 이번 요청 시각을 새 차트 수집시각으로 기록하거나 순위·표시 원문을 바꾸지 않았다. 각 chart canonicalId를 staging의 current/playlist 안정 ID와 연결하고 기존 관측 문자열 번호를 읽었다. 번호만 같다는 이유로 확정하지 않았다.

## 개별 편집 판단

자동 추출 후보8개는 모두 `verification: unreviewed` 원형으로 보존했다. 아래 판단8개는 별도 reviewer·reviewedAt·reason을 가진 편집 기록이며, confirm6개에만 대응하는 ledger 항목이 있다. 두 defer는 suppress도 아니며 기존 관측 번호를 그대로 둔다.

### 동결 15위 · TJ `27031` · confirm

- 안정 ID: `jpop-playlist-samfree-ルカルカ-ナイトフィ-ハ-f6722acd10`
- staging 전체 원문: `ルカルカ★ナイトフィーバー` / `samfree`
- frozen chart 전체 원문: `ルカルカ★ ナイトフィーバー` / `samfree(Feat.巡音ルカ)`
- 실제 공식 표시셀 전체: `ルカルカ★ ナイトフィーバー` / `samfree(Feat.巡音ルカ)`

공식 제목의 ★ 뒤 공백과 samfree(Feat.巡音ルカ) 전체 명의가 동결차트15위 원문과 정확히 일치합니다. staging의 기본 제목·samfree와 차트 canonicalId가 연결된 범위에서 Feat.巡音ルカ 표기를 근거에 그대로 보존합니다. 가수 별칭을 합치거나 다른 버전·음원 동일성을 추정하지 않습니다.

### 동결 15위 · KY `43185` · defer

- 안정 ID: `jpop-playlist-samfree-ルカルカ-ナイトフィ-ハ-f6722acd10`
- staging 전체 원문: `ルカルカ★ナイトフィーバー` / `samfree`
- frozen chart 전체 원문: `ルカルカ★ ナイトフィーバー` / `samfree(Feat.巡音ルカ)`
- 실제 공식 표시셀 전체: `ルカルカ★ナイトフィーバー` / `SAM feat.巡音ルカ`

공식 전체 제목 ルカルカ★ナイトフィーバー와 번호는 일치하지만 공식 가수 SAM feat.巡音ルカ와 staging의 samfree, 동결차트의 samfree(Feat.巡音ルカ) 사이에서 SAM과 samfree가 같은 명의라는 근거를 이 조회만으로 확보하지 못했습니다. 별칭 동일성을 생성하지 않고 KY43185를 기존 observed-unverified로 보존하며 confirm/suppress하지 않습니다.

### 동결 16위 · TJ `68868` · confirm

- 안정 ID: `jpop-playlist-ado-逆光-c8c1ba8b20`
- staging 전체 원문: `逆光` / `Ado`
- frozen chart 전체 원문: `逆光(映画 'ONE PIECE FILM RED' OST)` / `Ado`
- 실제 공식 표시셀 전체: `逆光(映画 'ONE PIECE FILM RED' OST)` / `Ado`

공식 전체 제목 逆光(映画 'ONE PIECE FILM RED' OST)와 가수 Ado는 동결차트16위 전체 원문과 정확히 일치합니다. staging의 기본 제목 逆光 및 동일 canonicalId에 대응하는 영화 OST 부제를 근거에 그대로 보존합니다. 다른 편곡·수록 버전·음원 동일성을 추정하지 않습니다.

### 동결 16위 · KY `76509` · defer

- 안정 ID: `jpop-playlist-ado-逆光-c8c1ba8b20`
- staging 전체 원문: `逆光` / `Ado`
- frozen chart 전체 원문: `逆光(映画 'ONE PIECE FILM RED' OST)` / `Ado`
- 실제 공식 표시셀 전체: `逆光(ウタ from ONE PIECE FILM RED)..` / `Ado`

공식 가수 Ado와 번호는 일치하지만 표시된 전체 제목 셀 자체가 逆光(ウタ from ONE PIECE FILM RED)..로 끝나 생략된 내용을 알 수 없습니다. 이 문자열을 완전한 OST 부제로 바꾸거나 TJ 버전과 같다고 추정하지 않습니다. KY76509는 기존 observed-unverified로 보존하며 confirm/suppress하지 않습니다.

### 동결 17위 · TJ `68677` · confirm

- 안정 ID: `jpop-playlist-米津玄師-kick-back-5f2ec0135c`
- staging 전체 원문: `KICK BACK` / `米津玄師`
- frozen chart 전체 원문: `KICK BACK( 'チェンソーマン' OP)` / `米津玄師`
- 실제 공식 표시셀 전체: `KICK BACK( 'チェンソーマン' OP)` / `米津玄師`

공식 전체 제목 KICK BACK( 'チェンソーマン' OP)와 가수 米津玄師가 동결차트17위 전체 원문과 정확히 일치합니다. staging의 기본 제목 KICK BACK에 동결차트가 명시한 チェンソーマン OP 부제를 대응시켜 원문 그대로 보존합니다. 오디오 동일성·추가 버전 지원을 추정하지 않습니다.

### 동결 17위 · KY `44855` · confirm

- 안정 ID: `jpop-playlist-米津玄師-kick-back-5f2ec0135c`
- staging 전체 원문: `KICK BACK` / `米津玄師`
- frozen chart 전체 원문: `KICK BACK( 'チェンソーマン' OP)` / `米津玄師`
- 실제 공식 표시셀 전체: `KICK BACK ("チェンソーマン"OP)` / `米津玄師`

공식 전체 제목 KICK BACK ("チェンソーマン"OP)와 가수 米津玄師를 직접 대조했습니다. staging 기본 제목 KICK BACK 및 동결차트17위 KICK BACK( 'チェンソーマン' OP)와 본제·チェンソーマン·OP 표기가 대응하며 차이는 괄호 전후 공백과 인용부호뿐입니다. 금영의 전체 표기를 그대로 근거에 보존하고 오디오 동일성·새 버전 지원을 추정하지 않습니다.

### 동결 18위 · TJ `52773` · confirm

- 안정 ID: `jpop-playlist-優里-レオ-4439d77563`
- staging 전체 원문: `レオ` / `優里`
- frozen chart 전체 원문: `レオ` / `優里`
- 실제 공식 표시셀 전체: `レオ` / `優里`

공식 전체 제목 レオ와 가수 優里가 staging 및 동결차트18위 원문과 정확히 일치합니다. 별도 버전·부제·공동명의 표기는 이 결과 셀에 없으며 원문을 그대로 보존합니다. 음원 동일성·반주기 모델 지원을 추정하지 않습니다.

### 동결 18위 · KY `57826` · confirm

- 안정 ID: `jpop-playlist-優里-レオ-4439d77563`
- staging 전체 원문: `レオ` / `優里`
- frozen chart 전체 원문: `レオ` / `優里`
- 실제 공식 표시셀 전체: `レオ` / `優里`

공식 전체 제목 レオ와 가수 優里가 staging 및 동결차트18위 원문과 정확히 일치합니다. 별도 버전·부제·공동명의 표기는 이 결과 셀에 없으며 원문을 그대로 보존합니다. 음원 동일성·반주기 모델 지원을 추정하지 않습니다.

TJ27031의 feature 명의는 실제 공식 셀과 기존 동결차트가 **동일한 전체 명의**를 제공한다는 범위에서 보존했다. KY43185의 `SAM`을 그 명의와 같다고 추정하지 않았다. TJ 역광의 영화 OST·TJ/KY KICK BACK의 OP 표기는 실제 부제 문자를 각각 대조했다. 금영 역광의 `..`는 실제 표시 제목 셀의 생략 표기이며 숨은 내용·완전한 제목을 재구성하지 않았다. 이 조회에는 오디오 청취·편곡 동일성·반주기 모델 지원을 판단할 근거가 없다.

## 실제 공식 요청

변경하지 않은 `scripts/provider-metadata.py` SHA-256은 `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243`다. 기존 safe CLI를 순차8회 실행했으며 urllib timeout20초·외부 프로세스 상한28초·최대2MiB/strict UTF-8·공식 numeric URL와 정확한 finalUrl·단일 일치 행 검증을 그대로 사용했다. 요청 실패·파싱 실패·후보 쓰기 실패는 실제0건이다. 편집상 보류2건을 전송 실패로 섞지 않았다.

```sh
# provider/number는 아래 표의8쌍으로 순차 실행했다.
python3 scripts/provider-metadata.py fetch tj 27031 --output /tmp/jpop-0400-provider-candidates/tj-27031.json
```

실제 첫 요청 시작은 `2026-10-15T19:06:47.951811Z`(2026-10-16T04:06:47.951811+09:00)다. 실제 helper checkedAt는 아래와 같으며 편집 판단시각 `2026-10-15T19:08:10.066408Z`(2026-10-16T04:08:10.066408+09:00)과 구분한다. 아래 링크는 각 실제 request URL 및 finalUrl과 정확히 같다.

| 순위 / 제공사 / 판단 | 공식 번호 | checkedAt (KST) | 응답 bytes / SHA-256 |
| --- | --- | --- | --- |
| 15 / TJ / confirm | [27031](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27031&strWord=Y) | `2026-10-16T04:06:49.340877+09:00` | 49,269 / `024ead7135ab7abe272b98720e90b8e17e8d6779e662c85dc4c0536596043c20` |
| 15 / KY / defer | [43185](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43185) | `2026-10-16T04:06:53.142654+09:00` | 45,519 / `abce76cc307d158f31172cb5bd4126bb451aa32086e2c0e93d50c8ed8199abee` |
| 16 / TJ / confirm | [68868](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68868&strWord=Y) | `2026-10-16T04:06:53.828995+09:00` | 49,243 / `9c1759af47529a06fa63fc1d3512fbfdf9e840a728edbd16f9b9f9b3acf5572a` |
| 16 / KY / defer | [76509](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=76509) | `2026-10-16T04:06:56.561637+09:00` | 47,910 / `13835e841cbae8659075adac54a4f97ac9c3dcb16f88383fa84afd831bf96354` |
| 17 / TJ / confirm | [68677](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68677&strWord=Y) | `2026-10-16T04:06:57.259478+09:00` | 49,263 / `ab92bf8a7123f609799f8b67d5c522208b26d9451a40aef8ab7f251d91360ce9` |
| 17 / KY / confirm | [44855](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44855) | `2026-10-16T04:06:59.892091+09:00` | 50,381 / `b8f0d678f5c516ac62caeb9f61024861f8b1ddbb0175f5a6de2465b44e5b2219` |
| 18 / TJ / confirm | [52773](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52773&strWord=Y) | `2026-10-16T04:07:00.559714+09:00` | 49,213 / `3b73929e8e281996d6cb66f4f53c1b0af91da4d6a734fc169831d9a840693362` |
| 18 / KY / confirm | [57826](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57826) | `2026-10-16T04:07:03.279190+09:00` | 45,998 / `528f6bf0d28542ee0307797860655b4d45735021b0f8c37e8030ba7b9edf725a` |

원문 HTML·주변 문맥·가사는 parser 내부 메모리에서만 처리하고 출력/저장하지 않았다. 영속 [metadata JSON](provider-review-requests-20261016-0400.json)은 실제 requestStartedAt·HTTP/status·전체 표시 제목/가수·정확한 URL/최종 URL·번호·실제 checkedAt·응답 bytes/hash·원 unreviewed 후보 및 별도8개 편집판단을 담는다. 임시 후보도 metadata JSON만 저장했다. 전체 환경·시크릿·credential 파일은 읽거나 기록하지 않았다.

각 응답 hash는 실제 fetch가 받은 전체 bytes에 대해 helper가 계산했다. 원 응답을 보존하지 않았으므로 다른 담당/다음 환경에서 이 hash만으로 같은 원문을 독립 재파싱할 수 없다. 이 한계를 숨기거나 root가 독립 재파싱했다고 쓰지 않는다. 새 요청을 하면 그 실제 시각·hash의 별도 후보이며 기존 checkedAt를 덮어쓸 근거가 되지 않는다.

## 원본 보존과 실제 검증

기존 ledger71개 전체 객체·순서·최상위 metadata·근거/이유/실제 시각은 작업 직전 `/tmp/jpop-0400-base-ledger.json`과 직접 비교했다. 마지막 배열/객체 닫기 이전까지의 bytes prefix를 유지한 채 명시적으로 확정한6개만 끝에 추가했다. old71 canonical은 키 순서가 보존된 `JSON.stringify(entries)` 표현이며 키 정렬 표현과 혼동하지 않았다. 원본 staging/chart/legacy identity/safe helper/global CSS bytes는 시작 시점과 같다.

| 입력 / 결과 | SHA-256 |
| --- | --- |
| 기존 ledger71 ordered canonical | `35487404af46c08e2bf0471e0bf05fff088c0545b69639a57d9c928390797689` |
| 기존 ledger71 bytes | `12996fc061e68be0a17924fea9fd2fa624cce8626a9e1699fb26055c2b6c22d6` |
| 결과 ledger77 bytes | `a2d8ea8e9eba737a38a1f2968a0a1687498bf04fb0f8742ea7f42eedaef8f708` |
| `data/staging/legacy-catalog.json` / 시작 bytes와 동일 | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/chart-snapshot.json` / 시작 bytes와 동일 | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| `data/legacy-identity-baseline.json` / 시작 bytes와 동일 | `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8` |
| `scripts/provider-metadata.py` / 시작 bytes와 동일 | `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243` |
| `src/styles/global.css` / 시작 bytes와 동일 | `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889` |

`2026-10-15T19:08:37.771Z`에 실제 `validateProviderCorrections`로77개 schema/revision·중복 ID/provider·관측번호·문자열/정확한 numeric URL·checkedAt/hash를 검증했다. `buildPublicCatalog`/`applyProviderCorrections`를 이전71·이후77에 각각 적용해3,317개 안정 ID와 `songHref` 전체, 비대상 public 객체 전체를 deep-equal로 비교했다. 추가6개는 원래 문자열 번호·`provider-confirmed`·`conflict:false`, 보류KY2개는 기존 `observed-unverified`/같은 관측번호/근거null임을 확인했다. public projection에 response hash·unlisted 문자열이 없음을 확인했다. 원본 unknown을 미수록으로 바꾸지 않았다.

현재 playlist3,299곡 중 전체 confirmed는 TJ30·KY26이며 충돌 번호를 제외한 안전 탐색 대상은 **TJ28·KY24·합집합28곡**이다. 동결차트100행에 속한 current/playlist의 안전 대상은 **TJ19·KY17·합집합19곡**이다. 업체별 합52개 번호는52곡이 아니며 차트 업체19+17도36곡이 아니다. 관측 TJ3,105·KY2,465, unknown TJ164·KY808이며 기존unknown 수를 보존했다. 제시한 수는 이번 실제 입력에 대한 projection 결과이며 동결 차트의 새 수집이나 전체 카탈로그 공식검토를 뜻하지 않는다.

`node --test --test-isolation=none tests/provider-corrections.test.mjs`를 직접 실행하여 기존 합성 계약 회귀 **8/8 PASS·실패0·51.745942ms**를 확인했다. 합성 fixture 검사가 실제8개 정체성 검토를 대신하지 않는다. 상세 validation과 요청/decision/append의 대응은 metadata JSON에 있다. 전체 unit/type/build·브라우저·two-target 재현·sameSHA CI·실제 preview/운영·복구·GitHub Release는 root가 실제 수행하고 수용해야 한다. 이 시점에는 미실행 단계를 통과라고 기록하지 않았다.

## 미확인 상태와 인계

공식 공개 번호 검색을 읽은 사실은 재배포 허락·가사/이미지/음원 권리·Google AdSense 계정/신청/심사 결과의 근거가 아니다. 권리·Google 실제 심사 상태는 unknown이다. SAM과 samfree의 동일성·금영 역광 표시셀의 생략된 부분·오디오 동일성·모든 반주기 모델 제공 여부는 확인하지 않았다. 근거 없는 별칭·버전·난이도·권리·확인일·승인을 만들지 않았다.

수집 실패가 없었다고 실패 보존 계약을 제거하지 않는다. 기존 수집/파싱/부분 결과/쓰기 실패에서 마지막 정상 snapshot을 유지하는 하네스는 변경하지 않았고, 실서비스 오류나 실제 광고 클릭/요청을 만들지 않았다. 새 내부 검토문서만으로 제품 개선·운영 배포가 완료됐다고 주장하지 않는다. root와 품질 담당에게 6confirm/2defer·원71 보존·전체/차트 고유 ID 수·실제 시각/metadata 경계를 전달했다.

다른 두 담당의 실제 파일·검증 근거 읽기 평가는 root의 후속 요청 후 본인 문서에 기록한다. 점수/투표·최종 운영/Release 결과를 미리 생성하지 않았다.

## 실제 동료 읽기 평가 · 04시 run

평가 기록 UTC `2026-10-15T23:12:11.405549Z`. root 요청으로 본인 문서 끝에만 기록했다. 앱/품질의 실제 소스와 두 문서·QA/대비 JSON·root 실제 검증 로그를 읽었다. 영속12PNG의 존재/bytes/hash를 확인하고 최종 대표4장과 수정 전 모바일 결함1장을 직접 열었다. 수정 전 결함·대기 누락·JSON collection 실패와 수정 후 최종 결과, dirty 로컬 artifact와 아직 실행하지 않은 clean CI/원격 배포를 구분한다. 새 조회·브라우저·테스트·Git·빌드·agent·배포는 실행하지 않았다. 두 산출물은 현재 할당된 기준에서 동점으로 수용하며 순위를 만들기 위한 감점은 하지 않는다.

```json
{
  "reviews": [
    {
      "reviewer": "/root/resume04_evidence",
      "candidate": "/root/resume04_app",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "chart.astro 전체를 읽었다. 기존100행 canonicalId/순위를 검증한 뒤 기존 confirmedNumbers 계약으로 current/playlist·공식근거·충돌없음 업체를 선별하고 업체별 Set/합집합 Set을 따로 계산한다. 100곡 분모·0≠미수록·관측 제외·금영도 TJ 원순위임을 SSR로 설명한다. 원100행/기존상세/SongRow/저장은 유지하고 chart-안정ID anchor로 연결하며 날짜/보관차트 경계를 보존한다. 실제 TJ19/KY17/전체19와 보류KY2 제외가 자료 판단에 맞는다.",
        "evidence": "최종 19:15:18–24Z dirty preview QA/대비 JSON과 영속12PNG 파일을 읽고 desktop-first/mobile-summary/mobile-anchor/short-last 4장을 직접 열었다. 3viewport의 counts/100행·JSoff키보드·브랜드/색인/오류0 기록과 화면이 일치한다. 수정 전 mobile-anchor도 열어 실제 badge/업체라벨 충돌을 확인했고 최종 이미지의 분리/한줄 표시·100행 bbox 기록에서 해소를 확인했다. root 실제314unit/type0/preview28/prod28 로그를 읽었고 초기 QA대기누락·collection오류 및 dirty 결과를 remote/clean 배포 증거와 구분한다.",
        "value": "방문 업체에서 확인된 번호가 있는 차트곡을 원래 순위 순서로 찾아 기존행/상세/공식근거로 이동하게 한다. 100곡과 업체별 수/합집합을 함께 보여 중복합산·미수록·최신차트 오해를 줄인다. 360px JSoff에서 제목/가수/번호가 읽히고 원행의 양업체 버튼/확인badge를 분리했다. 짧은 desktop 마지막81위 바로가기와 전체100곡 복귀가 접근 가능하다.",
        "maintainability": "기존 helper/Layout/SongRow·static SSR/native anchor를 재사용하며 새 클라이언트코드·의존성·공용CSS 변경이 없다. 수치는 자료에서 계산되고 stableID anchor는 재정렬에 종속되지 않는다. 모바일 보완을 해당차트 scoped CSS에 한정하고 명시적 data/aria 속성과 모든100행 bbox 회귀로 보완했다. 현재 수용 범위에 필수적인 유지보수 결함을 발견하지 않았다."
      },
      "reason": "소유 페이지/문서·실제 최종 로컬 근거를 읽어 수용한다. root의314/type0/두 target28 통과를 확인했으며 초기 결함 발견/수정/재검증을 숨기지 않았다. 수정된 과거 결함만을 이유로 감점하거나 미확인 결함을 생성하지 않는다. 본 리뷰는 새 브라우저·공식조회·Git·빌드·배포 실행이 아니다.",
      "evidence": [
        "jpoptoday/src/pages/chart.astro",
        "docs/engineering/chart-confirmed-navigation-20261016-0400.md",
        "docs/operations/runs/resume-20261016-0400-ui-qa.json",
        "docs/operations/runs/resume-20261016-0400-ui-contrast.json",
        "docs/engineering/design/chart-confirmed-20261016-0400-desktop-first.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-summary.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-anchor.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-short-last-shortcut.png",
        "docs/operations/runs/resume-20261016-0400-ui-before-fix-mobile-anchor.png",
        "/tmp/jpop-0400-unit.txt",
        "/tmp/jpop-0400-check-final.txt",
        "/tmp/jpop-0400-preview-e2e-final.txt",
        "/tmp/jpop-0400-preview-e2e-after-jsonfix.txt",
        "/tmp/jpop-0400-production-e2e.txt"
      ],
      "acceptance": "accepted_for_integration; 할당된 앱/디자인·로컬 QA 수용 기준 충족, 필수 보완 없음. clean sameSHA CI·실제 preview/운영·복구·Release는 root 후속 게이트로 pending."
    },
    {
      "reviewer": "/root/resume04_evidence",
      "candidate": "/root/resume04_quality",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "4unit/2E2E 전체를 읽었다. 원71 canonical/동결 rawbytes/3317ID/100rank와 실제6항목의 전체 feature/OST/OP 제목·가수·URL·실제시각/hash를 고정한다. slice0..77 projection에서 비대상전체객체·ID/URL·동일제목다른가수·unknown을 보존하고 불명KY2는 observed/evidence null로 확인한다. E2E는 UI helper를 import하지 않고 servedcatalog+동결chart로 100행/순위/기존URL·업체별/합집합·보류제외를 독립 대조하며 officialLookupMatches의 근거URL 계약을 재사용한다.",
        "evidence": "문서의 실제4unit PASS197.164329ms·metadata 독립대조8/8과 root314전체에 포함된4검사 결과를 읽었다. root 최종preview28/50.3s·production28/50.5s에서 새 SSR/360JSoff 검사 각각 PASS를 확인했다. 초기 ERR_IMPORT_ATTRIBUTE_MISSING collection오류를 실제 로그에서 확인했고 readFileSync/JSON.parse 수정과 list2수집을 브라우저 통과와 구분한 기록을 읽었다. 최종 QA/대표4PNG와 수정 전 실제겹침을 읽고 새100행 bbox assertion이 그 결함을 직접 겨냥함을 확인했다.",
        "value": "기존근거의 손상·버전/명의불명의 자동확정·같은제목 다른가수로의 근거전파·관측을 확인으로 계산하는 회귀를 막는다. 사용자에게 보이는 100분모/중복합/원순위·0의미와 안정anchor/상세·실제 모바일 업체라벨/번호/badge 겹침을 검증한다. 단순 overflow PASS로 실제겹침을 숨기지 않는 회귀이며 광고/색인/브랜드도 실제검사 경계 안에서 확인한다.",
        "maintainability": "별도4unit/2E2E 파일과 문서로 범위를 한정하고 공용 package/config/서버를 변경하지 않는다. old71/cohort77을 고정해 후속 append를 허용하며 E2E counts/order는 동결자료/servedcatalog 기반이다. URL자격을 별도 축소 heuristic으로 복제하지 않고 기존계약을 재사용해 이전 wwwKY/port/query정합성 한계를 피했다. JSON import문제를 의존성 추가 없이 표준fs/parse로 정정했고 실패/미실행/담당실행과 root실행 경계를 보존한다. 필수 유지보수 보완을 발견하지 않았다."
      },
      "reason": "실제 소스/자료 metadata/문서/최종 로컬 로그와 화면을 읽어 현재4unit/2E2E 계약을 수용한다. 이미 정정하고 실제 재실행한 collection오류를 숨기거나 새28실패로 세지 않았으며 테스트 수/말수로 평가하지 않았다. 새 테스트·브라우저·조회·Git·빌드·배포는 실행하지 않았다. 원응답 미보존의 독립재파싱 불가와 실제AdSense unknown도 유지한다.",
      "evidence": [
        "jpoptoday/tests/reviewed-chart-cohort0400.test.mjs",
        "jpoptoday/e2e/chart-confirmed-navigation.spec.ts",
        "docs/engineering/chart-confirmed-quality-20261016-0400.md",
        "docs/data/provider-review-requests-20261016-0400.json",
        "docs/operations/runs/resume-20261016-0400-ui-qa.json",
        "docs/operations/runs/resume-20261016-0400-ui-contrast.json",
        "docs/engineering/design/chart-confirmed-20261016-0400-desktop-first.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-summary.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-mobile-anchor.png",
        "docs/engineering/design/chart-confirmed-20261016-0400-short-last-shortcut.png",
        "docs/operations/runs/resume-20261016-0400-ui-before-fix-mobile-anchor.png",
        "/tmp/jpop-0400-unit.txt",
        "/tmp/jpop-0400-check-final.txt",
        "/tmp/jpop-0400-preview-e2e-final.txt",
        "/tmp/jpop-0400-preview-e2e-after-jsonfix.txt",
        "/tmp/jpop-0400-production-e2e.txt"
      ],
      "acceptance": "accepted_for_integration; 할당된 회귀·독립집계/모바일 검사와 최종로컬통과 수용, 필수 보완 없음. clean sameSHA CI·실제 preview/운영·복구·Release는 root 후속 게이트로 pending."
    }
  ]
}
```
