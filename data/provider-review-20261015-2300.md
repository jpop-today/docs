# 동결 차트 11–14위의 공식 번호 대조 · 2026-10-15 23시

`DATA-001` · `CONTENT-001` · `SONG-001` · `SEO-001` · `ADS-001` · [Issue24](https://github.com/jpop-today/jpoptoday/issues/24) · [상위 Issue6](https://github.com/jpop-today/jpoptoday/issues/6) · [제품 스펙](../product/spec.md) · [ADR-006](../adr/006-reviewed-corrections-and-production.md).

## 계약과 범위

| 항목 | 이번 실행 |
| --- | --- |
| run / 담당 / 통합 | `resume-20261015-2300` / `/root/resume23_evidence` / root 단독 통합 |
| source base / branch | `dffa93bc6982c9f186a269da577e3dbc108ad189` / `codex/24-provider-counts-20261015` |
| docs base | `daa79c0828ad6906461f88a6dc5edef36842fd73` |
| 조직 규칙 base | `b7aad18fe2f793453b6505bb150f359d05ae3554` |
| 단독 편집 | source `data/reviewed/provider-corrections.json`, docs의 이 문서와 `data/provider-review-requests-20261015-2300.json` |
| 후보 | 기존 동결 차트 rank11–14의 canonical current/playlist 4ID, staging의 TJ4·KY4 관측 문자열 번호 |
| 실제 조회 | 기존 metadata-only CLI를 변경 없이 순차 8회 실행. 요청/최종 URL·HTTP200·단일 정확번호·전체 제목/가수 셀·실제 시각/hash를 기록 |
| 결과 | 8개 후보의 전체 제목·가수·버전을 개별 대조한 뒤 confirm8만 append. 기존63항목·순서·metadata·닫기 직전 bytes prefix 불변, 총71항목(confirm58/suppress13) |
| 수용 경계 | 자료 검토·계약/projection 검증 완료. 앱 전체 검사·CI·브라우저·게시/배포·복구·Release는 root의 별도 실제 결과로 판단 |

시작에 세 저장소 AGENTS와 spec/status/roadmap/daily-development/continuous-development 및 ADR-006을 읽고 source/docs `git status --short`를 확인했다. root의 기존 운영 문서 미커밋 변경은 보존했다. 이 담당은 Git·공용 package/lockfile·CI·배포·서버를 조작하지 않았다. 추가 하위 에이전트를 생성하지 않았다. root가 전달한 현재 managed 환경 revision228/current/enforced·API 읽기 성공과 이 담당의 실제 공식 GET 성공을 구분한다. 이 담당의 네트워크 요청은 추가 network capability 안에서 기존 proxy·CA/TLS 경로를 그대로 사용했다.

차트의 기준은 `2026-10-13T10:26:45.525Z`에 수집한 기존 스냅샷이다. 요청 기간은 `2026-09-29–2026-10-13`, legacy revision은 `94d615f5e355f4b00ebbb824547ac6dfa034ad18`이며 이를 새 수집·새 순위로 갱신하지 않았다. 이번 확인은 4ID의 8번호 범위이며 전체3,299곡 검토나 가수의 전체 수록 목록을 뜻하지 않는다.

## 개별 정체성 판단

차트 canonicalId를 staging의 안정 ID와 직접 연결한 뒤 `availability: current`, `origin: playlist` 및 해당 제공사의 기존 관측 번호를 확인했다. 기존 ledger에는 이 8개 ID/provider 조합이 없었다. 번호만 같다는 이유로 대응을 판정하지 않았으며 실제 결과의 **전체** 제목과 가수를 읽었다.

| 동결 순위 / 안정 ID | 원본과 실제 공식 결과의 전체 제목 / 가수 | TJ / KY | 편집 대조 |
| --- | --- | --- | --- |
| 11 / `jpop-playlist-novelbright-walking-with-you-6d7d7f64d1` | Walking with you / Novelbright | `68419` / `44686` | 두 제공사 모두 제목 대소문자·공백 및 가수 전체 문자열이 원본과 정확히 일치. 별도 버전·부제·공동명의 표기 없음 |
| 12 / `jpop-playlist-優里-ト-ライフラワ-1125dddd02` | ドライフラワー / 優里 | `68333` / `44652` | 두 제공사 모두 제목·가수 전체 문자열이 원본과 정확히 일치. 별도 버전·부제·공동명의 표기 없음 |
| 13 / `jpop-playlist-back-number-水平線-187ab55662` | 水平線 / back number | `68446` / `44701` | 두 제공사 모두 제목·가수의 영문 소문자·공백까지 원본과 정확히 일치. 별도 버전·부제·공동명의 표기 없음 |
| 14 / `jpop-playlist-vaundy-踊り子-fe694e97f6` | 踊り子 / Vaundy | `68807` / `76508` | 두 제공사 모두 제목·가수 전체 문자열이 원본과 정확히 일치. 별도 버전·부제·공동명의 표기 없음 |

위 표는 표시된 메타데이터를 대조한 판단이다. 오디오를 듣거나 모든 반주기·모델의 제공 범위, 음원 동일성·권리 허락을 확인한 것은 아니다. 가수 별칭·공동명의·ID를 병합하지 않았다. fetched 후보의 `verification: unreviewed` 원본은 보존했고 별도 reviewer·reviewedAt·판단 이유를 담은 `decisions`에서 해당8행만 확정한다. 추출 도구가 자동으로 confirm한 것으로 기록하지 않는다.

## 실제 요청과 근거

기존 `scripts/provider-metadata.py` SHA-256은 `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243`다. 각 CLI는 urllib timeout20초·외부 프로세스 상한28초를 적용했다. 2MiB/strict UTF-8·정확한 공식 numeric URL/최종 URL·단일 일치 행 검사는 도구 그대로 사용했다. 모든 요청은 성공해 safe failure 0건이다. 원문 응답은 parser 내부 메모리에서만 처리했고, raw HTML·주변 문맥·가사·이미지·전체 환경·시크릿을 출력하거나 저장하지 않았다. 저장한 후보와 영속 기록은 번호·제목·가수 및 provenance metadata만 포함한다.

```sh
# provider/number는 아래 표의8쌍으로 순차 실행했다.
python3 scripts/provider-metadata.py fetch tj 68419 --output /tmp/jpop-2300-provider-candidates/tj-68419.json
```

요청 시작은 `2026-10-15T14:03:21.493633Z`(23:03:21 KST)이며 실제 checkedAt 범위는 23:03:23–23:03:39 KST다. 아래 시각은 실제 helper의 UTC checkedAt에서 시간대만 변환한 값이다. 편집 대조시각 `2026-10-15T14:04:46.972659Z`(23:04:46 KST)를 원 조회시각으로 바꾸지 않았다.

| 순위 / 제공사 | 공식 조회 번호 | 실제 결과 제목 / 가수 | checkedAt (KST) | 응답 bytes / SHA-256 |
| --- | --- | --- | --- | --- |
| 11 / TJ | [68419](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68419&strWord=Y) | Walking with you / Novelbright | `2026-10-15T23:03:23.439453+09:00` | 49,238 / `4b6cd434251f7488a9886e72d94052ec066b20b88b84859a3bcd311df1414746` |
| 11 / KY | [44686](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44686) | Walking with you / Novelbright | `2026-10-15T23:03:27.519224+09:00` | 45,984 / `b2ca7709794e0e3c34be6f4a4e7143fd973920aa13d829bd792f3dc60c17d5f8` |
| 12 / TJ | [68333](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68333&strWord=Y) | ドライフラワー / 優里 | `2026-10-15T23:03:28.525583+09:00` | 49,228 / `4a12616965b426eb669af86fef538491768ae061780d6fcc867a726649cbdc10` |
| 12 / KY | [44652](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44652) | ドライフラワー / 優里 | `2026-10-15T23:03:31.661086+09:00` | 46,040 / `0c65cb223d178a9e580f2a9a83f29d462588f22132e04f8606ee946226b97736` |
| 13 / TJ | [68446](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68446&strWord=Y) | 水平線 / back number | `2026-10-15T23:03:32.326257+09:00` | 49,239 / `3bd510368d5ddb8bca9f6dac149977ba87a518ce5af047fdf02d35721f60a404` |
| 13 / KY | [44701](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44701) | 水平線 / back number | `2026-10-15T23:03:35.981521+09:00` | 46,284 / `7e51835302e3dad5315006e940b38833f479bf4fca321c59a162041a0e47ea6d` |
| 14 / TJ | [68807](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68807&strWord=Y) | 踊り子 / Vaundy | `2026-10-15T23:03:36.625532+09:00` | 49,216 / `5534f29b576c5e4178cfe0e783ffc21ff6c54adef85fdcb754ec63fe2341ee6c` |
| 14 / KY | [76508](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=76508) | 踊り子 / Vaundy | `2026-10-15T23:03:39.142231+09:00` | 46,172 / `92d855f18468d8cb77b562eaeba36c9ad5ee9972acc461136f6a1d7fa58f515d` |

각 요청의 UTC 원시각·시작 시각·status200·동일 finalUrl·원 unreviewed 후보·8개 개별 decision은 [metadata 전용 JSON](provider-review-requests-20261015-2300.json)에 영속 보존했다. hash는 실제 fetch가 받은 전체 응답 bytes로 계산한 값이다. 원 응답을 저장하지 않았으므로 다음 환경에서는 이 hash로 같은 원문을 다시 재해석할 수 없으며, 제공사 응답을 새로 받으면 새 시각/hash의 별도 후보가 된다. 원문 부재를 숨기거나 새 조회 없이 날짜를 갱신하지 않는다.

## 보존과 실제 검증

기존 ledger63개 전체 객체·순서·checkedAt·evidence·reason 및 최상위 metadata는 `/tmp/jpop-2300-base-ledger.json`과 같음을 직접 확인했다. 마지막 배열/객체 닫기 전까지의 기존 bytes prefix도 그대로 유지했다. confirm8만 끝에 추가했으며 suppress13은 변경하지 않았다. 원본 unknown을 unlisted로 바꾸거나 번호/ID/기존 URL/localStorage를 수정하지 않았다. 독립 root/품질 담당 검증은 별도로 수행한다.

| 입력 / 결과 | SHA-256 |
| --- | --- |
| ledger63 원본 bytes | `c2ae151b40a694ad99642255be2b87d26ef0729fd7557e201b681c3cfba64513` |
| append 후 ledger71 bytes | `12996fc061e68be0a17924fea9fd2fa624cce8626a9e1699fb26055c2b6c22d6` |
| staging / base bytes와 동일 | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| chart snapshot / base bytes와 동일 | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| legacy identity baseline / base bytes와 동일 | `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8` |
| safe helper / base bytes와 동일 | `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243` |

23:05:00 KST에 기존 `validateProviderCorrections(ledger, staging, {now})`를 직접 실행해71개 entry의 정확한 schema·revision·실존ID·중복 ID/provider·관측번호 일치·문자열/숫자조회 URL·실제 checkedAt·hash 필드 검증을 통과했다. 같은 입력에 `buildPublicCatalog`와 `applyProviderCorrections`을 직접 호출해3,317 stableID와3,299 current/playlist를 유지하고 새8행 모두 `provider-confirmed`, 원래 문자열 번호, `conflict:false`로 투영됨을 확인했다. 검증 원시각과8행 출력은 위 JSON의 `validation`에 있다.

current/playlist3,299곡에서 전체 confirmed는 TJ26·KY24이나 이중 충돌 번호는 안전 탐색에 포함하지 않는다. 실제 conflict없는 confirmed는 **TJ24·KY22, 업체 합집합24곡**이다. TJ24+KY22=46은 업체별 번호 수이며46곡이 아니다. 관측 TJ3,109·KY2,467, unknown TJ164·KY808을 유지한다. 업체0이나 목록 미포함을 공식미수록으로 설명하지 않는다. 새8개는 기존 observed에서만 confirm으로 바뀌었으며 suppress·교체번호·새unknown번호·unlisted선언은0개다.

## 한계와 인계

이번 실행의 실제 조회 실패는0건이다. 조회·파싱·쓰기 실패나 불명인 경우 후보/ledger를 자동 승격하지 않고 마지막 정상 자료를 보존하는 기존 하네스 계약은 유지했다. 실패를 일부러 실서비스에 만들거나 광고 요청·클릭을 실행하지 않았다.

공식 공개 검색을 읽은 것은 재배포 허락·가사/이미지/음원 권리·AdSense 계정/심사 결과의 근거가 아니다. 권리와 Google 실제 상태는 미확인이다. 가사·원문 HTML·시크릿·전체 환경·credential 파일을 Git/문서/로그/public artifact에 남기지 않았다. 새 편집문서가 사용자용 독창적 공개 본문·운영 배포를 대신한다고 주장하지 않는다. 공개 projection은 기존 allowlist대로 내부 response hash를 제거하며 root가 artifact/browser에서 다시 확인해야 한다.

담당 검증 완료 범위는 실제8요청/편집대조·원본보존·기존 validator/projection 실행이다. 전체 unit/type/build·두 target 재현·브라우저·CI·sameSHA preview/production·실제복구·GitHub Release·Discord는 root와 다른 담당의 실제 결과에 연결한다. 이 시점에는 해당 결과를 통과로 미리 기록하지 않았다. source/docs commit·PR·최종 SHA도 root가 실제 생성한 뒤 인계한다.

동료 두 명의 읽기 평가와 점수는 실제 요청 후 다른 두 산출물을 읽고 아래에 별도 보존한다. 현재 평가를 미리 만들어 쓰지 않았다.

### 인계 전 담당 회귀 확인

`node --test --test-isolation=none tests/provider-corrections.test.mjs`를 직접 실행해 기존 계약 회귀 **8/8 PASS·실패0·46.79252ms**를 확인했다. 이 합성 fixture 검사를 새8행의 공식 정체성 판단으로 대신하지 않았다. 별도 읽기 검사에서 요청8개·명시적 decision8개·append8개의 ID/provider/번호/전체evidence가 일대일로 일치하고, 문서의 번호/전체제목·가수/공식URL/hash가 각 candidate와 일치함을 확인했다. 문서 상대 링크의 실제 파일, 후보 임시 디렉터리에 metadata JSON만 있는 것, ledger 결과 bytes SHA도 확인했다. 상세 결과는 영속 JSON `localChecks`에 있다.

## 실제 동료 읽기 평가 · 23시 run

평가 기록 UTC `2026-10-15T14:16:52.292881Z`. root 요청에 따라 본인 문서만 편집했다. 앱/품질 소스·두 문서·최종 영속 QA JSON·root unit/type/preview 로그를 읽고 영속 PNG6장을 모두 열었다. 리뷰 시점 production26은 실행중이며 미확정이다. 새 서버/Git/공식요청/worker나 추가 브라우저 실행은 하지 않았다. 평가 기준은 할당된 수용 기준의 실제 충족이며 순위를 만들기 위한 감점이나 미확인 결함을 만들지 않았다.

```json
{
  "reviews": [
    {
      "reviewer": "/root/resume23_evidence",
      "candidate": "/root/resume23_app",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "artists.astro를 직접 읽었다. 기존 confirmedNumbers/buildConfirmedArtistGroups 판정을 그대로 사용하고 업체별 song.id Set과 전체 ID Set을 따로 계산해 TJ24·금영22·합집합24를 표시한다. 전역·모든 가수 바로가기·그룹 수와 0곡의 미수록 아님 의미를 SSR 본문 및 실제 aria-describedby/role=group으로 제공하고, 기존 anchor·곡URL·원문명의·근거/시각을 보존한다.",
        "evidence": "최종 14:12:35–39Z dirty preview의 영속 QA JSON과 PNG6장을 모두 실제 열어 읽었다. 3viewport에서82개 수치·20그룹·24/22/24 독립집계, JS-off 키보드·0곡·마지막그룹, 넘침/clip/console/광고0 및 정확한 브랜드/palette가 코드/화면과 맞는다. 잘못된 초기 URL/브랜드 selector 진단3회와 이전 p artifact는 최종divrole 결과와 구분해 기록했다.",
        "value": "방문할 노래방 업체에 맞는 확인곡 수를 가수 선택 전후에 바로 비교하게 하며 전체46곡으로 오해하는 중복합산과 0=미수록 오해를 실제 문구로 줄인다. 360px 화면의 상단요약·두열목록·0곡번호카드와 마지막 가수도 읽을 수 있고 기존 상세/공식근거 흐름이 이어진다.",
        "maintainability": "소유 페이지와 문서만 수정하며 기존 Layout/helper·static SSR·원문그룹·native anchor·scoped CSS를 재사용한다. 간단한 countProviders 함수가 global/group 계산을 공유하고 수치를 하드코딩하지 않는다. 명시적 data속성/aria 계약과 최종82수치 독립회귀가 후속자료 append를 지원한다. 검토 범위에서 별도 유지보수 차단을 발견하지 않았다."
      },
      "reason": "할당된 업체별 확인곡 수와 모바일/키보드·브랜드 계약을 실제 파일/최종PNG/QA 및 root310unit·type0·preview26 로그로 수용한다. 새 브라우저나 공식조회를 이 리뷰에서 실행하지 않았고, root coverage 기존고정값 수정은 앱 담당 소유 성과로 가산하지 않는다. 현재 cleanCI/원격preview/운영/복구/Release는 pending이다.",
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
        "/tmp/jpop-2300-unit.txt",
        "/tmp/jpop-2300-check-final.txt",
        "/tmp/jpop-2300-preview-e2e-final.txt"
      ],
      "acceptance": "accepted_for_integration; 담당 구현/최종 로컬 QA 계약 충족, 필수 보완 없음. clean sameSHA CI·원격preview·운영·실제복구·Release는 root의 후속 검증 필요."
    },
    {
      "reviewer": "/root/resume23_evidence",
      "candidate": "/root/resume23_quality",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "3개 cohort unit과2개 E2E를 직접 읽었다. 기존63전체객체·순서의 canonicalhash, raw/chart bytes, 실제8행 전체 제목/가수/번호/URL/시각/hash를 고정하며3,317ID·기존URL·비대상전체객체·같은제목Ado의 관측/unknown을 보존한다. E2E는 production helper를 import하지 않고 served catalog에서 distinct ID Set을 계산하여 global/index/group·실제번호포함·합집합·0곡·JS-off/360px/키보드·target별meta를 검사한다.",
        "evidence": "품질 문서의3unit PASS214.918209ms/최초 단위 가정오류2FAIL/정정2PASS11.5s와 최종role추가 경계를 읽었다. root 실제310/310 PASS11.481s 로그에서3새unit을, 최종preview26/26 PASS44.6s 로그에서2새E2E를 확인했다. 실제8metadata와 test의 frozen tuple도 일치한다. 초기24PASS2FAIL은 root의coverage기존고정값 문제이며 품질2검사의 실패나 최종통과로 뒤섞지 않는다.",
        "value": "잘못된 근거가 같은 제목의 다른 가수에 전파되는 문제와 기존63정정/ID/URL 손상, 업체번호 수를 곡수로 합산하는 문제를 구체적으로 막는다. 실제0곡·공동명의 원문그룹·키보드상세·광고/오버레이 경계가 사용자에게 보이는 결과를 독립검증한다. 검증이 공식사람대조나 AdSense승인을 대체한다고 하지 않는다.",
        "maintainability": "검사 범위가 별도 두 소스파일에 분리되고 frozen cohort63~71만 고정하여 나중 append가 회귀 의미를 바꾸지 않도록 했다. 최소 tuple/URL builder·independentCounts·verifyCounts를 재사용하고 서버/공용package/CI를 변경하지 않는다. 합성/실제자료/dirty실행/원격미실행과 실패원인을 문서로 구분해 재개할 수 있다. 현재 계약에 필수적인 유지보수 결함을 발견하지 않았다."
      },
      "reason": "실제 파일·메타데이터·로그를 읽어 할당된 회귀와 독립집계 검사를 수용한다. 새 테스트/브라우저를 리뷰에서 재실행하지 않았고 기존8계약 회귀 실행과 root전체310을 중복 합산하지 않는다. 아직 실행중인 production26과 cleanCI/preview/운영/복구/Release를 통과로 가정하지 않는다.",
      "evidence": [
        "jpoptoday/tests/reviewed-cohort-2300.test.mjs",
        "jpoptoday/e2e/artist-provider-counts.spec.ts",
        "docs/engineering/artist-provider-counts-quality-20261015-2300.md",
        "docs/data/provider-review-requests-20261015-2300.json",
        "docs/operations/runs/resume-20261015-2300-ui-qa.json",
        "/tmp/jpop-2300-unit.txt",
        "/tmp/jpop-2300-check-final.txt",
        "/tmp/jpop-2300-preview-e2e-final.txt"
      ],
      "acceptance": "accepted_for_integration; 담당3unit/2E2E 및 최종로컬preview 통과 수용, 필수 보완 없음. production로컬/cleanCI·원격preview·운영·실제복구·Release는 후속 실제 증거 필요."
    }
  ]
}
```
