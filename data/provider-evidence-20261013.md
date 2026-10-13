# TJ/KY 공식 번호 대조 기록 — 2026-10-13

## 작업 계약과 결과

`LOCAL-DATA-EVIDENCE-20261013` · `DATA-001` · 관련 [원본 이관 Issue #3](https://github.com/jpop-today/jpoptoday/issues/3), [미리보기 Issue #5](https://github.com/jpop-today/jpoptoday/issues/5).

| 항목 | 범위 |
| --- | --- |
| 담당 | 현재 위임 작업의 `data_evidence` 하위 에이전트. Git·통합·배포는 상위 담당자 |
| 기준 소스 SHA | `33e782cb2a49e6df82da5f747d2ad5ca1cd3e01f` |
| 기준 문서 SHA | `ba8368edc28de170173e581684a637041305ecde` |
| 원본 운영 revision | `6dbe5a65d1bd729c09393f746f715f4363089a43` |
| 변경 파일 | 이 문서만. 원본·staging·카탈로그 계약·의존성·공용 파일 변경 없음 |
| 조사일 | 실제 요청·행 대조를 수행한 `2026-10-13`, 한국 표준시(KST, UTC+09:00) |
| 완료 범위 | staging 공유 번호 11그룹의 23개 ID를 읽고, 각 번호의 공식 검색 결과를 대조. HTTP 응답·번호·제목·가수·요청 경로·해시 기록 |
| 미완료 범위 | 전체 3,317개 ID 검증, 권리 검토, ID alias 확정, 데이터 수정·v1 승격·운영 배포 |

11개 공유 번호 모두 공식 검색에서 해당 번호의 결과 행을 확인했다. 번호가 존재한다는 사실과 모든 staging ID가 그 번호의 곡이라는 판단은 다르다. 현재 공식 응답은 TJ `27944`의 버전, KY `40554`·`44671`·`57803`의 곡 또는 가수에 원본 관측과 충돌을 보여준다. KY `43060`의 가수 표기도 추가 정체성 검토가 필요하다. 이번 문서는 원본 번호를 수정하거나 공유 ID를 합치지 않는다.

이 조사일은 아래에 기록된 요청과 대조의 실제 날짜다. 전체 카탈로그의 `checkedAt`을 이 날짜로 채우지 않는다. 데이터 공개·승격 기준은 [카탈로그 계약](catalog-contract.md), 원본 보존 기준은 [staging 계약](legacy-staging.md), 관측값 미리보기는 [ADR-004](../adr/004-observed-preview-read-model.md)를 따른다.

## 실제 사용할 수 있는 공식 출처

### TJ

[TJ 공식 사이트맵](https://www.tjmedia.com/sitemap)의 반주곡 검색 링크가 [현재 검색 화면](https://www.tjmedia.com/song/accompaniment)을 가리킨다. 실제 HTML form과 검색 함수에서 `GET /song/accompaniment_search`, `nationType=JPN`, `strType=16`(곡 번호), `searchTxt=<번호>`, `strWord=Y`(단일검색)를 확인했다. 아래 4개 번호 요청은 모두 HTTP 200과 곡 결과 1행을 반환했다.

현재 경로를 페이지에서 읽어 사용했다. 추정 경로 `/song/search`는 웹 도구가 접근하지 못했고 번호 근거로 사용하지 않았다. 검색에 나타난 예전 모바일 도메인의 2023년 가을호 애니송북 PDF는 실제 열기에서 404여서 증거로 채택하지 않았다. 검색 색인의 제목·요약만으로 번호를 확정하지 않는다.

### KY

[금영 공식 홈페이지](https://www.kyentertainment.kr/)의 반주곡 검색 링크는 [K VOICE 검색 사이트](https://kygabang.com/shop/)로 연결된다. 해당 사이트 HTML form은 `POST /chart/search_list.php`, `mode=SongSearch`, `val=<검색어>`를 사용한다. 공식 홈페이지의 링크와 서비스 페이지의 운영 법인 표시를 확인한 경로이며, 제삼자 번호 모음을 근거로 쓰지 않았다.

7개 공유 번호는 POST 요청으로 각각 곡번호 결과 1행을 반환했다. 동일 파라미터를 GET query로 전달하는 번호별 URL도 별도 요청했다. 7개 모두 HTTP 200이며 해당 POST 응답과 바이트 SHA-256이 같았다. 따라서 아래 표에는 실제 작동을 확인한 번호별 GET 링크를 둔다. 이 GET 지원을 장기 API 계약으로 가정하지 않는다.

공식 홈페이지의 다른 링크인 `https://kysing.kr/`는 HTTP 200이지만 757바이트의 JavaScript cookie challenge만 반환했다. 실제 곡 결과를 얻지 못했으며 이를 우회하지 않았다. 기존 키싱 경로의 접근 실패와 현재 K VOICE 검색의 성공을 구분한다.

### 실행 환경과 차단 구분

| 시도 | 관찰 | 결론 |
| --- | --- | --- |
| 기본 shell sandbox에서 TJ·금영·K VOICE·키싱 URL 요청 | 모두 `Operation not permitted` | 실행 sandbox의 네트워크 제한. 제공사 차단이나 미수록 근거가 아님 |
| 지원되는 추가 네트워크 권한을 지정한 동일 공개 URL 요청 | 모두 HTTP 200. 기존 proxy·TLS 검증 유지 | 네트워크 접근 성공. credentials나 환경 값을 출력하지 않음 |
| TJ 4개 번호·KY 7개 번호 검색 | 모두 해당 번호·제목·가수 행 확인 | 아래 번호 수준의 공식 대조 성공 |
| KY 7개 번호의 GET 재확인 | POST와 응답 해시 동일 | 아래 deep link의 실제 작동 확인 |
| 키싱 HTTP 200 | JavaScript challenge만 수신 | 곡 조회 미완료. HTTP 성공만으로 내용 확인을 주장하지 않음 |
| 일반 웹 검색의 일부 번호 질의 | 정확한 공식 곡 결과를 찾지 못함 | 검색 색인 실패이며 `unlisted` 근거가 아님 |

클라우드 런타임은 연결 상태를 보고했지만 네트워크 정책 readiness는 `unknown`이었다. 설정값으로 성공을 추정하지 않고 위 실제 요청 결과를 증거로 삼았다.

## 공유 번호 11그룹 대조

표의 staging 표기는 보존된 원본 메타데이터다. 공식 표기는 실제 검색 결과 행의 번호·제목·가수다. 같은 번호로 검색된 서로 다른 ID의 동일성은 아직 확정하지 않았다. `《レコおと》`, 영화·애니 부제, 한국어/일본어 표기는 버전이나 언어를 뜻할 수 있으므로 무조건 제거하거나 alias로 합치지 않는다.

| 제공사 / 번호 / 공식 근거 | staging의 공유 표기 | 공식 검색에서 확인한 제목 / 가수 | 검토 판단과 후속 작업 |
| --- | --- | --- | --- |
| [TJ 25837](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=25837&strWord=Y) | `丸ノ内サディスティック` / `丸の内サディスティック`, 모두 椎名林檎 | `丸の内サディスティック` / 椎名林檎 | 표기 차이 후보. 양쪽 ID·URL·즐겨찾기를 보존한 alias 검토 필요 |
| [TJ 27944](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y) | `前前前世 [original ver.]` / `前前前世(Movie ver.)(映画'君の名は。' OST)`, 모두 RADWIMPS | `前前前世(Movie ver.)(映画'君の名は。' OST)` / RADWIMPS | 공식 번호의 movie 표기와 playlist의 original 표기가 충돌. original ID를 movie ID에 자동 합치지 않음 |
| [TJ 52756](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52756&strWord=Y) | `拝啓、少年よ` / `拝啓, 少年よ('ベネッセ 進研ゼミ 高校講座' CM)`, 모두 Hump Back | `拝啓, 少年よ('ベネッセ 進研ゼミ 高校講座' CM)` / Hump Back | 구두점·부제 차이 후보. 버전 차이 없음은 별도 확인 필요 |
| [TJ 68961](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68961&strWord=Y) | `うたたね《レコおと》` / `うたたね`, 모두 Leina | `うたたね` / Leina | 공식 검색은 《レコおと》를 확인해 주지 않음. 영상/녹음 버전 의미를 추정해 공개 설명으로 확정하지 않음 |
| [KY 40554](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=40554) | `Butter-Fly` / 전영호, `Butter-Fly` / 和田光司 | `Butter-Fly ("デジモンアドベンチャー"OP)` / 和田光司 | 전영호 ID의 관측 번호와 현재 공식 가수 충돌. 전영호의 별도 번호 후보 51714를 아래에서 확인 |
| [KY 41637](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=41637) | `눈의 꽃` / `雪の華`, 모두 中島美嘉 | `雪の華` / 中島美嘉 | 번역 제목 후보지만 제목만으로 동일 녹음/언어를 확정하지 않음. 각 ID의 TJ 번호도 다르므로 제공사별 대조 필요 |
| [KY 43011](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43011) | `ワールドイズマイン` / supercell, 같은 제목 / 보컬로이드 | `ワールドイズマイン` / supercell feat.初音ミク | 창작자·보컬 표기 후보. 장르명 `보컬로이드`를 아티스트 동일성 근거로 사용하지 않음 |
| [KY 43060](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43060) | `Don’t say “lazy”` / 사쿠라고 경음부, `Don't say "lazy"` / V급밴드, 같은 제목 / 桜高軽音部 | `Don't say "lazy" ("けいおん!"ED)` / 桜高軽音部 | 경음부 한글/일본어 표기는 alias 후보. V급밴드 표기의 정체성·커버 여부는 별도 검토. 3개 ID를 일괄 병합하지 않음 |
| [KY 44671](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44671) | `내 꿈은 파티시엘` / 아이유, `夢にエール！パティシエール♪` / 五條真由美 | `夢にエール!パティシエール♪ ("夢色パティ..` / 五條真由美 | 아이유 ID와 현재 공식 가수/제목 충돌. 공식 행 제목은 실제로 말줄임되어 있어 부제 전체를 복원하지 않음 |
| [KY 57803](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57803) | `フォニイ` / ツミキ, `チューリングラブ feat.Sou` / ナナヲアカリ | `フォニイ` / ツミキ feat.可不 | 서로 다른 제목·가수. 튜링 러브 ID에 이 번호를 확인된 번호로 적용할 수 없음 |
| [KY 80603](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=80603) | `최종화` / `最終花`, 모두 아이리 칸나 | `최종화 (The Last Flower)` / 아이리 칸나 | 가수 동일·다른 언어 제목 후보. 같은 곡 또는 다른 언어 버전인지는 추가 근거 필요 |

### 충돌에 대한 추가 공식 조회

KY의 `val=Butter-Fly` 제목 검색은 `40554`(和田光司)와 `51714`(전영호)의 2개 번호 행을 반환했다. 이어 [KY 51714 번호 검색](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=51714)을 실제 GET 요청해 `Butter-fly(만화 "디지몬 어드벤처") / 전영호`를 확인했다. 이는 전영호 행의 수정 후보 근거다. 이번 작업에서 staging의 `40554`를 교체하지 않았고, 원본의 다른 제공사 번호·언어·버전을 검토하기 전 동일 ID의 전체 정체성이 확정된 것은 아니다.

TJ의 `nationType=JPN&strType=1&searchTxt=前前前世` 제목 검색은 movie 표기의 `27944` 1행을 반환했다. original 버전의 미수록을 증명하지 않는다. KY의 제목 검색 `val=チューリングラブ`, `val=내 꿈은 파티시엘`은 이 요청에서 번호/제목 결과 행을 추출하지 못했다. 검색어 변형·제공사 표기·검색 동작을 추가 확인해야 하며 `unlisted`로 변환하지 않는다.

## 원본 행과 안정 ID 연결

조사 입력은 `jpoptoday/data/staging/legacy-catalog.json`이다. 바이트 SHA-256은 `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3`이며, `unresolvedAliases` 11그룹이 아래 23개 ID를 참조한다. 이 해시는 공식 번호의 진위를 증명하지 않고 조사한 staging을 고정한다.

playlist의 행은 원본 `data/jpop-playlist-karaoke-tj-ky-merged.txt`의 물리 행이며, chart의 index는 staging `sources[2]`가 가리키는 운영 revision의 `js/chart.js` 0 기반 index다. 제목·artist 대조에는 raw/generated refs도 사용 가능하지만, 아래 위치만으로 기존 ID를 정확히 찾을 수 있다.

| 그룹 | 기존 ID | 원본 위치 |
| --- | --- | --- |
| TJ 25837 | `jpop-playlist-椎名林檎-丸ノ内サテ-ィスティック-da51e27aca` | TSV 행 1164 |
| TJ 25837 | `椎名林檎-丸の内サテ-ィスティック` | chart index 36 |
| TJ 27944 | `jpop-playlist-radwimps-前前前世-original-ver-1ff3eb38b9` | TSV 행 2400 |
| TJ 27944 | `radwimps-前前前世-movie-ver-映画-君の名は-ost` | chart index 69 |
| TJ 52756 | `hump-back-拝啓-少年よ-ヘ-ネッセ-進研セ-ミ-高校講座-cm` | chart index 27 |
| TJ 52756 | `jpop-playlist-hump-back-拝啓-少年よ-29d81a757e` | TSV 행 1991 |
| TJ 68961 | `jpop-playlist-leina-うたたね-レコおと-0edbc5afc9` | TSV 행 2113 |
| TJ 68961 | `leina-うたたね` | chart index 62 |
| KY 40554 | `jpop-playlist-butter-fly-71e0adeb5b` | TSV 행 983 |
| KY 40554 | `jpop-playlist-和田光司-butter-fly-106e35d9b9` | TSV 행 1328 |
| KY 41637 | `jpop-playlist-中島美嘉-3c88d9a0a8` | TSV 행 1072 |
| KY 41637 | `jpop-playlist-中島美嘉-雪の華-1aab071dd8` | TSV 행 1071 |
| KY 43011 | `jpop-playlist-supercell-ワ-ルト-イス-マイン-59499634d4` | TSV 행 2555 |
| KY 43011 | `jpop-playlist-ワ-ルト-イス-マイン-d1f861df09` | TSV 행 453 |
| KY 43060 | `jpop-playlist-don-t-say-lazy-ca9960cc97` | TSV 행 532 |
| KY 43060 | `jpop-playlist-v-don-t-say-lazy-a5102a0589` | TSV 행 2671 |
| KY 43060 | `jpop-playlist-桜高軽音部-don-t-say-lazy-fb29186706` | TSV 행 1361 |
| KY 44671 | `jpop-playlist-song-a9ab46307a` | TSV 행 781 |
| KY 44671 | `jpop-playlist-五條真由美-夢にエ-ル-ハ-ティシエ-ル-23b7562bb2` | TSV 행 830 |
| KY 57803 | `jpop-playlist-ツミキ-フォニイ-a5e8ebf8d0` | TSV 행 3103 |
| KY 57803 | `jpop-playlist-ナナヲアカリ-チュ-リンク-ラフ-feat-sou-0adf58fbdd` | TSV 행 3136 |
| KY 80603 | `jpop-playlist-song-b781ffa754` | TSV 행 778 |
| KY 80603 | `jpop-playlist-最終花-5c6619182b` | TSV 행 773 |

## 응답 증거와 재현

해시는 실제 수신한 전체 응답 바이트 기준이다. KY는 같은 번호의 POST·GET 해시가 일치했고 아래 시각은 GET 재확인 시각이다. TJ 시각은 번호 GET 시각이다. 날짜는 모두 `2026-10-13` KST다. 다른 시점에는 차트·사이트 변경으로 해시가 달라질 수 있으므로 해시 불일치만으로 곡 번호가 바뀌었다고 판단하지 않는다.

| 요청 | 실제 시각(KST) | HTTP | SHA-256 |
| --- | --- | --- | --- |
| TJ 25837 | 14:08:37 | 200 | `f4cae9c5ad0ce252c22043329b5ca270bd1d99f84e6b04c3b50856d0b7598201` |
| TJ 27944 | 14:08:36 | 200 | `5a75a77e73b82e95a1b431966d252a87e408978d143ba8726e9447eed90d04df` |
| TJ 52756 | 14:08:37 | 200 | `3c15821b6215c0bcf9d8ec4eb2b61512352064fb8fc0cf8c574e603a9e47c0dc` |
| TJ 68961 | 14:08:36 | 200 | `0da57b152e75e7f6b45f1024b3beedd9b4f3fbdb373ec19e80c232881056305c` |
| KY 40554 | 14:10:11 | 200 | `3f09841934dc33d34492be9625eaf1ff4e9817e9509975649562b1dad2c14c89` |
| KY 41637 | 14:10:10 | 200 | `d31156c3c05a3e366ab5d429dbefab97a5569ba1c785b8c63d1cc8c86d40c12a` |
| KY 43011 | 14:10:10 | 200 | `aaffb50f113e21dd51f0f6b8c0319af854f5af9d11b8536dee3a7eff2a804f3b` |
| KY 43060 | 14:10:12 | 200 | `4b4b3d6197e76d06958c9a08e3253b10cd57e1ec6f92b69be03e5c0f3b0a036f` |
| KY 44671 | 14:10:12 | 200 | `bb37f840f889711b4d2bbfd947ab6afb6cd904a090e0477807a35f7b236c25ba` |
| KY 57803 | 14:08:44 | 200 | `9f9b9d6ee4951ad904be4344f836ada63fd0d89bf8d3d91309cb26a3b5b2b93c` |
| KY 80603 | 14:10:12 | 200 | `614577a6e76d1d677ce5abde935ea345364259515f0f68bd4c49a396530ac161` |
| KY 51714 수정 후보 | 14:10:56 | 200 | `a4303401cb1bcd33bf90022ec0a039a9d3992ec860e475567f4d6a5887c64cb3` |

추가 제목 조회의 POST/GET 요청과 실제 응답 해시:

| 요청 | 시각(KST) | HTTP / 관찰 | SHA-256 |
| --- | --- | --- | --- |
| KY POST `mode=SongSearch&val=Butter-Fly` | 14:09:40 | 200 / 40554·51714 2행 | `a7ae07ab98cab94587fde61f28acd7251715a39a8b8137662ae69d2bf986d445` |
| KY POST `mode=SongSearch&val=チューリングラブ` | 14:09:39 | 200 / 번호·제목 행 추출 0 | `01b1c0d1bb86b6e62a8bfe3a03806d5edea0e39377b2cf6a1eb1f15d83078938` |
| KY POST `mode=SongSearch&val=내 꿈은 파티시엘` | 14:09:39 | 200 / 번호·제목 행 추출 0 | `3ae0d763f9583c5f0128243afd2a4cddf2ae014a7115f8484f8b0121602bfe57` |
| TJ GET `nationType=JPN&strType=1&searchTxt=前前前世` | 14:09:40 | 200 / 27944 movie 표기 1행 | `2f7ee8b3f3416e75c7752d6bc3fc6856d5c70c300d2b5167be797b0e0cb2a783` |

공개 조회는 Python 3 표준 라이브러리 `urllib.request`·`urllib.parse`로 실행했다. 인증·로그인·개인 정보가 필요하지 않았다. 최대 동시 요청은 3개였고 HTML의 번호·제목·가수 영역만 결과로 기록했다. 공식 페이지 응답에는 가사가 포함될 수 있어 전체 HTML을 저장소나 공개 artifact에 복사하지 않았다. 조사용 원문은 `/tmp/data-evidence-20261013/`의 일시 파일이며 영속 감사 자료로 보장하지 않는다. 이 문서의 메타데이터·해시·요청은 남지만, 당시 원문 전체를 저장소에서 재구성할 수 있다는 뜻은 아니다.

같은 네트워크 권한이 허용된 shell에서 다음처럼 재현할 수 있다. 출력 파일은 저장소 밖에 두고 가사 본문을 로그에 출력하지 않는다.

```sh
curl --fail --show-error --location --max-time 25 \
  --user-agent 'Mozilla/5.0' \
  --output /tmp/tj-27944.html \
  'https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y'

curl --fail --show-error --location --max-time 25 \
  --user-agent 'Mozilla/5.0' \
  --data-urlencode 'mode=SongSearch' \
  --data-urlencode 'val=57803' \
  --output /tmp/ky-57803.html \
  'https://kygabang.com/chart/search_list.php'

sha256sum /tmp/tj-27944.html /tmp/ky-57803.html
```

번호와 검색 입력값이 같다는 검사만으로 통과시키면 안 된다. TJ 결과의 `span.num2`, `li.grid-item.title3`, `li.grid-item.title4.singer`와 KY의 `td.search_col02`, `td.search_col03`에서 실제 결과 행을 확인한다. 검색어 input에 번호가 반영된 것과 결과 행에 번호가 있는 것을 구분한다. HTTP 200이어도 challenge·에러 HTML·빈 결과일 수 있다.

## 후속 큐와 승격 제약

| 순서 / 로컬 작업 | 담당과 실행 내용 | 완료 증거 |
| --- | --- | --- |
| 1 / `DATA-001-COLLISION` | 데이터 담당: TJ27944, KY40554·44671·57803, KY43060 V급밴드 ID의 버전·언어·가수 검토. KY51714를 전영호 수정 후보로 검토 | ID별 관측값·공식값·판단 사유·번호/메타데이터 변경 제안. 원본 관측 보존 |
| 2 / `DATA-001-ALIAS` | 데이터 + 호환 담당: 나머지 표기 후보와 1번 해결 결과의 canonical/alias 여부 결정 | 모든 기존 ID·URL·즐겨찾기 키를 보존하는 명시적 매핑. 무손실 검증 |
| 3 / `DATA-001-EVIDENCE` | 데이터 담당: 번호별 HTTPS 근거, 실제 대조 날짜, 요청 파라미터·행 메타데이터·수집 상태·검토 대상 ID를 영속 검토 artifact로 구조화 | 네트워크 실패/빈 응답/다른 곡 응답을 분리한 증거. 가사·비밀 값 제외 |
| 4 / `DATA-001-FULL` | 데이터 담당: 공유 그룹 밖 전체 행의 공식 대조·제공사/모델 적용 범위·권리 검토 | 검토 완료 범위·미완료 범위·실제 권리 근거·변화 보고. 부분 결과가 전체 snapshot을 덮지 않음 |
| 5 / 공개 승격 | 통합 담당: 확정된 정체성과 공식 증거를 [v1 계약](catalog-contract.md)의 후보/change gate로 검증 | 실제 전체 범위 후보·검증 로그·기존 snapshot 보존·복구 증거 |

번호별 공식 페이지가 존재해도 여러 ID에 같은 `registered` 번호를 붙이면 v1의 번호 유일성 계약과 충돌한다. alias 결정이나 버전 분리 없이 이 문서의 날짜/URL만 복사해 승격하지 않는다. 공식 검색의 현재 표기만으로 과거 staging이 어떻게 수집됐는지나 당시 번호 상태를 소급 확정하지 않는다.

[ADR-004](../adr/004-observed-preview-read-model.md)의 관측값 미리보기에서도 이 그룹은 공유 번호 경고·공식 재확인 경로를 제공하고, 제목/버전/가수 충돌을 확인 완료로 표시하지 않아야 한다. 이 문서는 문서 작성·읽기 전용 대조 결과이며 운영 번호 수정이나 배포 완료 기록이 아니다.

검증 범위는 문서의 내부 링크 실재 여부, 11그룹/23개 ID/원본 refs의 staging 재대조, 실제 응답의 결과 영역과 기록 메타데이터·해시의 일치다. 앱 테스트·v1 승격·브라우저 미리보기·운영 배포는 이 변경에서 실행하지 않았다.
