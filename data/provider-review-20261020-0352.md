# 두 업체 확인곡 탐색 · 실제 공식 번호 검토

`DATA-001` · `CONTENT-001` · `SEARCH-001` · [Issue44](https://github.com/jpop-today/jpoptoday/issues/44) · [제품 계약](../product/spec.md) · [ADR-006](../adr/006-reviewed-corrections-and-production.md).

동결차트 27·29·30·31위와 연결된 current/playlist 4 ID의 기존 TJ·금영 관측번호 8개를 metadata-only로 실제 조회하고, 제목·가수·부제 전체를 읽어 별도의 편집 판단을 남겼다. 기존 ledger 84개는 객체·순서·내용 그대로 보존하고 confirm 8개만 끝에 추가했다. 가사 원문·이미지·음원·원응답 HTML은 저장하지 않았다. 자료 대조는 권리 허가·전체 카탈로그 검증·새 차트 수집·Google 승인 결과와 별개다.

| 작업 계약 | 실제 범위 |
| --- | --- |
| 실행 / 담당 | `manual-20261020-0352` / `/root/run44_evidence`; root가 Git·공용 파일·CI·통합·배포 소유 |
| 브랜치 / 기준 | `codex/issue44-provider-both`; source `49d6806bb00e813bb3dcda15a22f2cf8d2d21da8`; docs `4ddfa9d20b2d8aee5c538d25388c499a83c8675f` |
| 변경 소유 | source `data/reviewed/provider-corrections.json`, `src/content/guides.ts` 첫 `find-japanese-songs`만; 이 문서와 같은 접두사의 metadata 후보·요청/검증 JSON |
| 수용 기준 | 공식 exact numeric HTTPS 단일행 metadata + 실제 checkedAt/hash; 제목·가수·버전 판단; 기존84·raw·ID/URL·다른5가이드 보존; 첫 가이드 AND/조건해제/0곡/JS·정적대안 |
| 미실행 | full unit/type/build/artifact/browser/CI/preview/production/recovery/Release는 root의 통합 검증 대상; 이 문서는 완료를 주장하지 않음 |

## 실제 요청과 편집 판단의 분리

요청 시작 `2026-10-19T18:55:45.289430+00:00` / 마지막 완료 `2026-10-19T18:56:01.555030+00:00`. KST 실제 공식 checkedAt는 아래와 같다. 편집 판단 시각은 `2026-10-19T18:56:38.387138Z` (`2026-10-20T03:56:38.387138+09:00`). 원후보의 `verification: unreviewed`는 수정하지 않았고 [요청·별도 편집 판단 JSON](provider-review-20261020-0352-requests.json)에 연결했다.

기존 `scripts/provider-metadata.py` (SHA-256 `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243`)의 fetch_metadata(provider, number, timeout=20)를 정확히 8회 순차 호출했다. inherited proxy/CA와 TLS 검증, 2MiB 제한·strict UTF-8·공식 exact numeric URL/redirect 제한·단일 결과행 조건을 유지했다. 모두 HTTP200/번호일치/파싱 성공이며, confirm8/suppress0/defer0다. 추가 재조회는 하지 않았다.

수집된 제목·가수·번호·URL·checkedAt·응답 SHA-256/bytes/status만 남긴다. 전체 응답은 metadata helper의 메모리에서만 처리했으므로 hash만으로 원문 HTML을 재파싱했다고 주장할 수 없다. 다른 담당자가 검토할 때도 metadata와 실제 편집 판단의 경계를 유지한다.

| 차트 / 업체 | 실제 번호와 공식 URL | checkedAt (KST) | 응답 bytes / SHA-256 |
| --- | --- | --- | --- |
| 27 / TJ | [27984](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27984&strWord=Y) | `2026-10-20T03:55:46.875855+09:00` | 49,255 / `c12859c173e9f0d2877549755c307bab0789c63f05fca00edd7f565b1c8cdfb8` |
| 27 / KY | [44091](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44091) | `2026-10-20T03:55:50.513829+09:00` | 46,535 / `4aa57c859d7b8f4dfc07fe2ac15d0ab9a621a319ddca6038e74d82746206fddb` |
| 29 / TJ | [68461](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68461&strWord=Y) | `2026-10-20T03:55:51.860999+09:00` | 49,283 / `3b131a20449211a8c308f4bb805900f9e95f5a3d564dcd64deace1477a88a160` |
| 29 / KY | [75996](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=75996) | `2026-10-20T03:55:54.727123+09:00` | 56,144 / `173207a515933cbec1d668cf802004866e78b6144cb6020b47a7fcf62b75a031` |
| 30 / TJ | [27805](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27805&strWord=Y) | `2026-10-20T03:55:55.401885+09:00` | 49,251 / `b9c5e42c18461c14ade1d619a94c9aff5d80027f659dc05cb65046f9bd863e45` |
| 30 / KY | [43932](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43932) | `2026-10-20T03:55:58.398530+09:00` | 48,062 / `03397b55b424ef2c7ec7f3785b3885fc58519c687356d418a35ae624d95e2e31` |
| 31 / TJ | [6773](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=6773&strWord=Y) | `2026-10-20T03:55:59.099286+09:00` | 49,220 / `803c1b21543c0064ab5cfb920858caccb4865131c5d0f48f278b1a21f8f49c24` |
| 31 / KY | [40993](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=40993) | `2026-10-20T03:56:01.553677+09:00` | 37,072 / `55ef88f620c9ccf38bf2ac634175f21813a6327ade5ab33d753b6b992e32bb82` |

## 전체 표시셀과 행별 검토

동결 입력은 staging revision `6dbe5a65d1bd729c09393f746f715f4363089a43`, 차트 fetchedAt `2026-10-13T10:26:45.525Z`, 요청기간 `2026-09-29`–`2026-10-13`, 차트 legacy revision `94d615f5e355f4b00ebbb824547ac6dfa034ad18`이다. 실제 번호 조회일을 이 차트의 수집일이나 새 순위로 바꾸지 않는다. 아래 검토는 source metadata 대응에 한하며 음원·편곡·현장 기기의 동일성이나 수록을 확정하지 않는다.

### 동결 27위 · TJ 27984 · confirm

- stable ID: `jpop-playlist-orangestar-feat-ia-アスノヨソ-ラ哨戒班-45d82ae042`
- staging 전체 원문: `アスノヨゾラ哨戒班` / `Orangestar feat.IA`
- 동결차트 전체 원문: `アスノヨゾラ哨戒班` / `Orangestar(Feat.IA)`
- 실제 공식 표시셀 전체: `アスノヨゾラ哨戒班` / `Orangestar(Feat.IA)`
- 원후보: [provider-review-20261020-0352-tj-27984-candidate.json](provider-review-20261020-0352-tj-27984-candidate.json)

TJ 공식 제목 アスノヨゾラ哨戒班은 staging 본제와 같고 가수 Orangestar(Feat.IA)는 동일 canonicalId의 동결차트 전체 명의와 정확히 일치합니다. staging Orangestar feat.IA와는 Feat 대소문자·괄호·공백 차이이며 전체 명의 IA를 보존합니다. 음원·편곡 또는 기기 모델의 동일성은 판단하지 않습니다.

### 동결 27위 · KY 44091 · confirm

- stable ID: `jpop-playlist-orangestar-feat-ia-アスノヨソ-ラ哨戒班-45d82ae042`
- staging 전체 원문: `アスノヨゾラ哨戒班` / `Orangestar feat.IA`
- 동결차트 전체 원문: `アスノヨゾラ哨戒班` / `Orangestar(Feat.IA)`
- 실제 공식 표시셀 전체: `アスノヨゾラ哨戒班` / `Orangestar feat.IA`
- 원후보: [provider-review-20261020-0352-ky-44091-candidate.json](provider-review-20261020-0352-ky-44091-candidate.json)

금영 공식 제목 アスノヨゾラ哨戒班과 가수 Orangestar feat.IA는 staging 원문과 정확히 일치합니다. 동결차트의 Orangestar(Feat.IA)와는 feat 대소문자·괄호·공백 차이를 검토했습니다. 전체 명의를 보존하며 음원·편곡 또는 현장 수록 동일성을 추정하지 않습니다.

### 동결 29위 · TJ 68461 · confirm

- stable ID: `jpop-playlist-mrs-green-apple-ライラック-845233e093`
- staging 전체 원문: `ライラック` / `Mrs. GREEN APPLE`
- 동결차트 전체 원문: `ライラック(アニメ '忘却バッテリー' OST)` / `Mrs. GREEN APPLE`
- 실제 공식 표시셀 전체: `ライラック(アニメ '忘却バッテリー' OST)` / `Mrs. GREEN APPLE`
- 원후보: [provider-review-20261020-0352-tj-68461-candidate.json](provider-review-20261020-0352-tj-68461-candidate.json)

TJ 공식 전체 제목 ライラック(アニメ '忘却バッテリー' OST)와 가수 Mrs. GREEN APPLE은 동일 canonicalId의 동결차트 전체 표기와 정확히 일치합니다. staging 본제 ライラック·가수와 대응하며 공식 작품 부제를 그대로 보존합니다. 오디오·반주기 버전의 동일성은 판단하지 않습니다.

### 동결 29위 · KY 75996 · confirm

- stable ID: `jpop-playlist-mrs-green-apple-ライラック-845233e093`
- staging 전체 원문: `ライラック` / `Mrs. GREEN APPLE`
- 동결차트 전체 원문: `ライラック(アニメ '忘却バッテリー' OST)` / `Mrs. GREEN APPLE`
- 실제 공식 표시셀 전체: `ライラック ("忘却バッテリー"OP)` / `Mrs. GREEN APPLE`
- 원후보: [provider-review-20261020-0352-ky-75996-candidate.json](provider-review-20261020-0352-ky-75996-candidate.json)

금영 공식 본제 ライラック과 가수 Mrs. GREEN APPLE은 staging과 일치하며 작품명 忘却バッテリー는 동일 canonicalId의 동결차트와 대응합니다. 공식 부제의 OP와 TJ 차트의 OST 표기는 작품에서의 사용 표기 차이로 읽고 금영 전체 셀을 그대로 보존합니다. 이를 같은 녹음·편곡 또는 모든 기기 수록의 근거로 확대하지 않습니다.

### 동결 30위 · TJ 27805 · confirm

- stable ID: `jpop-playlist-back-number-高嶺の花子さん-2433a62d20`
- staging 전체 원문: `高嶺の花子さん` / `back number`
- 동결차트 전체 원문: `高嶺の花子さん` / `back number`
- 실제 공식 표시셀 전체: `高嶺の花子さん` / `back number`
- 원후보: [provider-review-20261020-0352-tj-27805-candidate.json](provider-review-20261020-0352-tj-27805-candidate.json)

TJ 공식 전체 제목 高嶺の花子さん과 가수 back number가 staging 및 동일 canonicalId의 동결차트 원문과 정확히 일치합니다. 이 번호의 제목·가수 대조만 확인하며 음원·편곡 또는 현장 기기 지원을 추정하지 않습니다.

### 동결 30위 · KY 43932 · confirm

- stable ID: `jpop-playlist-back-number-高嶺の花子さん-2433a62d20`
- staging 전체 원문: `高嶺の花子さん` / `back number`
- 동결차트 전체 원문: `高嶺の花子さん` / `back number`
- 실제 공식 표시셀 전체: `高嶺の花子さん` / `back number`
- 원후보: [provider-review-20261020-0352-ky-43932-candidate.json](provider-review-20261020-0352-ky-43932-candidate.json)

금영 공식 전체 제목 高嶺の花子さん과 가수 back number가 staging 및 동일 canonicalId의 동결차트 원문과 정확히 일치합니다. 업체별 번호와 근거를 보존하며 음원·편곡 또는 현장 기기 지원을 추정하지 않습니다.

### 동결 31위 · TJ 6773 · confirm

- stable ID: `jpop-playlist-x-japan-endless-rain-a5ac0ff8b8`
- staging 전체 원문: `ENDLESS RAIN` / `X JAPAN`
- 동결차트 전체 원문: `Endless Rain` / `X-JAPAN`
- 실제 공식 표시셀 전체: `Endless Rain` / `X-JAPAN`
- 원후보: [provider-review-20261020-0352-tj-6773-candidate.json](provider-review-20261020-0352-tj-6773-candidate.json)

TJ 공식 전체 제목 Endless Rain과 가수 X-JAPAN은 동일 canonicalId의 동결차트 전체 표기와 정확히 일치합니다. staging ENDLESS RAIN / X JAPAN과는 제목 대소문자 및 가수 하이픈·공백 차이를 검토했습니다. 별도 명의 병합·오디오 동일성은 추정하지 않습니다.

### 동결 31위 · KY 40993 · confirm

- stable ID: `jpop-playlist-x-japan-endless-rain-a5ac0ff8b8`
- staging 전체 원문: `ENDLESS RAIN` / `X JAPAN`
- 동결차트 전체 원문: `Endless Rain` / `X-JAPAN`
- 실제 공식 표시셀 전체: `Endless Rain ("X2","乙姫CONNECTION"OST,映画"ジパング")` / `X JAPAN`
- 원후보: [provider-review-20261020-0352-ky-40993-candidate.json](provider-review-20261020-0352-ky-40993-candidate.json)

금영 공식 본제 Endless Rain은 staging ENDLESS RAIN과 대소문자 차이이고 가수 X JAPAN은 staging과 정확히 일치합니다. 공식 셀의 X2·乙姫CONNECTION·영화 ジパング 작품 부제를 생략 없이 보존합니다. 동결차트 X-JAPAN과의 하이픈·공백 차이를 검토했으며 부제 없는 원본에 새 버전·녹음 동일성을 만들어 넣지 않습니다.

## 사용 가이드 개선

`find-japanese-songs`의 기존 URL·제목을 유지하면서 본문 확인일을 실제 KST `2026-10-20`으로 갱신했다. 검색 시작→두 업체 공식 확인의 AND 선택→한쪽 해제/전체로 번호 조건만 해제→0곡의 의미→원문·업체·버전 대조→JavaScript 필요/정적 `/artists/` 대안 순서로 설명했다. 두 조건 링크는 `/search/?number=confirmed-tj&number=confirmed-ky`다. 번호 조회일은 곡 상세에서 별도로 읽도록 명시했으며 이 편집일로 다른 5개 가이드의 날짜를 바꾸지 않았다. 공개 본문에 운영 상태·worker·심사 결과를 넣지 않았다.

가이드 링크와 `/artists/`의 실제 새 교집합 표시·모바일/키보드/JS-off 동작은 앱 담당 구현과 root 브라우저 검증에 의존한다. 이 담당자는 렌더 화면을 확인하지 않았으며 단위 계약 통과로 브라우저 통과를 대신하지 않는다.

## 실행한 독립 검증과 실패 기록

[자료 보존·적용 검증 JSON](provider-review-20261020-0352-validation.json), 실행시각 `2026-10-19T18:58:02.681Z`:

- ledger92 shape/provenance/date 검증 통과; baseline 첫84 객체·순서·내용/상위 metadata 동일, confirm71→79·suppress13 유지.
- staging3317 ID와 모든 상세 URL 동일, 기존 메타데이터 동일, 이번8개를 제외한 업체셀6626개 전체 동일; 새8개 모두 string·nonconflict·strict confirmed.
- 실제 metadata8 파일과 요청 JSON/ledger evidence의 URL·checkedAt·제목·가수·hash 값 전체 동일; 후보는 unreviewed 그대로.
- `guides.ts`를 평가해 다른5개 가이드 전체 객체(본문/날짜/출처/링크 포함)가 baseline과 deep-equal임을 확인. 첫글 slug 유지, AND/정적대안 링크 확인.
- staging·chart·identity baseline·global CSS의 전체 byte SHA-256이 시작 동결값과 일치. IDs를 합치거나 raw를 갱신하지 않음.
- 기존 `hasConfirmedNumber`로 안전 ID32→36, TJ32→36, KY27→31, 두업체 교집합27→31. 실제 AND query predicate 결과도31; 이는 전체 카탈로그·미검토 번호의 공식 확인을 뜻하지 않음.
- `node --test --test-isolation=none tests/provider-corrections.test.mjs`: 실제 leaf8/pass8/fail0. 정정·raw/ID·unknown·미래날짜/공식URL·private field·동시 충돌을 다루는 기존 계약 검사이며 신규 테스트 파일은 추가하지 않음.

처음 custom Node 검증은 `execFileSync(git show)`가 EPERM으로 종료돼 assertion을 수행하지 못했다. 이어 나온 기본 Node test-runner의 파일단위 pass1을 leaf8 통과로 채택하지 않았다. readonly shell `git show`로 baseline을 `/tmp/run44-evidence-base-{ledger.json,guides.js}`에 저장하고, Node는 파일만 읽도록 바꿔 같은 비교를 통과시켰다. Node test runner는 `--test-isolation=none`으로 현재 프로세스 안에서 기존8개를 실제 실행해 위 결과를 얻었다. 권한·프록시·TLS 설정을 우회하거나 원격 원문을 로그에 남기지 않았다.

| 산출물 | SHA-256 |
| --- | --- |
| `data/reviewed/provider-corrections.json` | `a0d23d04cc705d444325f1ea8662cc00044fcba3f86074c66800569840a6eae9` |
| `src/content/guides.ts` | `7478cbdcbb8286597000cb0bbe3d4ea4703a3b4e316e7bf2c34d863c6cfc1e61` |
| `data/provider-review-20261020-0352-requests.json` | `cd770ff5bd73ae9fbdfb2eefdd141e3036bbd30878d48c065ad0600b5bd101d1` |
| `data/provider-review-20261020-0352-validation.json` | `30d3b15b589a3f5433a84e16091e5b74a3b27f0998a26d8957dc20e1cc3418c5` |

## 인계

담당 범위 구현·metadata/편집검토·독립 자료 보존검사는 완료했다. 아직 결과 commit SHA는 없으며 root가 통합할 변경이다. peer review는 root 요청 뒤 읽기 전용으로 별도 수행한다. full type/build/artifact/browser와 동일 SHA CI·실제 preview·rollback/restore·production·Release·Discord 결과는 root의 실행 기록을 따른다. Google 계정/약관·권리·실제 승인 결과는 이 작업으로 확인하지 않았다.
