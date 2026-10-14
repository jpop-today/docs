# TJ/KY 충돌 행 공식 재확인 — 2026-10-14

## 작업 계약과 확인 범위

`LOCAL-DATA-REVIEW-20261014` · `DATA-001` · `CONTENT-001` · `ADS-001`. 위임받은 연계 작업은 [공식 정정 Issue #6](https://github.com/jpop-today/jpoptoday/issues/6), [일일 운영 Issue #9](https://github.com/jpop-today/jpoptoday/issues/9)다.

| 항목 | 이번 작업 |
| --- | --- |
| 담당 / 통합 | `daily_20261014_evidence` / root. Git·package·workflow·통합·배포는 root 담당 |
| 소유 범위 | 이 새 문서 하나. 원본·staging·번호·ID·소스 코드·공개 overlay를 수정하지 않음 |
| 공유받은 문서 기준 | `jpop-today/docs`, `codex/daily-development-20261013`, `5c3faaca5a77d3fd16cef865db98c68a7c204eed` |
| 공유받은 소스 기준 | `jpop-today/jpoptoday` main `2beb1bf02ec2aa67161cd16e77ed96b18bbf580e` |
| 공유받은 기존 운영 기준 | `kim75503/todayjpop` dev `6dbe5a65d1bd729c09393f746f715f4363089a43`. 운영 API·배포 상태를 이 담당자가 다시 검증한 것은 아님 |
| 직접 읽은 입력 | `/tmp/jpop-original-audit`의 원본 4파일, 소스의 staging·identity baseline, [전일 공식 근거](provider-evidence-20261013.md) |
| 실제 공식 조회 | `2026-10-14 09:04:33–09:07:30 KST`(UTC+09:00). 아래 응답 표는 요청 완료 시각 |
| 완료 범위 | TJ `27944`·KY `57803` 공유 2그룹의 4개 stable ID와 원본 행 대조. 공식 번호 4개 및 RADWIMPS 버전 자료 확인 |
| 미완료 범위 | KY 튜링 러브 대체 번호, original 버전의 제공사 번호, 전체 3,317 ID 검증, 권리 허락, 공개 후보 승격·운영 정정·AdSense 심사 |

이번 공식 응답에서 KY `57803`은 포니(フォニイ), TJ `27944`와 KY `44067`은 전전전세의 영화 버전이었다. `57803`을 튜링 러브의 확인 번호로 쓰거나 `27944`·`44067`을 original 버전의 확인 번호로 쓰는 것은 이 근거와 충돌한다. 결과는 행별 정정 제안이며 적용 완료가 아니다. 상태·출처·확인일은 [제품 스펙](../product/spec.md), [카탈로그 계약](catalog-contract.md), 원본 보존은 [staging 계약](legacy-staging.md)과 [ADR-003](../adr/003-evidence-and-publication.md)을 따른다.

## 공식 결과와 원본-ID 대조

모든 번호는 문자열 그대로 읽었다. TSV는 물리 행, raw/generated/chart index는 0부터 시작한다. 원본의 `lyricsText`·`lyrics`·이미지 값은 출력하거나 이 문서로 복사하지 않았다.

| stable ID | 원본 메타데이터·관측 번호 | 이번 공식 대조 | 정정 제안과 남은 판단 |
| --- | --- | --- | --- |
| `jpop-playlist-ツミキ-フォニイ-a5e8ebf8d0` | TSV 3103행, raw·songs index 2248: `フォニイ` / `ツミキ`, TJ `52537`, KY `57803`. chart index 79는 같은 ID·제목, 가수 `ツミキ(Feat.可不)` | [KY 57803](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57803): `フォニイ` / `ツミキ feat.可不`, 결과 1행 | KY 번호와 제목이 대응한다. 검토 후보의 제공사 가수 표기에 `feat.可不`를 보존한다. 원본 가수 관측값은 그대로 남긴다. 이 작업에서 TJ `52537`은 공식 재조회하지 않음 |
| `jpop-playlist-ナナヲアカリ-チュ-リンク-ラフ-feat-sou-0adf58fbdd` | TSV 3136행, raw·songs index 2282: `チューリングラブ feat.Sou` / `ナナヲアカリ`, TJ `52536`, KY `57803` | 같은 [KY 57803](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57803)은 다른 곡·가수. [TJ 52536](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52536&strWord=Y)은 `チューリングラブ` / `ナナヲアカリ(Feat.Sou)`, 결과 1행 | 검토 후보에서는 KY `57803`을 이 ID의 확인 번호로 채택하지 않는다. KY 대체 번호는 미확인으로 보류하고 원본 관측값은 staging에 보존한다. TJ 번호는 이 ID의 제목·가수와 대응하는 부분 근거 확보 |
| `jpop-playlist-radwimps-前前前世-original-ver-1ff3eb38b9` | TSV 2400행, raw·songs index 1060: `前前前世 [original ver.]` / `RADWIMPS`, TJ `27944`, KY `44067` | [TJ 27944](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y): `前前前世(Movie ver.)(映画'君の名は。' OST)` / `RADWIMPS`. [KY 44067](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44067): `前前前世(movie ver.)("君の名は。"OST)` / `RADWIMPS`. 각각 1행 | 양쪽 번호 모두 원본의 original 표기와 충돌한다. 이 ID를 original 곡으로 유지하는 후보에서는 두 번호의 확인 완료 처리를 보류한다. 영화 버전으로 제목을 바꾸거나 movie ID의 alias로 합치려면 기존 ID가 의도한 곡의 정체성 검토가 먼저 필요 |
| `radwimps-前前前世-movie-ver-映画-君の名は-ost` | 원본 chart index 69: `前前前世(Movie ver.)(映画'君の名は。' OST)` / `RADWIMPS`, TJ `27944`, KY 빈 문자열. staging은 chart-only·KY unknown | [TJ 27944](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27944&strWord=Y)와 원본 제목·가수가 대응. [KY 44067](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44067)도 영화 버전 표기 | TJ 근거를 이 movie ID에 연결하는 후보와 KY `44067` 추가 후보를 검토할 수 있다. KY는 이 원본 행에 있던 관측값이 아니므로 원본에 있던 번호처럼 기록하지 않는다. original ID와의 병합은 보류 |

4개 ID는 실제 원본 raw/generated 또는 chart의 `id`와 staging·identity baseline에서 일치했다. TSV의 제목·가수·TJ·KY tuple은 위 raw 행과 일치했다. ID 기준선은 행 관측을 연결하는 근거이며 공식 번호 진위나 녹음 동일성의 근거가 아니다. 정정 시 ID를 제목·가수·번호로 재계산하지 않고 기존 `/song?id=`·URL·즐겨찾기 키를 보존해야 한다([ADR-002](../adr/002-stable-identity.md)).

### 전전전세의 버전 판단

RADWIMPS 공식 [『人間開花』 앨범 페이지](https://radwimps.jp/music/8678/)는 발매일 `2016.11.23`, 5번 트랙을 `前前前世 [original ver.]`로 표시한다. 공식 [『君の名は。』 앨범 페이지](https://radwimps.jp/music/8451/)는 발매일 `2016.08.24`, 8번 트랙을 `前前前世 (movie ver.)`로 표시한다. 공식 [2016.08.30 공지](https://radwimps.jp/news/8644/)는 영화에 맞춘 구성과 original 앨범을 위한 구성을 구분한다. 세 페이지를 웹 도구로 실제 열고, 이후 shell의 HTTP 200 응답에서도 해당 메타데이터를 대조했다.

이 자료는 original/movie를 단순 구두점 차이로 지워서는 안 된다는 근거다. 제공사가 실제 재생하는 반주의 오디오·길이·키를 청취한 것은 아니며, 공식 검색의 movie 표기에서 그 이상의 녹음 세부사항을 추정하지 않는다. original ID를 영화 버전으로 재정의하거나 두 ID를 합치는 결정은 아직 내리지 않았다.

### 튜링 러브의 KY 대체 번호

공식 검색의 UTF-8 GET `mode=SongSearch&val=チューリング`은 결과 번호 셀 0개였다. `val=ナナヲアカリ`는 물리 결과 행 3개를 반환했으며, 번호·표기를 중복 제거하면 `44776` / `ダダダダ天使` / `ナナヲアカリ`, `44775` / `ディスコミュ星人(2022 ver.)` / `ナナヲアカリ`의 2개다. 어느 행도 튜링 러브를 확인해 주지 않았다. 이 두 번호를 튜링 러브의 대체 번호로 제안하지 않는다.

제공사 검색어·색인·표기·페이지 범위를 모두 확인한 것은 아니다. 0행 또는 다른 곡만 나온 결과를 `unlisted`로 바꾸지 않고 KY 대체 번호는 unknown으로 남긴다. 전일의 전체 제목 검색 실패와 이번 부분 제목·가수 조회를 구분한다.

## 관측 입력 고정

원본 4파일의 실제 바이트 해시가 staging `sources`에 기록된 dev revision의 해시와 모두 일치했다. 원본 JS는 JSON 배열 literal만 파싱했으며 실행·eval하지 않았다. 원본 체크아웃의 Git branch/HEAD 확인과 Git 조작은 이 담당자가 수행하지 않았다.

| 실제 읽은 파일 | SHA-256 |
| --- | --- |
| `/tmp/jpop-original-audit/data/jpop-playlist-karaoke-tj-ky-merged.txt` | `24e2f97e082b932414bb6e9c46cee67b2ed258283f82fa544af934a410d18ce0` |
| `/tmp/jpop-original-audit/data/jpop-playlist.raw.json` | `1805bbcb21397c23b21a4f761f75db7b7cceb1072fcccf7746ee5df5fea6d3cf` |
| `/tmp/jpop-original-audit/js/chart.js` | `5722774c7047f6314d2726389692f09578111f4752296001881c4d1a2eeab50e` |
| `/tmp/jpop-original-audit/js/songs.js` | `9a024c4259f74001215a36c61e0b1cf37199a6fcb57d9b0cfd596c2c842fd9c9` |
| `/workspace/jpoptoday/data/staging/legacy-catalog.json` | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |

원본에서 가사·이미지 필드의 존재는 확인했으나 내용과 외부 이미지 주소를 기록하지 않았다. 위 해시는 입력 고정용이며 자료 이용 허락을 뜻하지 않는다. 공개 콘텐츠의 권리·편집 검토는 [편집 기준](../content/editorial-policy.md)을 적용한다.

## 요청·행 근거와 실제 접근 상태

KY 공식 [홈페이지](https://www.kyentertainment.kr/)를 실제 읽어 `반주곡 검색`이 `https://kygabang.com/shop/`으로 연결되는 것을 확인했다. KY 번호 결과 화면에서 실제 form `method=post`, `action=/chart/search_list.php`, `mode=SongSearch`, `val`을 읽었다. 번호 `57803`은 GET·POST 모두 같은 결과 1행과 같은 응답 해시였다. GET deep link의 장기 지원을 API 계약으로 가정하지 않는다.

TJ 공식 [사이트맵](https://www.tjmedia.com/sitemap)을 실제 읽어 `반주곡 검색`이 `/song/accompaniment`로 연결됨을 확인했다. 번호 GET은 `nationType=JPN&strType=16&searchTxt=<번호>&strWord=Y`, 제목 GET은 `strType=1&searchTxt=前前前世`였다. 제목 조회도 movie 표기의 `27944` 1행만 반환했으나 original 버전 미수록의 증명은 아니다.

| 시도 | 실제 관찰 | 기록상 판단 |
| --- | --- | --- |
| callable 도구 목록 검색 | 인터넷 도구 `search_service_web_run`과 환경 상태 도구가 있음 | 도구가 없는 것으로 가정하지 않고 실제 사용 |
| 웹 도구로 KY57803·TJ27944 URL open | 두 URL 모두 `not accessible via this tool` | 웹 도구의 접근 실패. 공식 결과·미수록 여부를 확인한 응답 아님 |
| 기본 shell에서 KY57803 GET, `09:03:57 KST` | `URLError: [Errno 1] Operation not permitted` | 실행 sandbox 차단. 제공사 차단 또는 `unlisted`의 근거가 아님 |
| 환경 상태 / 비밀 값 없는 네트워크 정책 확인 | `observations_current:true`, HTTP `unrestricted` / `enforced`, `vpn_configured:false` | 런타임 설정과 실제 요청 성공을 구분. 자격 증명 값 출력 없음 |
| 지원되는 추가 네트워크 권한으로 공개 HTTPS 재요청 | 아래 모든 요청 HTTP 200, 결과 셀·트랙 메타데이터 확인 | inherited proxy·CA·TLS 검증 유지. 직접 라우트·challenge 우회 없음 |
| Python 패키지 확인 | `requests`·`bs4` 미설치 | 설치·의존성 수정 없이 표준 `urllib.request`·`urllib.parse`·`HTMLParser` 사용 |

아래 SHA-256은 수신한 응답 바이트로 계산했다. 전체 응답은 메모리에서 읽고 번호·제목·가수·선별 메타데이터만 출력했으며 저장소·공개 artifact·임시 파일로 전체 HTML을 저장하지 않았다. 응답 해시만으로 당시 원문 전체를 재구성할 수는 없다. 해시가 전일과 같은 번호 응답도 이번 실제 요청 시각을 따로 남겼으며, 전체 카탈로그의 `checkedAt`으로 복사하지 않는다.

| 요청 / 파라미터 | 완료 시각 KST (2026-10-14) | HTTP / 확인한 결과 | 응답 SHA-256 |
| --- | --- | --- | --- |
| KY GET `mode=SongSearch&val=57803` | 09:04:37.230 | 200 / `57803`, 포니 결과 1행 | `9f9b9d6ee4951ad904be4344f836ada63fd0d89bf8d3d91309cb26a3b5b2b93c` |
| TJ GET `nationType=JPN&strType=16&searchTxt=27944&strWord=Y` | 09:04:35.202 | 200 / `27944`, movie 결과 1행 | `5a75a77e73b82e95a1b431966d252a87e408978d143ba8726e9447eed90d04df` |
| KY 공식 홈페이지 GET | 09:04:36.776 | 200 / 공식 검색 서비스 링크 | `27842cdaeb496b3e54bf4347e264508337839359eb03cc3eec67864a9c0d3dd1` |
| TJ sitemap GET | 09:04:37.527 | 200 / 공식 반주곡 검색 링크 | `503d56a25a68a9f67353984a6830931bb4ddba25c7189a25944f26b148171923` |
| KY GET `mode=SongSearch&val=チューリング` | 09:06:15.063 | 200 / 번호 결과 셀 0개 | `69eab12ade41413cbd288fe0705e5ceda3c349b07d9b04ed97cf9033a28775d7` |
| KY GET `mode=SongSearch&val=ナナヲアカリ` | 09:06:15.876 | 200 / 물리 3행, 중복 제거 번호 2개, 튜링 러브 근거 없음 | `c714af92009831ae1bef08b376cdb4c8e6c6f0374edb186d74a5e694ded47573` |
| KY GET `mode=SongSearch&val=44067` | 09:06:18.922 | 200 / `44067`, movie 결과 1행 | `4c8ed0fa25309315462b9ea032a4721b068983c657ce74bc2aeda43bc05b2374` |
| TJ GET `nationType=JPN&strType=16&searchTxt=52536&strWord=Y` | 09:06:17.346 | 200 / `52536`, 튜링 러브 결과 1행 | `07fa165f654134e3050d1f2bb50820fbe680cc28dcfb302db65f33034f097969` |
| TJ GET `nationType=JPN&strType=1&searchTxt=前前前世&strWord=Y` | 09:06:18.044 | 200 / `27944`, movie 결과 1행 | `a3a851e7d7e102463567d15feaec5c24336f9475bb04a5eee651d0a0f02d4b06` |
| RADWIMPS `/music/8678/` GET | 09:07:27.960 | 200 / original 트랙·발매일 | `b2f800a993df507f49454374646fbe8030d5180aa195b43859956d7980ab84c6` |
| RADWIMPS `/music/8451/` GET | 09:07:28.085 | 200 / movie 트랙·발매일 | `ba8dd0c2566bf4c10b025c026d9d090eae819dc98275bd35d7e9017cd56d66f6` |
| RADWIMPS `/news/8644/` GET | 09:07:28.941 | 200 / 두 버전의 구성 차이 설명 | `f983a34fefee3a2cbae60d100b2169546f2a8998549366600e9a4cb091a743d9` |
| KY POST `/chart/search_list.php`, `mode=SongSearch&val=57803` | 09:07:30.188 | 200 / GET과 같은 번호·메타데이터·해시 | `9f9b9d6ee4951ad904be4344f836ada63fd0d89bf8d3d91309cb26a3b5b2b93c` |

검색어의 일본어 부분은 `urllib.parse.urlencode`로 UTF-8 percent encoding했다. 최종 URL은 요청 URL과 같았다. 결과 검사는 TJ `span.num2`, `li.grid-item.title3`, `li.grid-item.title4.singer`, KY `td.search_col02`와 같은 행의 `td.search_col03`에 한정했다. input에 검색 번호가 반영된 것과 결과 번호 셀을 구분했다. 이번 KY 응답의 제목·가수는 하나의 `search_col03` 셀에 `제목 / 가수`로 표시된다.

## 다음 정정 후보와 콘텐츠 사용 범위

1. 포니 KY `57803`과 튜링 러브 TJ `52536`, movie ID의 TJ `27944`은 행별 검토 후보 근거로 전달한다. provider 표시 제목·가수와 확인 URL·실제 확인일을 별도 근거 레코드에 연결한다.
2. 튜링 러브 KY `57803`은 다른 곡 응답이라는 반증을 기록하고 대체 번호를 더 조사한다. v1 후보에서는 unknown이 필요할 수 있지만 원본 staging의 관측 번호를 지워 계약을 통과시키지 않는다.
3. original ID의 TJ `27944`·KY `44067`은 movie 표기 충돌을 별도 기록한다. 원곡 정체성·번호를 추가 확인하고 두 버전의 ID를 보존한다. movie ID의 KY `44067` 추가는 새 검토 제안으로만 남긴다.
4. 위 제안을 적용하려면 실제 ID 전체를 유지한 후보·번호 유일성·변경 게이트·이전 URL/저장 복구를 통합 담당자가 확인한다. 이 4행 부분 근거를 전체 검증 snapshot으로 승격하지 않는다.

`CONTENT-001`에 사용할 수 있는 확인된 소재는 “같은 곡 제목이어도 original/movie 버전 표기를 확인해야 한다”, “번호 공유 후보에는 실제로 다른 곡이 포함될 수 있다”, “검색 0행은 미수록 확정이 아니다”라는 행별 비교다. 이 문서는 내부 검토 문서이며 공개 가이드 작성·reviewed 발행 완료를 뜻하지 않는다. 가이드에는 위처럼 제한된 비교와 출처를 자체 문장으로 설명하고 공식 본문·가사·앨범 이미지·썸네일을 복제하지 않는다. 음역·난이도·추천·반주 음질·권리 상태는 이번 조사에서 확인하지 않았다.

`ADS-001`에는 데이터 정확성의 부분 증거로만 연결한다. 공개 메타데이터를 읽은 사실은 재배포 허락이나 광고 심사 승인과 별개다. [AdSense 기준](../operations/adsense.md)의 실제 계정 상태·소유권·권리·광고·동의·크롤링 검토는 남아 있다. 조직 preview는 noindex·광고 없음이라는 공유 기준이며, 기존 운영 정정이나 오늘 reviewed overlay 승격이 완료됐다고 주장하지 않는다.

## 검증·미실행·인계

| 항목 | 이번 실행 상태 |
| --- | --- |
| 원본 행·ID·번호 대조 | 실제 원본 파일 해시와 대상 TSV/raw/generated/chart 4 ID 대조 완료. staging·identity baseline과 일치 |
| 공식 외부 근거 링크 | 표에 연결한 제공사 번호·홈페이지·사이트맵·RADWIMPS 3페이지 실제 HTTP 200과 관련 메타데이터 확인. KY 제목/가수 조회는 행 확보 여부를 따로 기록 |
| 내부 문서 링크 | `2026-10-14 09:10:16 KST`, `python3` / `pathlib`·Markdown 링크 추출로 상대 경로 8개 모두 실제 파일 존재 확인 |
| GitHub Issue 링크 | 상위 담당자가 제공한 작업 연결. 이 담당자의 Issue API·HTTP 재조회는 미실행 |
| 앱 테스트·빌드·브라우저 | 문서·읽기 전용 조사 범위라 미실행. UI/하네스 담당의 결과와 분리 |
| 데이터 쓰기·공개 v1/overlay 승격 | 미실행. 부분 근거·미해결 충돌·정체성 검토를 보존 |
| CI·preview·운영 배포·AdSense 계정 | 이 담당자 미실행. root의 실제 실행 기록 또는 Google 결과가 필요 |
| 동료 평가 | 완성된 동료 산출물의 읽기 전용 평가를 요청받기 전에는 점수를 만들지 않음 |

실행 명령은 shell의 `python3 - <<'PY'`였다. 원본 조사에는 `hashlib`·`json`·`csv`·정적 JS JSON literal 파싱을, 네트워크 조사에는 `urllib.request`·`urllib.parse`·`HTMLParser`를 사용했다. 공개 요청의 동시 실행은 최대 2개, timeout은 25초였다. 마지막 문서 검사에서 위 8개 내부 링크, 2그룹/4 stable ID의 문서 포함, staging SHA-256 불변, 해당 제공사 상태 `observed-unverified` 보존 assertion이 모두 통과했다. 이 assertion은 이미 확인한 조사 범위·입력 보존만 검사하며 앱 회귀 테스트가 아니다.

최종 변경 SHA·PR·배포 상태는 root가 통합 기록에 남긴다. 다음 담당자는 이 문서의 4개 stable ID·공식 결과 행을 입력으로 별도 후보를 만들고, 원본을 보존한 정정·호환성 검사를 수행한다. 현재 공식 조회 자체를 막는 차단은 추가 네트워크 권한을 통해 해소됐으며, KY 대체 번호와 original 번호/ID 정체성 판단은 자료 미확인 상태로 남아 있다.
