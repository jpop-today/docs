# 동결차트 20·22·24·25위 공식 번호 검토

`DATA-001` · `SEARCH-001` · `SONG-001` · `CONTENT-001` · [Issue #35](https://github.com/jpop-today/jpoptoday/issues/35) · [제품 계약](../product/spec.md) · [ADR-006](../adr/006-reviewed-corrections-and-production.md).

이 기록은 지정된 current/playlist 4ID의 기존 관측번호 TJ4·KY3에 대한 실제 공식 metadata-only 조회와 편집 검토다. 기존 77개 ledger 객체·순서·내용을 보존하고 7개 confirm만 끝에 추가했다. 전체 카탈로그 공식 확인·새 차트 수집·권리 허가·AdSense 승인을 뜻하지 않는다. 앱·빌드·CI·배포·복구·Release는 통합 담당자가 별도로 검증한다.

| 작업 계약 | 실제 범위 |
| --- | --- |
| 담당 | `/root/resume22_evidence`; root가 Git·공용 계약·CI·배포·보고를 관리 |
| run / branch | `resume-20261017-2200` / `codex/35-confirmed-search-20261017` |
| 기준 SHA | source `2964f7ea3f8c43f147422107f40629f51c010698`; docs `d679cf57592b285a30d762784fdb62ae0bf73581`; org `b7aad18fe2f793453b6505bb150f359d05ae3554` |
| 소유 파일 | source `data/reviewed/provider-corrections.json`; 이 문서; [요청·원후보·별도 판단·검증 JSON](provider-review-requests-20261017-2200.json) |
| 환경 | 실제 cloud revision443 current/running, unrestricted/enforced. networking·VPN false 확인 후 inherited proxy/CA와 TLS 검증을 유지 |
| 조회 도구 | 변경하지 않은 `scripts/provider-metadata.py`의 `fetch_metadata(provider, number, timeout=20)`를 정확히 7회 순차 호출 |

## 동결 입력과 판단 경계

원본 staging revision `6dbe5a65d1bd729c09393f746f715f4363089a43`의 3,317ID를 읽었다. 차트는 `fetchedAt: 2026-10-13T10:26:45.525Z`, 요청 기간 `2026-09-29`–`2026-10-13`, legacy revision `94d615f5e355f4b00ebbb824547ac6dfa034ad18`인 기존 100행 스냅샷이다. 차트의 stable canonicalId·rank·전체 제목/가수와 staging 원문·관측번호를 직접 대조했다. 아래 공식 checkedAt를 차트 수집일·새 순위로 바꾸지 않았다.

원후보의 `verification: unreviewed`를 그대로 보존하고 실제 파일을 읽은 편집 판단은 `editorialReview`에 별도로 기록했다. `validateProviderCorrections`의 shape/date/provenance 통과 자체를 곡의 제목·가수·버전 검토로 대신하지 않았다. 실제 원문 표시셀의 대소문자·구두점·부제·feature 표기를 아래에 생략 없이 적었다.

## 실제 요청과 확인 시각

첫 요청 시작 `2026-10-17T13:06:54.460740Z` (2026-10-17T22:06:54.460740+09:00); 마지막 조회 완료 `2026-10-17T13:07:09.838140Z` (2026-10-17T22:07:09.838140+09:00). 공식 요청 7회 모두 HTTP200·조회번호와 같은 단일행·정확한 요청/최종 URL 조건을 통과했다. 전송/파싱 실패0, 편집 confirm7·suppress0·defer0이다. 없는 Tani Yuuki KY번호는 조회하지도 추가하지도 않았다.

| 차트 / 업체 | 실제 공식 번호 | checkedAt (KST) | 응답 bytes / SHA-256 |
| --- | --- | --- | --- |
| 20 / TJ | [28214](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=28214&strWord=Y) | `2026-10-17T22:06:56.244122+09:00` | 49,213 / `93061854fb8fa0703b5b907f85765a698e339b8869b3cd9f4ea12cdc8e04eb0d` |
| 20 / KY | [43862](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43862) | `2026-10-17T22:06:59.502802+09:00` | 40,966 / `f65894a000bdf4bd15c41015c2c813a7a8fd641426ba5ec788fc74e20f75bff9` |
| 22 / TJ | [52444](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52444&strWord=Y) | `2026-10-17T22:07:01.038505+09:00` | 49,228 / `9d222b7aee0a98e15d182a1dafe4c883c29faddc2762fa76d5e1402cf549f761` |
| 24 / TJ | [68127](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68127&strWord=Y) | `2026-10-17T22:07:02.030760+09:00` | 49,282 / `2613ebe66de4ea373ba0fe65a30da260c8cd6c254c7c7d8dee64f5c35cad33ae` |
| 24 / KY | [44466](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=44466) | `2026-10-17T22:07:06.340055+09:00` | 46,420 / `0ff186fef867ac0263da512da962b2de640afe6225c1432afc35bd9d8489a9e4` |
| 25 / TJ | [52468](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52468&strWord=Y) | `2026-10-17T22:07:06.981424+09:00` | 49,247 / `43829489694925b5eef0ecd2e46b7be23ed9a4532fd9cad15ce7aa241c0e7124` |
| 25 / KY | [57766](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=57766) | `2026-10-17T22:07:09.837464+09:00` | 57,073 / `9d23064efa2f8142baa5cd1e6a3f0e7e3339a7881da85dd8e4892d9041be8b4e` |

편집 검토 시각은 `2026-10-17T13:08:15.816795Z` (2026-10-17T22:08:15.816795+09:00)다. helper의 실제 checkedAt와 구분한다. metadata helper SHA-256은 `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243`이며 baseline과 바이트 동일하다. 최대2MiB·strict UTF-8·timeout20초·공식 exact numeric URL/redirect 제한·단일 결과행 확인은 기존 helper 계약을 유지했다.

각 응답의 전체 bytes SHA-256·크기·HTTP상태·정확한 URL·전체 제목/가수 metadata만 보존한다. 원문 HTML·가사·credential 파일·전체 환경은 출력하거나 저장하지 않았다. 원응답을 남기지 않았으므로 hash만으로 원문을 독립 재파싱할 수 없다. 다른 담당자에게 원문을 재파싱했다고 주장하지 않으며 재조회는 별도의 시각/hash인 새 관측이다.

## 행별 편집 판단

### 동결 20위 · TJ `28214` · confirm

- 안정 ID: `jpop-playlist-kana-boon-シルエット-52bb3cbb6c`
- staging 전체 원문: `シルエット` / `KANA-BOON`
- 동결차트 전체 원문: `シルエット(NARUTO-ナルト- 16th OP)` / `KANA-BOON`
- 실제 공식 표시셀 전체: `シルエット(NARUTO-ナルト- 16th OP)` / `KANA-BOON`

공식 전체 제목 シルエット(NARUTO-ナルト- 16th OP)와 가수 KANA-BOON은 동결차트20위의 전체 원문과 일치하며 동일 canonicalId의 staging シルエット / KANA-BOON에 대응합니다. 공식 부제를 그대로 보존하며 오디오·반주기 모델의 동일성은 판단하지 않습니다.

### 동결 20위 · KY `43862` · confirm

- 안정 ID: `jpop-playlist-kana-boon-シルエット-52bb3cbb6c`
- staging 전체 원문: `シルエット` / `KANA-BOON`
- 동결차트 전체 원문: `シルエット(NARUTO-ナルト- 16th OP)` / `KANA-BOON`
- 실제 공식 표시셀 전체: `シルエット ("NARUTO-ナルト- 疾風伝"OP)` / `KANA-BOON`

공식 전체 제목 シルエット ("NARUTO-ナルト- 疾風伝"OP)와 가수 KANA-BOON을 직접 읽었습니다. staging 본제 シルエット 및 동결차트20위의 KANA-BOON·NARUTO OP 표기에 대응하며 다른 가수·편곡·버전 표시는 없습니다. 금영의 疾風伝 부제를 그대로 보존하고 생략되지 않은 셀에 없는 16th나 오디오 동일성을 추가하지 않습니다.

### 동결 22위 · TJ `52444` · confirm

- 안정 ID: `jpop-playlist-tani-yuuki-w-x-y-43434ad678`
- staging 전체 원문: `W / X / Y` / `Tani Yuuki`
- 동결차트 전체 원문: `W / X / Y` / `Tani Yuuki`
- 실제 공식 표시셀 전체: `W / X / Y` / `Tani Yuuki`

공식 전체 제목 W / X / Y와 가수 Tani Yuuki가 staging 및 동일 canonicalId의 동결차트22위 원문과 정확히 일치합니다. 원본 unknown인 금영 번호를 추정하거나 추가하지 않습니다.

### 동결 24위 · TJ `68127` · confirm

- 안정 ID: `jpop-playlist-mrs-green-apple-青と夏-89182753e8`
- staging 전체 원문: `青と夏` / `Mrs. GREEN APPLE`
- 동결차트 전체 원문: `青と夏(映画'青夏 きみに恋した30日' OST)` / `Mrs. Green Apple`
- 실제 공식 표시셀 전체: `青と夏(映画'青夏 きみに恋した30日' OST)` / `Mrs. Green Apple`

공식 전체 제목 青と夏(映画'青夏 きみに恋した30日' OST)와 가수 Mrs. Green Apple은 동결차트24위의 전체 원문과 정확히 일치합니다. staging의 青と夏 / Mrs. GREEN APPLE과는 가수의 대소문자 및 공식 영화 부제 표기 차이이며 원문 부제를 보존합니다. 별도 녹음·편곡 동일성을 추정하지 않습니다.

### 동결 24위 · KY `44466` · confirm

- 안정 ID: `jpop-playlist-mrs-green-apple-青と夏-89182753e8`
- staging 전체 원문: `青と夏` / `Mrs. GREEN APPLE`
- 동결차트 전체 원문: `青と夏(映画'青夏 きみに恋した30日' OST)` / `Mrs. Green Apple`
- 실제 공식 표시셀 전체: `青と夏 (映画"青夏 きみに恋した30日")` / `Mrs. GREEN APPLE`

공식 전체 제목 青と夏 (映画"青夏 きみに恋した30日")와 가수 Mrs. GREEN APPLE을 직접 읽었습니다. staging의 본제·가수와 일치하며 동결차트24위의 같은 영화 제목과도 대응합니다. 괄호 공백·인용부호와 OST 표기 유무의 차이를 검토하고 금영의 전체 표기를 그대로 보존하며 숨은 버전·오디오 동일성을 추정하지 않습니다.

### 동결 25위 · TJ `52468` · confirm

- 안정 ID: `jpop-playlist-deco-27-モニタリンク-ed5c141bdb`
- staging 전체 원문: `モニタリング` / `DECO*27`
- 동결차트 전체 원문: `モニタリング` / `DECO*27(Feat.初音ミク)`
- 실제 공식 표시셀 전체: `モニタリング` / `DECO*27(Feat.初音ミク)`

공식 전체 제목 モニタリング와 가수 DECO*27(Feat.初音ミク)는 동결차트25위의 전체 원문과 정확히 일치하며 staging의 モニタリング / DECO*27와 동일 canonicalId에 연결됩니다. 初音ミク feature 명의는 실제 공식 셀 및 차트의 동일한 전체 명의를 근거로 그대로 보존하며 별칭·권리를 추정하지 않습니다.

### 동결 25위 · KY `57766` · confirm

- 안정 ID: `jpop-playlist-deco-27-モニタリンク-ed5c141bdb`
- staging 전체 원문: `モニタリング` / `DECO*27`
- 동결차트 전체 원문: `モニタリング` / `DECO*27(Feat.初音ミク)`
- 실제 공식 표시셀 전체: `モニタリング` / `DECO*27 feat.初音ミク`

공식 전체 제목 モニタリング와 가수 DECO*27 feat.初音ミク를 직접 읽었습니다. staging 본제·DECO*27 및 동결차트25위의 DECO*27(Feat.初音ミク)와 본제·주명의·feature 명의가 대응하며 차이는 괄호와 Feat. 대소문자 표기입니다. 공식 전체 명의를 보존하고 다른 가수의 별칭·녹음 동일성은 생성하지 않습니다.

위 확인은 제공사 번호 검색의 실제 제목·가수·기재된 버전 표기가 이 stable ID의 원본과 대응한다는 범위다. 오디오를 청취하거나 모든 반주기 모델·지역 제공·편곡/녹음 동일성·재배포 권리를 확인한 것이 아니다. KY43862에 없는 `16th`를 금영 표시 제목에 붙이지 않고, KY44466에 없는 `OST`를 재구성하지 않았으며, DECO*27의 feature 명의는 실제 셀과 동결차트에 명시된 初音ミク를 보존했다.

## 원본 보존과 실제 검증

검증 시각 `2026-10-17T13:09:35.583Z` (2026-10-17T22:09:35.583000+09:00). 독립 Python wrapper가 `git show <base>:<path>`로 시작 ledger와 동결 파일 바이트를 가져왔고, Node의 실제 `validateProviderCorrections`/`buildPublicCatalog`/`applyProviderCorrections`로 전후를 비교했다. ledger는 **77→84, confirm64→71, suppress13 그대로**다. 첫77 객체·키/값·순서·최상위 metadata를 deep-equal로, 원래 배열/객체 닫기 이전까지의 raw byte prefix를 exact 비교했다. 새7개는 전부 같은 관측 문자열 번호·provider-confirmed·conflict:false다.

| 실제 검증 | 결과 |
| --- | --- |
| 3,317 stable ID·`songHref` 전체 | 전후 전체 배열 일치 |
| 대상 외 3,313 공개 곡 객체 | 전체 deep-equal |
| 모든 곡의 제목·가수·태그·origin·availability | 전체 deep-equal |
| 대상 provider 7개 외 공개 karaoke 객체 | 전체 deep-equal |
| Tani Yuuki KY | 기존 unknown/null/evidence:null/correction:null 그대로 |
| 기존 SAM/samfree KY43185·Ado KY76509 | observed-unverified/같은 관측 문자열/evidence:null 그대로; 추가 조회·재판정 없음 |
| 공개 payload | responseSha256 미포함·unlisted 생성 없음 |
| staging/chart/identity baseline/helper/global CSS | base SHA의 파일과 bytes 동일 |

| 입력 / 결과 파일 | SHA-256 |
| --- | --- |
| 기존 ledger77 bytes | `a2d8ea8e9eba737a38a1f2968a0a1687498bf04fb0f8742ea7f42eedaef8f708` |
| 결과 ledger84 bytes | `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1` |
| `data/staging/legacy-catalog.json` | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/chart-snapshot.json` | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| `data/legacy-identity-baseline.json` | `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8` |
| `scripts/provider-metadata.py` | `96e471901adf0b07675c8ea610c6a5c143b16bd4f3d8ebea707c55e162cc7243` |
| `src/styles/global.css` | `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889` |

원본 current/playlist는 3,299곡이며 공식 근거·충돌없음의 기존 `confirmedNumbers`를 적용한 업체별 안전 대상은 아래와 같다. 59개 번호는 59곡이 아니고, 차트는 여전히 동일한 100행이다. 차트 안의 current/playlist86개 중 안전 대상을 따로 셌으며 chart-only 행을 이 검토에 승격하지 않았다.

| 범위 | 이전 TJ / KY / 양쪽 / 합집합 | 이후 TJ / KY / 양쪽 / 합집합 |
| --- | --- | --- |
| current/playlist 전체 | 28 / 24 / 24 / 28곡 | 32 / 27 / 27 / 32곡 |
| 동결차트에 속한 current/playlist | 19 / 17 / 17 / 19곡 | 23 / 20 / 20 / 23곡 |

실제 `node --test --test-isolation=none tests/provider-corrections.test.mjs`는 **8/8 PASS·실패0·46.190788ms**였다. 이는 기존 합성 계약 회귀이며 실제7행 정체성 검토를 대신하지 않는다. 독립 실자료 검증은 현재 dirty-worktree의 공용 계약을 사용했고, 해당 파일 hash를 요청 JSON에 보존했다. root의 공용 계약 수정과 clean-SHA CI·빌드·브라우저·배포 결과를 혼동하지 않는다.

첫 독립 Node 시도는 `execFileSync(git show)`가 sandbox `spawnSync git EPERM`으로 실패해 데이터 assertion 전에 중단됐다. 이 실제 실패를 JSON `verificationAttempts`에 보존했다. 재시도는 Python에서 read-only Git 바이트를 가져와 Node stdin으로 전달하여 같은 독립 assertion을 실제 통과했다. 이를 제품 데이터 오류나 네트워크 실패로 바꾸어 설명하지 않았다.

## 미확인 상태와 인계

이 담당은 Git 변경·패키지/lockfile·공용 계약·CI·서버·배포·Release를 수행하지 않았다. 전체 unit/type·two-target build/reproducibility·artifact/browser·같은SHA CI/actual preview·실제 old복구/new재적용·운영/Release는 root가 수행하고 실제 증거로 수용해야 한다. 현재 내부 근거문서와 ledger append만으로 운영에 반영됐다고 주장하지 않는다.

AdSense는 준비 중이며 실제 계정·신청/심사·승인 상태는 unknown이다. 공식 공개 번호 조회는 가사·이미지·음원·metadata 재배포 허락이 아니다. 수집/부분/쓰기 실패의 마지막 정상 자료 보존 계약은 그대로 유지했고 실제 광고 요청·자동클릭을 만들지 않았다. 

root에 전달할 결과는 7confirm·기존77/raw prefix 보존·Tani KYunknown/SAM/AdoKYdeferred불변·전후 전체/차트 고유곡 수·실제 후보/검토시각이다. 다른 두 담당에 대한 점수는 실제 파일과 검증 근거를 읽은 후 root 요청에 따라 별도로 기록하며 선투표하지 않는다.

## 10월18일 재개 인계의 독립 파일 검증

`/root/resume35_evidence`가 `2026-10-17T18:10:05.972965Z`(10월18일03:10:05.972965KST)에 이 문서·요청 JSON·현재 ledger와 base `2964f7e`의 실제 Git 바이트를 다시 대조했다. 원래 공급자 조회·검토자는 `/root/resume22_evidence`이며 이번 담당은 공급자 요청을 0회 수행했다. 기존 후보의 checkedAt·응답 hash·`unreviewed`와 별도 편집 판단 시각/주체를 그대로 보존했다. 원응답을 다시 파싱하거나 원래 요청을 자신이 수행했다고 주장하지 않는다.

7개의 후보→별도 판단→ledger77..83의 전체 제목/가수·번호문자열·공식 URL·응답 hash·시각·이유를 각각 대조했고 일치했다. 기존77 객체/순서/최상위 metadata와 raw byte prefix, 동결 staging/chart/identity/helper/global CSS 바이트도 base와 동일했다. 현재 ledger의 raw SHA-256은 위 `dbef4ae6…0356cc1` 그대로이며 소스 파일을 수정하지 않았다. 현재 공용 계약의 실제 `validateProviderCorrections`/`applyProviderCorrections`를 이용한 독립 전후 검사는 3,317ID·URL, 3,313 비대상 객체, Tani KY unknown/null, SAM/Ado KY 보류 및 위 전후 공식 확인곡 수를 모두 통과했다. 전체 테스트·타입·브라우저·clean SHA/CI·배포 검증을 대신하지 않는다.

첫 인계 검사 스크립트는 기록의 `frozenChart`가 원차트의 legacy `id`를 포함하지 않는 5필드 projection임을 빠뜨려 객체 전체 동일성 assertion에서 실패했다. 실제 보존된 5필드(rank/canonicalId/title/artist/tjNumber)를 정확히 비교하도록 검사 스크립트를 수정한 뒤 전체 검사를 통과했으며, 제품 데이터나 원래 기록은 바꾸지 않았다. 이 실패/재검증 경계와 결과는 요청 JSON의 `handoffReview`에 보존했다.

재개 환경에서 확인한 예전 임시 경로 `/tmp/jpop-20261017-dirty-unit-boolean-fix.log`, `/tmp/jpop35-app-qa/report.json`, `/tmp/jpop-resume22-quality/targeted-fixed.txt`는 없었다. 이들의 과거 성공은 영속 기록의 과거 관측으로만 남기며 현재 실행 결과로 재사용하지 않는다. 원래 run 이름과 조회일은 유지하고, 재개 검증 날짜만 별도로 기록한다.
