# Issue47 · 차트의 두 업체 확인곡 탐색을 위한 실제 번호 대조

- 실행: `manual-20261020-1325`, 자료·콘텐츠 담당 `/root/run47_evidence`.
- Issue: [소스 #47](https://github.com/jpop-today/jpoptoday/issues/47), 상위 [#6](https://github.com/jpop-today/jpoptoday/issues/6). 기준: DATA-001·CONTENT-001·SONG-001·SEO-001·DESIGN-001, [스펙](../product/spec.md)과 [ADR-006](../adr/006-reviewed-corrections-and-production.md).
- 기준 source SHA `4d4c021e668d49c39057e76558c21b30735b8d98`, docs SHA `1291c45fded5f9c22ec28b5bf78fa62d37b1d751`. root가 생성한 `codex/issue47-chart-both`의 공유 작업 사본에서 편집했다. 최종 커밋·CI·preview·production SHA는 root의 통합 기록으로 확인한다.
- 독점 범위: 소스 `data/reviewed/provider-corrections.json`, `src/content/guides.ts`의 `read-tj-chart` 객체만, 문서 `data/provider-review-20261020-1325*` 새 파일. Git 변경·package/lockfile·CI·배포·서버 조작·추가 agent 생성은 하지 않았다.

## 실제 조회와 별도 편집 판단

기존 `scripts/provider-metadata.py`의 `fetch_metadata(provider, number, timeout=20)`와 atomic `write_candidate`로 공식 numeric 검색을 7회 순차 조회했다. helper SHA256은 `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243`다. 상속 proxy·CA와 TLS 검증을 유지했다. 응답 본문은 메모리에서 번호·제목·가수만 추출하고 원문 HTML·가사·미디어는 저장하지 않았다. 시크릿이나 전체 환경을 출력하지 않았다.

7회 모두 HTTP200과 요청/최종 공식 URL 일치, 단일 번호 일치 검증을 통과했다. candidate 7개는 `verification: unreviewed` 그대로다. 추출 성공과 곡 연결 판단은 구분했으며, 전체 제목·가수(부제 생략 없이)를 staging 및 같은 canonicalId의 동결 차트와 대조한 뒤 별도의 `editorialReview`에 7건 confirm을 남겼다. 실제 조회 시각은 한국 시간 2026-10-20 13:27:27~13:27:39, 별도 편집 판단은 `2026-10-20T04:28:59.474980Z`다. 조회 실패 0건, 조회한 7건의 판단 보류·suppress 0건이다.

| 원TJ순위 | 전체 제목 / 전체 가수 | 업체·문자열 번호 | 실제 확인 UTC | 응답 바이트 | 응답 SHA256 |
| --- | --- | --- | --- | ---: | --- |
| 32 | 夜に駆ける / YOASOBI | [TJ 68381](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68381&strWord=Y) | 2026-10-20T04:27:27.495315Z | 49221 | `c99505193a90a987dfb94d68e65e029f5c883aa17c91255358e325da5215e9b7` |
| 32 | 夜に駆ける / YOASOBI | [금영 44656](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44656) | 2026-10-20T04:27:30.847513Z | 54281 | `ad1910655fdda3109a8ecb0bd749aec4f234a6e48183a8c3a1f77378ad1e8b41` |
| 33 | マリーゴールド / あいみょん | [TJ 28927](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=28927&strWord=Y) | 2026-10-20T04:27:31.646254Z | 49210 | `6bd310807c19bfda1bdaa8ed274cb36baff38a4ddc082e020b55de7696234ca9` |
| 33 | マリーゴールド / あいみょん | [금영 44314](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44314) | 2026-10-20T04:27:34.455094Z | 47735 | `9cac98004920d622f6b169fe403aca19c185e23973bd12d293e9eefe545d5a6b` |
| 34 | 青い珊瑚礁 / 松田聖子 | [TJ 27121](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27121&strWord=Y) | 2026-10-20T04:27:35.230268Z | 49243 | `7e525e011184817c54b51e17ccd0dd79e9ef58dbacf0c6e951b15a67465cc6cb` |
| 34 | 青い珊瑚礁 / 松田聖子 | [금영 40403](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=40403) | 2026-10-20T04:27:38.365949Z | 39724 | `35e0bd196b3df621d9c4c08eb89d07d10758477a1d4055cd5b123d476d12eceb` |
| 35 | プロポーズ / なとり | [TJ 52402](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52402&strWord=Y) | 2026-10-20T04:27:39.732463Z | 49231 | `1b350067961642ab834c6f9b99e3afd41183602be741803d3df07f6bb44309c5` |

각 행의 staging/동결 차트/공식 전체 표기는 위 표의 제목·가수와 정확히 같다. 구두점 제거·전사·명의 추정이나 부제 생략으로 맞춘 항목은 없다. 연결한 canonicalId는 아래와 같다.

| 원TJ순위 | canonicalId | 업체별 결정 |
| --- | --- | --- |
| 32 | `jpop-playlist-yoasobi-夜に駆ける-677d131f3f` | TJ 68381 confirm; 금영 44656 confirm |
| 33 | `jpop-playlist-あいみょん-マリ-コ-ルト-ebdb512207` | TJ 28927 confirm; 금영 44314 confirm |
| 34 | `jpop-playlist-松田聖子-青い珊瑚礁-f8e4a0a705` | TJ 27121 confirm; 금영 40403 confirm |
| 35 | `jpop-playlist-なとり-フ-ロホ-ス-5b000e662c` | TJ 52402 confirm; 금영 null · 조회하지 않음 · unknown 보존 |

프로포즈의 금영 관측값은 처음부터 null이다. 검색할 번호를 만들거나 TJ 번호를 대신 조회하지 않았고, 미수록·권리·명의·음원·편곡·모든 현장 기기 지원을 추론하지 않았다. 7개 confirm은 해당 번호의 제목·가수 메타데이터 대조에만 한정한다. Google 심사·계정·자료 사용 권리 결과는 이 확인의 대상이 아니다.

## 사용자에게 달라진 점

- 검토 ledger는 92→99건(기존 79 confirm/13 suppress → 86 confirm/13 suppress)이다. 원래 92개 전체 객체와 순서·상위 metadata를 보존하고 끝에 실제 근거 7개만 추가했다.
- 기존 trusted `hasConfirmedNumber`로 센 current/playlist 전체는 TJ 36→40, 금영 31→34, 합집합 36→40, 교집합 31→34이다. 공식 확인 번호 개수는 67→74이며, 곡 수와 다르다. 같은 차트 100행 범위는 TJ 27→31, 금영 24→27, 합집합 27→31, 교집합 24→27이다. 전체 카탈로그 교집합 34와 차트 교집합 27을 혼동하지 않는다. 이 수치는 독립 UI 오라클로 주장하지 않으며 품질 담당의 독립 계산과 대조한다.
- 기존 `read-tj-chart`만 실제 편집일 2026-10-20으로 갱신했다. 두 업체 목록을 JavaScript 없이 펼치기 → 원래 TJ 순위 행 → 기존 곡 상세/공식 근거·각 실제 확인일 읽기 동선을 안내하고 `/chart/#chart-both-heading`에 연결했다. 현재 `chart.astro`의 실제 같은 anchor 존재를 읽어 확인했다.
- 세 쌍의 실제 대조 사례와 TJ만 확인한 なとり 사례를 구분했다. TJ 순위는 금영 순위가 아니며, 목록 제외/0곡/금영 unknown은 공식 미수록을 뜻하지 않는다. 공개 글에 변하는 총 확인곡 수를 고정하지 않았다.
- 동결 차트는 요청 기간 2026-09-29~2026-10-13, 저장 시각 `2026-10-13T10:26:45.525Z` 그대로다. 이번 번호 조회일·본문 편집일을 새 차트 수집일이나 응답의 실제 집계 기간 확인으로 표현하지 않았다.

## 실제 보존·계약 검사

첫 검사 `2026-10-20T04:29:38.275Z`, 가이드 링크 추가 후 최종 검사 `2026-10-20T04:31:58.500Z`에 통과했다. `validateProviderCorrections`와 `applyProviderCorrections`로 전체 ledger를 검증하고, base SHA에서 읽은 원본 ledger/가이드와 실제 작업 사본을 deepEqual 비교했다. 최종 명령은 `node /tmp/run47-evidence-final-check.mjs`이며 임시 경로 자체를 영속 파일로 가정하지 않는다. 결과와 입력 hash는 [검증 JSON](provider-review-20261020-1325-validation.json)에 보존한다. 품질 담당의 저장소 회귀검사와 root의 전체 검증은 별도다.

| 항목 | 실제 결과 |
| --- | --- |
| 기존 ledger92 전체/순서/상위 metadata | deepEqual 통과 |
| 다른 5개 가이드 전체 객체·본문·날짜·링크 | deepEqual 통과 |
| 기존 차트 가이드 앞 2개 절 | deepEqual 통과 |
| ID/기존 곡 URL/순서 | 3317개 전체 동일 |
| 곡의 업체 외 metadata | 3317개 전체 동일 |
| 수정 대상 외 업체 셀 | 6627개 전체 동일 |
| candidate/요청/ledger/공개 가이드 출처 | 7건 실제 URL·전체 제목/가수·번호 string·checkedAt·응답 hash 일치 |
| なとり KY | unknown, observedNumber/evidence/correction null 그대로 |
| 가이드→차트 링크 | 실제 source anchor 존재, 본문 링크 일치 |

| 동결 입력 | 원본과 같은 SHA256 |
| --- | --- |
| `data/staging/legacy-catalog.json` | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/chart-snapshot.json` | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| `data/legacy-identity-baseline.json` | `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8` |
| `src/styles/global.css` | `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889` |

최종 소스 파일 SHA256:

- ledger: `d4339885092fd050c4f92b5993b7d9a1538e05ac4f0aa06128203d758f348bf3`
- guides.ts: `0d73f37804dec248e5d4d06ebdddde55990c0fd5e20049ffbb3d3e40c1fbc116`

localStorage 사용 코드나 저장 데이터·호환 어댑터는 수정하지 않았다. 위 검사는 실제 ID·URL·provider 셀 보존의 근거이며 브라우저에서 storage 동작을 실행했다는 주장이 아니다. CSS 전체 바이트가 같으므로 원래 blue `#4f7cff`→pink `#ff7aa8` 및 pink/mint/blue palette를 바꾸지 않았다.

## 근거 파일과 남은 검증

- [실제 7회 요청·별도 7개 편집 판단·조회하지 않은 KY null](provider-review-20261020-1325-requests.json)
- [실제 계약/보존 검사와 최종 파일 hash](provider-review-20261020-1325-validation.json)
- candidate 7개:
  - [provider-review-20261020-1325-tj-68381-candidate.json](provider-review-20261020-1325-tj-68381-candidate.json)
  - [provider-review-20261020-1325-ky-44656-candidate.json](provider-review-20261020-1325-ky-44656-candidate.json)
  - [provider-review-20261020-1325-tj-28927-candidate.json](provider-review-20261020-1325-tj-28927-candidate.json)
  - [provider-review-20261020-1325-ky-44314-candidate.json](provider-review-20261020-1325-ky-44314-candidate.json)
  - [provider-review-20261020-1325-tj-27121-candidate.json](provider-review-20261020-1325-tj-27121-candidate.json)
  - [provider-review-20261020-1325-ky-40403-candidate.json](provider-review-20261020-1325-ky-40403-candidate.json)
  - [provider-review-20261020-1325-tj-52402-candidate.json](provider-review-20261020-1325-tj-52402-candidate.json)

이 역할은 전체 unit/type/build/artifact/E2E·브라우저·CI·preview·실제 복구·production 배포를 실행하지 않았다. 같은 SHA의 정책/복구/운영 게이트 및 실제 사이트 검증은 root가 통합 실행한다. 이 문서만으로 운영 반영·AdSense 제출/승인을 주장하지 않는다. 상호 평가는 root가 다른 두 담당의 실제 산출물을 전달한 후 별도로 읽고 기록한다.
