# 동결 차트 상위 10곡의 공식 번호 검토 · 2026-10-15

`DATA-001` · `SONG-001` · `CONTENT-001` · `SEO-001` · `ADS-001` · [Issue #6](https://github.com/jpop-today/jpoptoday/issues/6) · [제품 스펙](../product/spec.md) · [ADR-006](../adr/006-reviewed-corrections-and-production.md).

## 작업 계약과 실제 범위

| 항목 | 이번 실행 |
| --- | --- |
| run / 담당 | `resume-20261015-0300` / `/root/resume_0300_evidence` |
| 소스 / base / branch | `jpop-today/jpoptoday` / `d17b9c8ad555cd97214c67b864a9c263be402186` / `codex/6-reviewed-discovery-20261015` |
| docs base | `78c91f42a01b10639c01463e84a7c2a427d503d1` |
| 단독 소유 파일 | 소스 `data/reviewed/provider-corrections.json`, docs `data/provider-review-20261015.md` |
| 환경 직접 확인 | managed runtime revision141, observed current/running/connected, network unrestricted/enforced. runtime/network skill과 executor policy를 읽고 상속 proxy/CA·TLS를 유지 |
| 후보 선정 | 동결 `chart-snapshot.json` rank1–10의 canonical playlist ID, 기존 TJ 관측 번호 10개와 KY 관측 번호 10개. 원본 43개 ledger entry에는 없는 ID/provider 쌍 |
| 결과 | 공식 numeric GET 20개 모두 HTTP200·최종 URL 동일·조회 번호와 일치하는 결과 1행. 제목·가수·부제·공동명의를 행별 대조해 20개 confirm만 추가 |
| 통합 경계 | Git·공용 계약·UI·package/lockfile·CI·build·preview·production·Release는 root 담당. 이 문서의 데이터 검토 완료는 게시/배포 또는 AdSense 승인 완료가 아님 |

후보의 차트 순위는 이미 동결된 `2026-10-13T10:26:45.525Z` 수집 스냅샷의 `2026-09-29–2026-10-13` 요청 기간에서 가져왔다. 이번 공식 번호 조회 시각을 차트 수집 시각이나 새 순위의 증거로 바꾸지 않는다. 이번 조회는 10곡·20제공사 번호의 범위이며 3,299곡 전체 목록을 검증한 것이 아니다.

원본 관측 번호만으로 공식 확인 상태를 만들지 않는다. 조회 실패·결과 0행·정체성 미확보는 기존 `observed-unverified`/`unknown` 및 ledger를 그대로 보존한다. 명시적인 다른 곡·가수·버전의 반증이 있는 경우에만 별도 검토로 suppress를 추가한다. 이번 20행은 실제 일치 근거가 확보돼 confirm했고, suppress·번호 교체·새 unknown 번호 추가·unlisted 선언은 모두 0개다. 이전 suppress13개는 변경하지 않았다.

## 대상 ID와 대응 판단

가수 별칭·정체성·공동명의를 자동 병합하지 않는다. 아래 표의 원본 제목·가수는 stable ID에 묶인 staging의 관리 표기다. 공식 결과의 별도 OST/영화 부제·공백·구두점 차이는 개별 검토했고, provider evidence에는 실제 공식 제목·가수를 그대로 보존했다. 오디오를 청취하거나 같은 음원·모든 반주기 모델 지원을 확인한 것은 아니다.

| 동결 순위 | stable ID | 원본 제목 / 가수 | TJ / KY | 개별 검토 |
| --- | --- | --- | --- | --- |
| 1 | `jpop-playlist-tuki-晩餐歌-9ccdde9634` | 晩餐歌 / tuki. | `52788` / `57828` | 두 제공사 제목·가수 일치 |
| 2 | `jpop-playlist-official髭男dism-pretender-89fe1cbab6` | Pretender / Official髭男dism | `68058` / `44438` | TJ 기본 제목, KY 영화 コンフィデンスマンJP 부제. 동일 제목·가수 대응, 별도 버전 추정 없음 |
| 3 | `jpop-playlist-vaundy-怪獣の花唄-cd1e6ddba7` | 怪獣の花唄 / Vaundy | `52988` / `57775` | 두 제공사 제목·가수 일치 |
| 4 | `jpop-playlist-優里-ヘ-テルキ-ウス-73cf88ae11` | ベテルギウス / 優里 | `68553` / `44746` | 제목·가수 대응, SUPER RICH 드라마 부제를 공식 그대로 유지 |
| 5 | `jpop-playlist-高橋洋子-残酷な天使のテ-セ-e2b770fb52` | 残酷な天使のテーゼ / 高橋洋子 | `25246` / `41645` | 제목·가수 대응, 新世紀エヴァンゲリオン OP 부제를 공식 그대로 유지 |
| 6 | `jpop-playlist-back-number-ヒロイン-ae2d342933` | ヒロイン / back number | `27762` / `43998` | 두 제공사 제목·가수 일치 |
| 7 | `jpop-playlist-spyair-サムライハ-ト-some-like-it-hot-3414d0a48e` | サムライハート(Some Like It Hot!!) / SPYAIR | `27527` / `43372` | 같은 SPYAIR·サムライハート. 공백·it/It·Hot/Hot!! 표기 차이와 銀魂 ED 부제를 개별 검토 |
| 8 | `jpop-playlist-米津玄師-lemon-656053b8dd` | Lemon / 米津玄師 | `28822` / `44253` | 제목·가수 대응, アンナチュラル 드라마 부제를 공식 그대로 유지 |
| 9 | `jpop-playlist-米津玄師-iris-out-f940512a4c` | IRIS OUT / 米津玄師 | `52959` / `57785` | 제목·가수 대응, チェンソーマン レゼ篇 OST 부제를 공식 그대로 유지 |
| 10 | `jpop-playlist-米津玄師-宇多田ヒカル-jane-doe-9679a6d5fb` | JANE DOE / 米津玄師，宇多田ヒカル | `52972` / `57784` | 두 가수 米津玄師·宇多田ヒカル 일치. 원본 ，/ TJ & / KY 、 연결기호 차이를 검토하고 공동명의를 보존 |

## 실제 조회와 원문 해시

`python3 /tmp/jpop-provider-review-20261015.py`에서 표준 `urllib.request`·`HTMLParser`·`hashlib`를 사용했다. 공개 numeric GET만 최대2개 동시·각25초 timeout·8MiB 본문 상한으로 요청했다. TJ는 `span.num2`, `li.grid-item.title3`, `li.grid-item.title4.singer`를 같은 `ul.grid-container.list`에서, KY는 같은 `tr`의 `td.search_col02`와 `td.search_col03`를 읽었다. input에 검색 번호가 보이는 것과 실제 결과 번호를 구분하고 KY의 `제목 / 가수` 셀을 마지막 구분자로 나눴다. 행 안의 공백 정규화와 HTML entity 해석만 했으며 원문 응답 bytes의 SHA-256을 계산했다.

요청 시작은 `2026-10-15T03:03:27.960810+09:00`, 각 완료/checkedAt 범위는 `2026-10-15T03:03:29.916792+09:00–03:03:50.023395+09:00`다. 아래 20행 모두 실제 HTTP200·최종 URL 동일·번호 결과 1행이며 전체 저장 bytes의 SHA-256과 UTF-8 strict decode를 재확인했다. 가사·이미지·원문 HTML을 Git이나 공개 artifact로 복사하지 않는다.

| 순위 / 제공사 | 실제 조회 번호·공식 URL | 결과 제목 / 가수 | checkedAt (KST) | response SHA-256 |
| --- | --- | --- | --- | --- |
| 1 / TJ | [52788](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52788&strWord=Y) | 晩餐歌 / tuki. | `2026-10-15T03:03:29.916792+09:00` | `14abaca062b775b697539a7386dfca73b7eb4b99e66dfcb509b30c144292667c` |
| 1 / KY | [57828](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57828) | 晩餐歌 / tuki. | `2026-10-15T03:03:32.395521+09:00` | `9ca6c7925ee9e8d69e033455758e8c6a5e28ea049d8df287ef3767550c14c352` |
| 2 / TJ | [68058](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68058&strWord=Y) | Pretender / Official髭男dism | `2026-10-15T03:03:31.931967+09:00` | `7ce0a9f03124607bc73cabd8d5acd862062777366c4d2699baf43379003c6311` |
| 2 / KY | [44438](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44438) | Pretender (映画"コンフィデンスマンJP") / Official髭男dism | `2026-10-15T03:03:35.207126+09:00` | `391d447e399960740acff76bf9552c229a70f9b0535539e4ef5897e6fe633c88` |
| 3 / TJ | [52988](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52988&strWord=Y) | 怪獣の花唄 / Vaundy | `2026-10-15T03:03:33.094945+09:00` | `5002f940cc4c84e851276fd9e9981eb8bea4609e6bae0b76412b6fd1e12c9c4d` |
| 3 / KY | [57775](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57775) | 怪獣の花唄 / Vaundy | `2026-10-15T03:03:36.605608+09:00` | `8fdd3d4523f640c22dc32f91ad3096281d93a7767492033fc4d630d89801459f` |
| 4 / TJ | [68553](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68553&strWord=Y) | ベテルギウス(ドラマ'SUPER RICH' OST) / 優里 | `2026-10-15T03:03:35.906179+09:00` | `7be80e5821e37019ef807a735e0edeba258dac0b3173e214953a6c428904f34e` |
| 4 / KY | [44746](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44746) | ベテルギウス (ドラマ"SUPER RICH") / 優里 | `2026-10-15T03:03:39.854415+09:00` | `6ab41f33502aede4f3dcbcc2d210837a1897d392439a9ebff2df859bfb42b3ec` |
| 5 / TJ | [25246](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=25246&strWord=Y) | 残酷な天使のテーゼ(新世紀エヴァンゲリオン OP) / 高橋洋子 | `2026-10-15T03:03:37.371398+09:00` | `1ba715a1126c192a293d25860ea46d01c316d4cbf9e980b3fd10aa798135c8d7` |
| 5 / KY | [41645](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=41645) | 残酷な天使のテーゼ ("新世紀エヴァンゲリオン"OP) / 高橋洋子 | `2026-10-15T03:03:40.731054+09:00` | `e55d654bedec41eae5dbba60fb539f6ad912d09ebacc33c0b52a6bad67ac7726` |
| 6 / TJ | [27762](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27762&strWord=Y) | ヒロイン / back number | `2026-10-15T03:03:40.701401+09:00` | `92784dc468b7dd6c09393d55808ee0fc56a99ab52987e0941f9eaf08962369d4` |
| 6 / KY | [43998](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43998) | ヒロイン / back number | `2026-10-15T03:03:44.033446+09:00` | `f296e3d4acbb7fb689b4ad8566aed0129caed8f8fa0b02826ecec99944a37062` |
| 7 / TJ | [27527](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27527&strWord=Y) | サムライハート (Some Like it Hot)(銀魂 ED) / SPYAIR | `2026-10-15T03:03:41.493546+09:00` | `bdc1fa3aa3402eeecdcd10e730d3689ff011a86cc9e3d1823c4c26d0c4eb3838` |
| 7 / KY | [43372](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43372) | サムライハート (Some Like It Hot!!) ("銀魂"ED) / SPYAIR | `2026-10-15T03:03:44.173265+09:00` | `a9cf41213631b8f3e5abad6d051cde81f4b2c7510346595ebb227d5bf81545cf` |
| 8 / TJ | [28822](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=28822&strWord=Y) | Lemon(ドラマ'アンナチュラル' OST) / 米津玄師 | `2026-10-15T03:03:44.695792+09:00` | `19d1371d4ab4cf292207cb1cda5ac4078ad8038e4d518b518dc04c6485593899` |
| 8 / KY | [44253](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44253) | Lemon (ドラマ"アンナチュラル") / 米津玄師 | `2026-10-15T03:03:47.031339+09:00` | `99ae24f5fbb2f864230580853358b92cf6ba6b0b232a09227db89ffdff3de76c` |
| 9 / TJ | [52959](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52959&strWord=Y) | IRIS OUT('劇場版 チェンソーマン レゼ篇' OST) / 米津玄師 | `2026-10-15T03:03:45.029594+09:00` | `f4bc975b9f1088c7ad072bad34e6fde6ddc14058c7d81ff00d6b06a236bd50cb` |
| 9 / KY | [57785](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57785) | IRIS OUT ("チェンソーマン レゼ篇"OST) / 米津玄師 | `2026-10-15T03:03:47.198363+09:00` | `60c7c0532fd418d5e195b6b9db7b895cb828f9d68f43a318ae1fa8718575e834` |
| 10 / TJ | [52972](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52972&strWord=Y) | JANE DOE('劇場版 チェンソーマン レゼ篇' OST) / 米津玄師 & 宇多田ヒカル | `2026-10-15T03:03:47.678068+09:00` | `743d8711c14ae6f92b529aeb4b59c338f5ff422cd1bcc1b593f1579fba8520c6` |
| 10 / KY | [57784](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57784) | JANE DOE ("チェンソーマン レゼ篇"OST) / 米津玄師、宇多田ヒカル | `2026-10-15T03:03:50.023395+09:00` | `53d9a05ca4f2c04e7c8383ba1415ddcb8f308c2ac674009d3265d7e22cf5a23a` |

## 원본 보존·실제 검증

원본 staging·동결 차트·ID·기존 URL·즐겨찾기/localStorage 키를 수정하지 않았다. 기존43개 ledger entry의 객체·배열 순서·각 checkedAt·근거·reason을 그대로 유지했고, 기존 파일의 마지막 배열/객체 닫기 부분 직전까지의 bytes prefix도 일치한다. 끝에 실제 검토한20개를 추가해 ledger63개(confirm50/suppress13)가 됐다. 번호는 기존 문자열 그대로이며 원본 null·선행0을 숫자로 변환하지 않았다.

| 입력 | 보존/결과 SHA-256 (원본 bytes) |
| --- | --- |
| `data/staging/legacy-catalog.json` / 변경 없음 | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/chart-snapshot.json` / 변경 없음 | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| 기존 ledger43entry | `afc086b277b749db7398e638c06487d8d9d1d09bb74a196d5d49211a7c62b8a8` |
| 추가 후 ledger63entry | `c2ae151b40a694ad99642255be2b87d26ef0729fd7557e201b681c3cfba64513` |

`validateProviderCorrections(ledger, staging, {now: new Date()})`을 실제 현재 시각 `2026-10-14T18:05:22.042Z`(KST03:05:22)에 호출해63개 entry의 revision·ID·provider 쌍 중복·expectedObservedNumber·string·숫자조회 HTTPS URL·실제 checkedAt·근거 shape 검사를 통과했다. validator의 형식 통과를 곡 정체성 검토로 대신하지 않았으며 정체성 판단은 위20개 실제 결과를 읽어 따로 수행했다.

같은 입력으로 `buildPublicCatalog`와 `applyProviderCorrections`을 직접 호출해 공개 projection이3,317 ID를 유지하고 추가20개 번호가 `provider-confirmed`, `conflict:false`임을 확인했다. current playlist3,299곡을 분모로 TJ 확인22/관측3113/unknown164, KY 확인20/관측2471/unknown808이다. 확인 번호 중 기존 conflict4개는 그대로 제외해야 하므로 conflict없는 확인 번호는 TJ20/KY18, 최소 한 제공사 안전 확인을 가진 playlist ID는20개다. 기존 수치에 새20번호를 실제로 더한 결과이며 chart-only·archived를 분모에 포함하지 않았다.

`node --test --test-isolation=none tests/provider-corrections.test.mjs` 실제8/8 PASS(45.94ms). 합성 fixture의 불변성·원본 보존·suppress unknown·추가/공유번호 conflict·허용공개필드·잘못된 revision/ID/번호/날짜/출처/private필드·부분 입력 실패를 확인한 기존 회귀 검사다. 이8개 테스트는 새20곡 공식 제목/가수의 독립 판정이나 전체 앱/browser/배포 검사를 뜻하지 않는다.

## 권리·실패·인계 경계

공식 공개 메타데이터를 읽은 사실은 재배포 허락·음원/가사/이미지 권리·광고 심사 승인과 별개다. 권리와 실제 Google 계정/심사 결과는 미확인이다. 공개 콘텐츠에는 번호·곡명·가수·공식 링크·실제 확인일·개별 판단만 전달하고 내부 responseSha256·raw HTML·가사·이미지를 보내지 않는다. preview는 전부 noindex·광고 요청 없음이며 production 게이트와 실제 상태는 root가 별도로 확인한다. 난이도·음역·추천·가수 약력·모델별 지원·권리 허락·심사 결과를 만들어내지 않았다.

원문 검사 중 KY 결과셀 주변을 고정 길이로 출력한 한 번의 도구 출력에 뒤따르던 modal 가사 일부가 섞였다. Git·문서·공개 artifact에는 복사하지 않았으며 이후 원문 주변 출력은 금지하고 번호/제목/가수 셀 allowlist만 추출한다. 이 기록에 그 원문을 재게시하지 않으며 가사는 번호 판정에 사용하지 않았다. 원문 응답은 아래 `/tmp` 검토 경로에만 보존한다.

| 인계 증거 | 상태 / 위치 |
| --- | --- |
| 실제 HTTP 요청·완료시각·번호/제목/가수·hash | [공개 검토 기록의 안전한 20행](provider-review-requests-20261015.json) / 원본 `/tmp/jpop-provider-review-20261015/requests.json`. root가 선별 복사한 metadata이며 원문 HTML을 포함하지 않음 |
| 원문 bytes | `/tmp/jpop-provider-review-20261015/tj-<number>.html`, `ky-<number>.html` (검토용 임시 파일, Git/public 제외) |
| 실행 스크립트 | `/tmp/jpop-provider-review-20261015.py` (metadata 결과셀만 파싱) |
| 변경 전 ledger | `/tmp/jpop-provider-review-20261015/ledger-before.json` |
| 실제 validator/projection 결과 | `/tmp/jpop-provider-review-20261015/validation.json` |
| 원본43개 유지·raw/staging/chart·metadata 불변 | 담당자 검사 완료, root 독립 검토 필요 |
| 앱 전체 unit/type/build/artifact/browser | 담당자 미실행, root/하네스 담당 실제 결과로 별도 확인 |
| CI·preview·production·복구·Release·AdSense 심사 | 담당자 미실행, root 실제 증거 또는 실제 Google 결과 필요 |
| 두 동료 평가/worker 교체 | 실제 파일을 읽고 평가 요청을 받기 전에는 점수·투표를 만들지 않음 |

[안전한 실제 요청 결과 20행](provider-review-requests-20261015.json)·[기존 정정 기준](provider-corrections.md)·[전일 공식 검토](provider-review-20261014.md)·[동결 차트](chart-snapshot.md)·[AdSense 준비](../operations/adsense.md)를 함께 읽는다. root는 행별 공식 근거와 원본 보존을 독립 확인하고 같은 SHA의 CI→clean preview→정책 게이트→실제 복구/운영 HTTP 검사를 수행한 결과만 통합·배포한다. 최종 source/docs SHA·PR·배포는 root의 실제 운영/릴리스 기록에 연결한다.

## 읽기 전용 동료 평가 원본

다른 두 담당의 실제 산출물·로그·이미지를 읽고 root에 전달한 최종 평가 배열을 내용·점수 변경 없이 보존한다. 평가 당시의 local dirty 검증 경계를 유지하며 이후 원격 CI·preview·production·복구 증거로 바꾸지 않는다.

```json
[
  {
    "reviewer": "/root/resume_0300_evidence",
    "candidate": "/root/resume_0300_ui",
    "scores": {"correctness": 5, "evidence": 5, "value": 5, "maintainability": 4},
    "criterionReasons": {
      "correctness": "실제 artists.astro는 root의 confirmedNumbers/group helper 결과만 렌더하고 현재 playlist·공식근거·conflict없는 번호만 표시한다. 원문 공동명의를 별도 그룹으로 유지하고 공식제목/가수/번호/checkedAt/기존 songHref를 그대로 사용한다. unknown·관측·충돌의 다른 업체는 번호없이 상태만 설명하며 미수록으로 단정하지 않는다.",
      "evidence": "실제 source/doc, QA receipt3viewport, desktop/mobile/mobilejoint/shortdesktop PNG4개를 직접 읽었다. receipt19그룹/20ID/38번호·360px JS-off/키보드→JANE DOE·52972/57784·실제시각·넘침/콘솔/광고없음이 렌더와 일치한다. root최종286unit/type0/full22browser34.3s 로그도 직접 읽었고 dirty local을 원격배포로 주장하지 않는다.",
      "value": "기존 검색 외에 아는 가수에서 공식 확인곡과 업체별근거로 바로 이동하는 정적탐색을 제공한다. 첫jump·가수nativeanchor·상세URL은 JS-off에서도 사용가능하고 별칭/공동명의·버전과번호업체를 구분하는 안내가 실제 정정사례가이드로 이어진다. 원래 gradient/palette 유지, 모바일본문과번호카드가 실제로 잘리지 않는다.",
      "maintainability": "순수정적페이지와 공용helper/기존Layout/songHref를 재사용하고 글로벌CSS·데이터·저장동작은 건드리지 않아 유지범위가 명확하다. 현재 날짜표시/업체라벨/카드별 scopedCSS는 페이지 내부에 모여 있으며 coverage와 유사한 표현이 반복돼 향후 여러 탐색페이지가 늘면 중복관리 여지는 남는다. 현재 수용을 막는 결함은 없다."
    },
    "evidencepaths": ["/workspace/jpoptoday/src/pages/artists.astro", "/workspace/docs/engineering/artist-discovery-20261015.md", "/workspace/docs/operations/runs/resume-20261015-0300-ui-qa-verified.json", "/workspace/docs/engineering/design/artists-20261015-desktop.png", "/workspace/docs/engineering/design/artists-20261015-mobile.png", "/workspace/docs/engineering/design/artists-20261015-mobile-joint-anchor.png", "/workspace/docs/engineering/design/artists-20261015-short-desktop.png", "/tmp/jpop-0300-unit.txt", "/tmp/jpop-0300-check.txt", "/tmp/jpop-0300-preview-e2e-passed.txt"],
    "reason": "실제파일·렌더·상호작용 근거가 데이터정확성/호환/브랜드 수용기준에 부합한다.",
    "acceptance": "accepted-local; clean 동일SHA CI/preview/production/복구는 root의 후속 게이트이며 아직 이 평가로 완료 선언하지 않음"
  },
  {
    "reviewer": "/root/resume_0300_evidence",
    "candidate": "/root/resume_0300_quality",
    "scores": {"correctness": 5, "evidence": 5, "value": 5, "maintainability": 4},
    "criterionReasons": {
      "correctness": "11unit negativefixture가 관측/unknown·conflict·불완전근거·URL업체/번호/credential·선행0/숫자강제변환·archived/chart-only·중복ID·공동명의/별칭/case·anchor불변·deepfreeze를 다룬다. 실제기대집합은 raw staging에 ledger를 독립적용한 중복수로 계산해 helper출력을 재사용하지 않는다. 3e2e는 실제catalog의전체ID/근거·JS-off360키보드·기존객체favorite/미해결ID/legacyURL을 확인한다.",
      "evidence": "실제 test/e2e/helper/doc·root286/type0/최종22PASS34.3s와 첫21PASS/1FAIL33.4s 로그를 직접 읽었다. navigation 전 URL읽기 실패를 숨기지 않고 같은 exact경로조건을 유지한 toHaveURL/fragment expect.poll로 실제완료를 기다렸다. 품질doc의 table/최종문단이22PASS로 갱신된 것도 재독했고 localdirty와 원격CI/배포 미완료가 구분된다.",
      "value": "공식확인으로 잘못 승격하거나 다른가수를 병합하는 핵심 데이터위험과 기존 저장·URL 호환을 실제 테스트로 막는다. 페이지표시를 실제 served catalog와 비교하고 모든근거·checkedAt·robots/sitemap·광고/콘솔경계를 확인해 release가 근거없는 콘텐츠를 배포하지 않도록 돕는다.",
      "maintainability": "의존성추가없이 Node test/기존Playwright에 통합하고 합성fixture와 실동결자료 검증을 구분한다. literal경로/독립 safe판정이 일부 기존하네스와 반복되며 실제catalog test는현재노래셋 전체를순회해 향후 증가시 실행시간 관리가 필요하다. 이 독립판정은 지금 회귀검증 가치가 있고 현재11unit·22browser가 실제통과했다."
    },
    "evidencepaths": ["/workspace/jpoptoday/tests/confirmed-discovery.test.mjs", "/workspace/jpoptoday/e2e/artists.spec.ts", "/workspace/jpoptoday/contracts/confirmed-discovery.mjs", "/workspace/docs/engineering/confirmed-discovery-harness-20261015.md", "/tmp/jpop-0300-unit.txt", "/tmp/jpop-0300-check.txt", "/tmp/jpop-0300-preview-e2e-final.txt", "/tmp/jpop-0300-preview-e2e-passed.txt", "/workspace/docs/operations/runs/resume-20261015-0300-ui-qa-verified.json"],
    "reason": "독립fixture와 실제 ledger/catalog/browser의 서로 다른 경계를 실제검증했고 실패원인과보완이 추적가능하다.",
    "acceptance": "accepted-local; Chromium외브라우저·필드성능·권리·실제Google심사와 cleanCI/배포/복구는 이 검사 범위밖"
  }
]
```
