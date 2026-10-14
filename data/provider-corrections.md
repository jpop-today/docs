# 검토된 제공사 번호 정정 ledger

`DATA-001` · `SEARCH-001` · `SONG-001` · `CONTENT-001` · `ADS-001` · [Issue #13](https://github.com/jpop-today/jpoptoday/issues/13) / [Issue #6](https://github.com/jpop-today/jpoptoday/issues/6). 기준은 [ADR-006](../adr/006-reviewed-corrections-and-production.md)이다.

## 목적과 책임

공식 결과와 다른 번호를 검색·표시·복사에서 제외하고, 실제 행별 대조가 끝난 번호만 제공사 확인 상태와 근거를 표시한다. 원본 관측값을 별도 ledger로 교정하며 stable ID·원본 staging·이전 URL·즐겨찾기 키를 보존한다. 전체 관리 목록의 검증이나 권리 허락·AdSense 승인을 선언하지 않는다.

| 항목 | 범위와 기준 |
| --- | --- |
| 담당 | 최초 작성 `/root/daily_20261014_evidence`; 재개 검토 `/root/resume_20261014_evidence` |
| 소유 파일 | 소스 `data/reviewed/provider-corrections.json`, 이 문서만 |
| 통합 | root가 Git·공용 read model·UI·CI·실제 운영 배포 담당. 독립 validator/apply는 하네스 담당 |
| 공유 기준 | 소스 `codex/13-production-transition-20261014` / `9d8cfb716e3ab5194e6711d19ec2b10a297089ad`; docs 같은 branch / `d7ccc98bb20c8d997c0ac20e8264528c63b77aab` |
| 실제 읽은 staging | revision `6dbe5a65d1bd729c09393f746f715f4363089a43`, 3,317 ID, 바이트 SHA-256 `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| 상태 | 23 ID의 43 제공사 entry 행별 검토·공식 28 URL 재확인·실제 ledger 검증 완료. 앱 적용·빌드·운영 배포는 별도 root 실행 |

운영 canonical SHA와 ledger의 `legacyRevision`은 같은 개념이 아니다. ledger는 실제 적용할 staging revision과 `expectedObservedNumber`에 묶인다. staging이 최신 원본으로 바뀌면 대상 ID와 원본 관측값을 다시 대조한 뒤 binding을 갱신해야 한다.

## 계약과 검토 판단

ledger의 `confirm`은 실제 numeric lookup의 번호·제목·가수와 대상 곡이 대응하는 경우다. 기존 관측 번호 교체와 원본 unknown의 새 번호 추가도 가능하되 `expectedObservedNumber`는 읽은 원본 값 그대로다. `suppress`는 공식 결과가 다른 곡·가수·버전이거나 해당 정체성의 적용을 확인하지 못한 경우에 유효 번호를 null로 만든다. 관측 번호는 staging에 남고 미수록을 뜻하지 않는다.

같은 번호의 ID를 자동 병합하지 않는다. 번호가 같은 여러 ID에 개별 confirm이 가능해도 기존 ID와 남은 공유번호 conflict는 보존한다. 문자열 번호·선행 0·원본 null을 그대로 쓰며 확인일은 실제 공식 요청 완료 시각이다. 내부 응답 SHA-256은 근거 추적용이며 원문 HTML·가사·이미지·내부 hash/refs는 공개 payload로 보내지 않는다.

## 우선 반영한 2그룹·4개 ID

아래 7개 제공사 entry는 [09시 공식 재확인 기록](provider-review-20261014.md)의 실제 요청·원본 행-ID 대조를 사용한다. 원본 4파일 해시·TSV/raw/generated/chart 연결과 RADWIMPS 공식 버전 자료는 같은 기록을 참조한다.

| 대상 ID | 제공사 / 원본 → 유효 번호 | 검토 action과 근거 |
| --- | --- | --- |
| `jpop-playlist-ツミキ-フォニイ-a5e8ebf8d0` | KY `57803` → `57803` | confirm. [공식 결과](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57803) `フォニイ` / `ツミキ feat.可不` |
| `jpop-playlist-ナナヲアカリ-チュ-リンク-ラフ-feat-sou-0adf58fbdd` | KY `57803` → null; TJ `52536` → `52536` | KY suppress: 같은 공식 번호는 포니다. TJ confirm: [공식 52536](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52536&strWord=Y) `チューリングラブ` / `ナナヲアカリ(Feat.Sou)` |
| `jpop-playlist-radwimps-前前前世-original-ver-1ff3eb38b9` | TJ `27944` → null; KY `44067` → null | suppress. [TJ 27944](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y)·[KY 44067](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44067)은 movie 표기. original과 movie는 공식 앨범·공지에서 별도 버전이며 병합하지 않음 |
| `radwimps-前前前世-movie-ver-映画-君の名は-ost` | TJ `27944` → `27944`; KY null → `44067` | confirm. 공식 가수·movie 버전과 대응. KY는 원본에 없던 번호를 새 근거로 추가한 것 |

튜링 러브의 KY 대체 번호와 전전전세 original의 제공사 번호는 unknown이다. 검색 0행을 `unlisted`로 변환하지 않는다. 상세·결과·즐겨찾기 번호 목록은 적용 후 유효 번호로만 표시·검색·복사해야 하며 숨긴 이전 번호를 `correction.previousNumber`에서 다시 복사하지 않는다.

## 확장 검토 완료 범위

[전일 11그룹·23개 ID 근거](provider-evidence-20261013.md)의 원본 ID·제목·가수·번호를 다시 읽고, 이전 담당자가 남긴 ledger 43entry를 실제 공식 numeric lookup 28개와 재대조했다. `2026-10-14 14:17:51.429–14:18:10.481 KST`에 28 URL 모두 HTTP 200, 조회 번호와 같은 결과 1행을 반환했다. 43entry의 제목·가수·응답 SHA-256은 기존 ledger와 모두 일치했다. 이번 재개 검토에서는 `checkedAt`만 이 실제 완료 시각으로 갱신했다. 이전 10시 조회를 새 시각의 근거로 둔갑시키지 않았다.

| 검토 결과 | 실제 수량과 의미 |
| --- | --- |
| 검토 대상 | 23 stable ID / 43 ID·provider entry. 전체 관리 목록 검증이 아님 |
| confirm | 30entry / 16 ID에 최소 한 제공사 확인. 동일 번호를 가진 별도 ID는 병합하지 않음 |
| suppress | 13entry. 명시적 다른 곡·가수·버전 5entry와 정체성 판단 보류 8entry를 구분 |
| 원본 unknown에 추가 | KY 4entry: 丸の内 `40074`, movie 전전전세 `44067`, 拝啓、少年よ `76548`, うたたね `57805` |
| 원본 번호 교체 | 전영호 Butter-Fly KY `40554` → `51714` 1entry |
| 원본 보존 | staging revision6d·3,317 ID·바이트 SHA-256 불변, 기존 URL·저장 키 변경 없음 |
| 공식 재확인 | TJ 16 URL / KY 12 URL, 합계 28 HTTP 200. 실제 번호 결과·제목·가수·hash 확인 |
| 미수록 처리 | 0entry. 검색 0행·제목/언어/버전 판단 보류는 unknown이며 unlisted 선언이 아님 |

`confirm`은 곡명·가수·기재된 버전의 공식 검색 대응이라는 뜻이다. 제공사가 실제 재생하는 오디오를 청취하거나 모든 반주기 모델 지원·권리 허락을 확인했다는 뜻은 아니다. TJ `28285`의 공식 행은 `60이상 반주기 전용곡`을 함께 표시하며 제목과 reason에 그 제한을 보존한다. KY `44671`의 실제 말줄임된 부제는 그대로 보존한다.

13개 suppress 중 전전전세 original TJ/KY, V급밴드의 KY, 아이유 파티시엘 KY, 튜링 러브 KY는 공식 결과의 버전·가수·곡명이 대상과 명시적으로 충돌한다. Leina의 《レコおと》 TJ/KY, 中島美嘉의 한국어 제목/MR 관측행 TJ/KY, 분류명 보컬로이드 행 TJ/KY, 사쿠라고 경음부 KY, 最終花 KY는 현재 원본만으로 녹음·언어·가수 정체성을 확정하지 못한 보류다. 후자를 틀린 곡 또는 미수록으로 확정하지 않는다. 단순 ノ/の·구두점·OST/CM 부제 차이와 별도 original/movie·언어·가수 차이를 구분했고 자동 ID 병합은 하지 않았다.

### ID별 유효 번호와 남은 판단

아래는 ledger의 행별 적용값이다. 표시/검색/복사는 유효 번호를 사용하며 suppress의 이전 번호는 복사 대상으로 복원하지 않는다. `—`는 해당 제공사가 미수록이라는 뜻이 아니다. 이 표에 없는 ID·provider는 원본 관측/unknown을 그대로 유지한다.

| stable ID | 제공사 | 원본 → 유효 번호 / action | 판단 근거 |
| --- | --- | --- | --- |
| `jpop-playlist-椎名林檎-丸ノ内サテ-ィスティック-da51e27aca` | TJ | `25837` → `25837` / confirm | 원본의 丸ノ内 표기와 TJ의 丸の内 표기는 제목의 ノ/の 차이이며 곡명과 가수 椎名林檎가 대응합니다. ID는 합치지 않습니다. |
| `jpop-playlist-椎名林檎-丸ノ内サテ-ィスティック-da51e27aca` | KY | `40074` → `40074` / confirm | 금영 공식 번호 검색의 제목 丸ノ内サディスティック와 가수 椎名林檎가 원본 곡과 일치합니다. |
| `椎名林檎-丸の内サテ-ィスティック` | TJ | `25837` → `25837` / confirm | TJ 공식 번호 검색의 제목 丸の内サディスティック와 가수 椎名林檎가 원본 차트 곡과 일치합니다. |
| `椎名林檎-丸の内サテ-ィスティック` | KY | `unknown` → `40074` / confirm | 금영 공식 결과의 丸ノ内/丸の内 표기 차이를 검토했고 같은 가수의 곡명과 대응합니다. 원본에 없던 금영 번호를 별도 근거로 추가합니다. |
| `jpop-playlist-radwimps-前前前世-original-ver-1ff3eb38b9` | TJ | `27944` → `— (unknown)` / suppress | TJ 27944은 공식 검색에서 movie 버전으로 표시됩니다. 별도 original 버전의 번호로 확인할 수 없어 표시와 복사를 보류합니다. |
| `jpop-playlist-radwimps-前前前世-original-ver-1ff3eb38b9` | KY | `44067` → `— (unknown)` / suppress | 금영 44067은 공식 검색에서 movie 버전으로 표시됩니다. 별도 original 버전의 번호로 확인할 수 없어 표시와 복사를 보류합니다. |
| `radwimps-前前前世-movie-ver-映画-君の名は-ost` | TJ | `27944` → `27944` / confirm | TJ 공식 번호 검색의 제목·가수·movie 버전이 이 곡의 원본 차트 표기와 일치합니다. |
| `radwimps-前前前世-movie-ver-映画-君の名は-ost` | KY | `unknown` → `44067` / confirm | 금영 공식 검색에서 같은 가수의 전전전세 movie 버전 44067을 확인했습니다. 원본에 없던 금영 번호를 별도 확인 근거로 추가합니다. |
| `hump-back-拝啓-少年よ-ヘ-ネッセ-進研セ-ミ-高校講座-cm` | TJ | `52756` → `52756` / confirm | TJ 공식 번호 검색의 제목·CM 부제·가수 Hump Back이 원본 차트 표기와 일치합니다. |
| `hump-back-拝啓-少年よ-ヘ-ネッセ-進研セ-ミ-高校講座-cm` | KY | `unknown` → `76548` / confirm | 금영 공식 검색의 拝啓、少年よ와 Hump Back이 대응합니다. 제목의 구두점과 TJ의 CM 부제 차이를 검토한 뒤 원본에 없던 금영 번호를 추가합니다. |
| `jpop-playlist-hump-back-拝啓-少年よ-29d81a757e` | TJ | `52756` → `52756` / confirm | 원본 拝啓、少年よ와 TJ의 拝啓, 少年よ는 구두점 차이이며 가수 Hump Back이 일치합니다. TJ의 CM 부제는 공식 표기 그대로 보존합니다. |
| `jpop-playlist-hump-back-拝啓-少年よ-29d81a757e` | KY | `76548` → `76548` / confirm | 금영 공식 번호 검색의 제목 拝啓、少年よ와 가수 Hump Back이 원본 곡과 일치합니다. |
| `jpop-playlist-leina-うたたね-レコおと-0edbc5afc9` | TJ | `68961` → `— (unknown)` / suppress | 공식 제목은 うたたね이며 원본의 《レコおと》 표기를 확인해 주지 않습니다. 다른 녹음·버전인지 확인하지 못해 이 곡의 번호 표시와 복사를 보류합니다. |
| `jpop-playlist-leina-うたたね-レコおと-0edbc5afc9` | KY | `57805` → `— (unknown)` / suppress | 공식 제목은 うたたね이며 원본의 《レコおと》 표기를 확인해 주지 않습니다. 다른 녹음·버전인지 확인하지 못해 이 곡의 번호 표시와 복사를 보류합니다. |
| `leina-うたたね` | TJ | `68961` → `68961` / confirm | TJ 공식 번호 검색의 제목 うたたね와 가수 Leina가 원본 차트 곡과 일치합니다. |
| `leina-うたたね` | KY | `unknown` → `57805` / confirm | 금영 공식 번호 검색의 제목 うたたね와 가수 Leina가 원본 차트 곡과 일치해 원본에 없던 금영 번호를 추가합니다. |
| `jpop-playlist-butter-fly-71e0adeb5b` | TJ | `81866` → `81866` / confirm | TJ 국내곡 번호 검색에서 Butter-Fly(디지몬어드벤처OST)와 전영호를 확인했습니다. 원본 곡명과 가수가 대응합니다. |
| `jpop-playlist-butter-fly-71e0adeb5b` | KY | `40554` → `51714` / confirm | 원본 금영 40554의 공식 가수는 和田光司였습니다. 전영호의 공식 Butter-fly 번호 51714를 확인해 이 곡의 유효 금영 번호를 교체합니다. |
| `jpop-playlist-和田光司-butter-fly-106e35d9b9` | TJ | `25589` → `25589` / confirm | TJ 공식 번호 검색의 Butter-Fly와 和田光司가 원본 곡과 대응하며 디지몬 OP 부제를 보존합니다. |
| `jpop-playlist-和田光司-butter-fly-106e35d9b9` | KY | `40554` → `40554` / confirm | 금영 공식 번호 검색의 Butter-Fly와 和田光司가 원본 곡과 대응하며 디지몬 OP 부제를 보존합니다. |
| `jpop-playlist-中島美嘉-3c88d9a0a8` | TJ | `62161` → `— (unknown)` / suppress | TJ 62161의 공식 제목은 MR 雪の華입니다. 원본의 한국어 제목 눈의 꽃만으로 MR 버전의 정체성을 확인하지 못해 번호 표시와 복사를 보류합니다. |
| `jpop-playlist-中島美嘉-3c88d9a0a8` | KY | `41637` → `— (unknown)` / suppress | 금영 공식 결과는 中島美嘉의 雪の華입니다. 원본의 한국어 제목 눈의 꽃과 별도 MR 관측 번호를 가진 행의 언어·버전 정체성을 확인하지 못해 번호 사용을 보류합니다. |
| `jpop-playlist-中島美嘉-雪の華-1aab071dd8` | TJ | `25627` → `25627` / confirm | TJ 공식 번호 검색의 제목 雪の華와 가수 中島美嘉가 원본 곡과 일치합니다. |
| `jpop-playlist-中島美嘉-雪の華-1aab071dd8` | KY | `41637` → `41637` / confirm | 금영 공식 번호 검색의 제목 雪の華와 가수 中島美嘉가 원본 곡과 일치합니다. |
| `jpop-playlist-supercell-ワ-ルト-イス-マイン-59499634d4` | TJ | `28285` → `28285` / confirm | TJ 공식 곡명과 supercell(Feat.初音ミク)가 원본 곡과 대응합니다. 공식 결과의 '60이상 반주기 전용곡' 기기 제한 표기를 함께 확인해 주세요. |
| `jpop-playlist-supercell-ワ-ルト-イス-マイン-59499634d4` | KY | `43011` → `43011` / confirm | 금영 공식 제목 ワールドイズマイン과 supercell feat.初音ミク가 원본 supercell 곡과 대응합니다. |
| `jpop-playlist-ワ-ルト-イス-マイン-d1f861df09` | TJ | `26941` → `— (unknown)` / suppress | 공식 결과는 World is Mine / 初音ミク입니다. 원본 가수가 보컬로이드라는 분류로만 기록돼 가수·녹음 정체성을 확정하지 못했으므로 번호 사용을 보류합니다. |
| `jpop-playlist-ワ-ルト-イス-マイン-d1f861df09` | KY | `43011` → `— (unknown)` / suppress | 공식 가수는 supercell feat.初音ミク입니다. 원본 가수가 보컬로이드라는 분류로만 기록돼 가수·녹음 정체성을 확정하지 못했으므로 번호 사용을 보류합니다. |
| `jpop-playlist-don-t-say-lazy-ca9960cc97` | KY | `43060` → `— (unknown)` / suppress | 공식 가수는 桜高軽音部입니다. 원본의 사쿠라고 경음부 표기와 별도 TJ 관측 번호를 가진 행의 가수·버전 정체성을 확인하지 못해 금영 번호 사용을 보류합니다. |
| `jpop-playlist-v-don-t-say-lazy-a5102a0589` | KY | `43060` → `— (unknown)` / suppress | 금영 43060의 공식 가수는 桜高軽音部입니다. V급밴드의 곡 또는 같은 녹음이라고 확인할 근거가 없어 이 곡의 번호 표시와 복사를 보류합니다. |
| `jpop-playlist-桜高軽音部-don-t-say-lazy-fb29186706` | TJ | `26908` → `26908` / confirm | TJ 공식 제목 Don’t say “lazy”와 가수 桜高軽音部가 원본 곡과 일치하며 케이온 ED 부제를 보존합니다. |
| `jpop-playlist-桜高軽音部-don-t-say-lazy-fb29186706` | KY | `43060` → `43060` / confirm | 금영 공식 제목과 桜高軽音部가 원본 곡과 대응합니다. 인용부호 표기 차이를 검토하고 공식 케이온 ED 부제를 보존합니다. |
| `jpop-playlist-song-a9ab46307a` | TJ | `84739` → `84739` / confirm | TJ 국내곡 번호 검색의 내 꿈은 파티시엘과 IU가 원본 제목·가수 아이유와 대응합니다. 공식 꿈빛파티시엘 OST 부제를 보존합니다. |
| `jpop-playlist-song-a9ab46307a` | KY | `44671` → `— (unknown)` / suppress | 금영 44671의 공식 결과는 夢にエール!パティシエール♪ / 五條真由美입니다. 아이유의 한국어 곡 번호로 확인할 수 없어 표시와 복사를 보류합니다. |
| `jpop-playlist-五條真由美-夢にエ-ル-ハ-ティシエ-ル-23b7562bb2` | TJ | `68414` → `68414` / confirm | TJ 공식 번호 검색의 夢にエール！パティシエール♪와 五條真由美가 원본 곡과 대응하며 애니메이션 OP 부제를 보존합니다. |
| `jpop-playlist-五條真由美-夢にエ-ル-ハ-ティシエ-ル-23b7562bb2` | KY | `44671` → `44671` / confirm | 금영 공식 결과의 주 제목 夢にエール!パティシエール♪와 가수 五條真由美가 원본 곡과 대응합니다. 실제 말줄임된 공식 부제를 임의로 복원하지 않습니다. |
| `jpop-playlist-ツミキ-フォニイ-a5e8ebf8d0` | TJ | `52537` → `52537` / confirm | TJ 공식 번호 검색의 제목 フォニイ와 가수 ツミキ(Feat.可不)가 원본 곡과 대응합니다. |
| `jpop-playlist-ツミキ-フォニイ-a5e8ebf8d0` | KY | `57803` → `57803` / confirm | 금영 공식 번호 검색의 제목이 포니와 일치하며 공식 가수는 ツミキ feat.可不로 표시됩니다. |
| `jpop-playlist-ナナヲアカリ-チュ-リンク-ラフ-feat-sou-0adf58fbdd` | TJ | `52536` → `52536` / confirm | TJ 공식 번호 검색에서 튜링 러브와 ナナヲアカリ(Feat.Sou)를 확인했습니다. 원본의 feat.Sou 표기와 대응합니다. |
| `jpop-playlist-ナナヲアカリ-チュ-リンク-ラフ-feat-sou-0adf58fbdd` | KY | `57803` → `— (unknown)` / suppress | 금영 57803의 공식 결과는 포니입니다. 튜링 러브의 번호로 확인할 수 없어 표시와 복사를 보류합니다. 금영 대체 번호는 미확인입니다. |
| `jpop-playlist-song-b781ffa754` | TJ | `44108` → `44108` / confirm | TJ 국내곡 번호 검색의 최종화 (The Last Flower)와 아이리 칸나가 원본 곡과 대응합니다. |
| `jpop-playlist-song-b781ffa754` | KY | `80603` → `80603` / confirm | 금영 공식 번호 검색의 최종화 (The Last Flower)와 아이리 칸나가 원본 곡과 대응합니다. |
| `jpop-playlist-最終花-5c6619182b` | KY | `80603` → `— (unknown)` / suppress | 금영 공식 제목은 최종화 (The Last Flower)입니다. 원본 最終花 표기가 번역 제목인지 다른 언어·녹음 버전인지 확인하지 못해 이 곡의 번호 사용을 보류합니다. |

## 실제 공식 재확인 요청 증거

지원되는 추가 네트워크 권한의 Python 표준 `urllib.request`·`HTMLParser`·`hashlib`로 실행했다. inherited proxy·CA·TLS 검증을 유지하고 동시 요청 최대 2개·timeout 25초를 사용했다. 전체 응답은 메모리에서만 읽었고 기록은 번호·제목·가수·HTTP·실제 시각·응답 hash로 제한했다. credentials·전체 환경·가사·외부 이미지·전체 웹 응답은 문서나 저장소로 복사하지 않았다.

TJ는 실제 결과 `span.num2`, `li.grid-item.title3`, `li.grid-item.singer`, KY는 같은 `tr`의 `td.search_col02`·`td.search_col03`에서 번호·제목·가수를 확인했다. 검색 입력값을 결과 행으로 오인하지 않았다. 최종 URL은 모든 요청 URL과 같았다. 해시는 실제 수신 응답 바이트이며 당시 본문 전체를 영속 보관하거나 재구성할 수 있다는 뜻은 아니다. 공식 GET deep link의 장기 지원을 API 계약으로 가정하지 않는다.

아래 완료 시각은 모두 2026-10-14 KST이며 HTTP는 28개 모두 200이다. 이 날짜를 검토하지 않은 전체 카탈로그의 확인일로 복사하지 않는다.

| 실제 요청 | 완료 시각 KST | 공식 결과 제목 / 가수 | 응답 SHA-256 |
| --- | --- | --- | --- |
| [TJ 25837](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=25837&strWord=Y) | 14:17:51.429368 | 丸の内サディスティック / 椎名林檎 | `f4cae9c5ad0ce252c22043329b5ca270bd1d99f84e6b04c3b50856d0b7598201` |
| [KY 40074](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=40074) | 14:17:53.656906 | 丸ノ内サディスティック / 椎名林檎 | `1537c5d01e3612286504625827b12d2faf2681738e5196a5cb8783d74371e9ba` |
| [TJ 27944](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y) | 14:17:53.032754 | 前前前世(Movie ver.)(映画'君の名は。' OST) / RADWIMPS | `5a75a77e73b82e95a1b431966d252a87e408978d143ba8726e9447eed90d04df` |
| [KY 44067](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44067) | 14:17:56.091445 | 前前前世(movie ver.)("君の名は。"OST) / RADWIMPS | `4c8ed0fa25309315462b9ea032a4721b068983c657ce74bc2aeda43bc05b2374` |
| [TJ 52756](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52756&strWord=Y) | 14:17:54.373164 | 拝啓, 少年よ('ベネッセ 進研ゼミ 高校講座' CM) / Hump Back | `3c15821b6215c0bcf9d8ec4eb2b61512352064fb8fc0cf8c574e603a9e47c0dc` |
| [KY 76548](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=76548) | 14:17:57.828211 | 拝啓、少年よ / Hump Back | `118110b1e8f38bc6dba1bce8a74704e8764e29fa1fd317b54a745f00ae0f5583` |
| [TJ 68961](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68961&strWord=Y) | 14:17:56.490564 | うたたね / Leina | `0da57b152e75e7f6b45f1024b3beedd9b4f3fbdb373ec19e80c232881056305c` |
| [KY 57805](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57805) | 14:17:59.278515 | うたたね / Leina | `5b57d9cde08597c7c4c4f69d32a7c943d20cf2e98bc9cce0d9de3e1c6d531acf` |
| [TJ 81866](https://www.tjmedia.com/song/accompaniment_search?nationType=KOR&strType=16&searchTxt=81866&strWord=Y) | 14:17:58.279840 | Butter-Fly(디지몬어드벤처OST) / 전영호 | `313ca4fcb19fd30f8f336c567e125131ce12f0dadec37672ba8b04a50922f81b` |
| [KY 51714](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=51714) | 14:18:00.106477 | Butter-fly(만화 "디지몬 어드벤처") / 전영호 | `a4303401cb1bcd33bf90022ec0a039a9d3992ec860e475567f4d6a5887c64cb3` |
| [TJ 25589](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=25589&strWord=Y) | 14:17:59.705309 | Butter-Fly(デジモンアドベンチャーOP) / 和田光司 | `867fda64f26819dec0705730075eb8d39a3799fbc5ef86eef10bfbd765ea0c9e` |
| [KY 40554](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=40554) | 14:18:02.242052 | Butter-Fly ("デジモンアドベンチャー"OP) / 和田光司 | `3f09841934dc33d34492be9625eaf1ff4e9817e9509975649562b1dad2c14c89` |
| [TJ 62161](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=62161&strWord=Y) | 14:18:00.724958 | MR 雪の華 / 中島美嘉 | `df392a563fc4c77fb0381dfdc8ac899c40f9ca6479a0b4ca71ba06a7803d419d` |
| [KY 41637](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=41637) | 14:18:02.677390 | 雪の華 / 中島美嘉 | `d31156c3c05a3e366ab5d429dbefab97a5569ba1c785b8c63d1cc8c86d40c12a` |
| [TJ 25627](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=25627&strWord=Y) | 14:18:02.717672 | 雪の華 / 中島美嘉 | `0399a473fb6fd081d6d749644017554e278076f232b1877d904b70d97a57d3f2` |
| [TJ 28285](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=28285&strWord=Y) | 14:18:03.286336 | 60이상 반주기 전용곡 ワールドイズマイン / supercell(Feat.初音ミク) | `b0e7670a6525ac7114e81f083d110cfb6a12cc3e735f3f790b1cd26e7978e3cf` |
| [KY 43011](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43011) | 14:18:04.525494 | ワールドイズマイン / supercell feat.初音ミク | `aaffb50f113e21dd51f0f6b8c0319af854f5af9d11b8536dee3a7eff2a804f3b` |
| [TJ 26941](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=26941&strWord=Y) | 14:18:03.610106 | World is Mine / 初音ミク | `153e6cc4d34b5917a59a4997d2c918983d200858d3f6937ee7ec593b3979b8b7` |
| [KY 43060](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43060) | 14:18:05.811871 | Don't say "lazy" ("けいおん!"ED) / 桜高軽音部 | `4b4b3d6197e76d06958c9a08e3253b10cd57e1ec6f92b69be03e5c0f3b0a036f` |
| [TJ 26908](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=26908&strWord=Y) | 14:18:04.848817 | Don’t say “lazy”(けいおん! ED) / 桜高軽音部 | `a696ac493c5554eecd15cc6ed6360d9a4d32fe82899be434e7e94f10392f6038` |
| [TJ 84739](https://www.tjmedia.com/song/accompaniment_search?nationType=KOR&strType=16&searchTxt=84739&strWord=Y) | 14:18:05.514502 | 내 꿈은 파티시엘(꿈빛파티시엘OST) / IU | `ee56f6cc50508c60956c7279936a42679f13191dd97010113224bd469a8c2b27` |
| [KY 44671](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44671) | 14:18:08.158581 | 夢にエール!パティシエール♪ ("夢色パティ.. / 五條真由美 | `bb37f840f889711b4d2bbfd947ab6afb6cd904a090e0477807a35f7b236c25ba` |
| [TJ 68414](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68414&strWord=Y) | 14:18:06.064389 | 夢にエール！パティシエール♪(夢色パティシエール OP) / 五條真由美 | `83f06f55f1c6ae3d12a51a1abebfeda22c463788a36f490d069dae6de4fdb7c8` |
| [TJ 52537](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52537&strWord=Y) | 14:18:06.322292 | フォニイ / ツミキ(Feat.可不) | `92f27f6dcd44a255906db7cb82cb37c7b00ac8578f1faf0a833788d589a9dc16` |
| [KY 57803](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57803) | 14:18:08.551976 | フォニイ / ツミキ feat.可不 | `9f9b9d6ee4951ad904be4344f836ada63fd0d89bf8d3d91309cb26a3b5b2b93c` |
| [TJ 52536](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52536&strWord=Y) | 14:18:08.573254 | チューリングラブ / ナナヲアカリ(Feat.Sou) | `07fa165f654134e3050d1f2bb50820fbe680cc28dcfb302db65f33034f097969` |
| [TJ 44108](https://www.tjmedia.com/song/accompaniment_search?nationType=KOR&strType=16&searchTxt=44108&strWord=Y) | 14:18:08.952055 | 최종화 (The Last Flower) / 아이리 칸나 | `280dd05cf857593c18ec937dbd16d744be2f9466aabc5da939e92e0e4a50cf5c` |
| [KY 80603](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=80603) | 14:18:10.481184 | 최종화 (The Last Flower) / 아이리 칸나 | `614577a6e76d1d677ce5abde935ea345364259515f0f68bd4c49a396530ac161` |

## 검증·차단·인계

| 실제 실행 | 결과와 범위 |
| --- | --- |
| `validateProviderCorrections(ledger, staging)` | 43entry 통과: 실제 staging revision·원본 expected 값·ID/provider 중복·번호 문자열·허용 HTTPS URL·실제 날짜·근거 필드 검사 |
| `applyProviderCorrections(buildPublicCatalog(staging).songs, staging, ledger)` | 3,317 ID 보존; confirm30/suppress13, 공식 확인 ID16. 원본 관측 바이트는 수정하지 않음 |
| `node --test --test-isolation=none tests/provider-corrections.test.mjs` | 실제 8/8 PASS: immutable ID/raw/metadata, suppress unknown·검색·복사 대상 제외, 추가번호·conflict, 공개 허용필드, revision/중복/번호/날짜/출처/private 필드 실패 검사 |
| 입력·공개 출력 별도 assertion | 아래 재개 실행에서 전체 stable ID 순서·원본 메타데이터 불변, suppress null, 내부 응답hash/refs·가사/이미지 제외, 허용 근거만 확인 |
| 실제 공식 내용 검토 | 28 요청 모두 numeric 결과 1행, ledger 43entry의 제목·가수·응답 hash와 일치. validator의 형식 통과를 인간 정체성 판단으로 대신하지 않음 |
| 내부 문서 링크 | 상대 링크 파일 존재 검사 완료. 원격 Issue/PR 상태와 앱 배포 확인은 root 담당 |
| 차단 | 공식 자료 조회·ledger 검증을 막는 차단 없음. 튜링 러브 KY 대체번호와 보류 8entry의 정체성 추가조사·전체 관리 목록·권리 검토는 미완료 범위 |
| 미실행 | 이 담당자는 Git·공용 코드·빌드·브라우저·CI·preview·운영 배포·Google 계정 심사를 실행하지 않음 |
| 동료 평가 | 별도 읽기 전용 평가 요청 전 점수를 생성하지 않음. 실제 두 평가와 통합 결과는 root의 기록으로 확인 |

공식 메타데이터를 조회한 사실은 자료 재배포 허락·콘텐츠 권리·광고 심사 승인과 별개다. 사용 목적은 해당 곡의 검색·번호 비교·정정 근거를 제공하는 최소 메타데이터이며, 가사·앨범/영상 이미지·난이도·음역·녹음 길이·추천·심사 결과는 추가하지 않았다. [편집 기준](../content/editorial-policy.md)과 [AdSense 단계](../operations/adsense.md)의 별도 검토를 따른다.

결과 SHA·PR·같은 SHA artifact 검증·preview·실제 jpop.today 운영 적용·복구는 root가 통합한 실제 실행 증거에 연결한다. 다음 담당자는 이 ledger의 정확한 checkedAt/hash를 포함한 최종 입력을 고정하고, suppress 번호가 상세/검색/즐겨찾기/차트에서 다시 복사되지 않는지 확인한다. 본 문서의 완료는 행별 자료·ledger 검증 완료이며 앱 배포 완료를 뜻하지 않는다.
