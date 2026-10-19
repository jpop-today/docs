# 충돌 번호 보류의 독립 회귀 인계

- 작업: [Issue41](https://github.com/jpop-today/jpoptoday/issues/41), `manual-20261019-0933`.
- 스펙: [충돌 번호의 화면·개별 복사 보류](../product/spec.md#충돌-번호의-화면개별-복사-보류--issue41-2026-10-19), DATA-001/SONG-001/FAVORITES-001/CONTENT-001/DESIGN-001.
- 담당: `/root/run41_quality`; root가 Git·공유 타입·package/lockfile·CI·서버·브라우저·배포를 소유한다.
- source base `1fd6ece7afb4ea7ebddf318f3ef6fa9dafa57951`, docs base `ea1b6cac498ada996b5ed6dc6a001d0270a37f98`; 공유 브랜치 `codex/41-conflicted-number-controls`.
- 쓰기 범위: source `e2e/conflicted-numbers.spec.ts` 신규 및 이 문서. 별도 unit 파일은 만들지 않았다. 신규 UI 분기를 문자열로 검사하는 unit보다, 실제 HTTP 공개자료와 렌더·동작을 독립 원자료 오라클에 대조하는 회귀가 이 변경의 수용 기준에 맞는다. 기존 strict 판정 계약은 변경하지 않는다.

## 독립 원자료 오라클

기대값은 앱 표시·확인·bulk formatter·projection helper를 import하지 않고, frozen staging의 각 업체 값에 ledger의 명시 action/number를 적용한 뒤 전체 3317 ID의 업체별 중복 빈도를 새로 계산한다. 공식 UI 기대 조건에는 current/playlist·confirm action·중복 없음이 필요하다. 현재 동결 ledger의 근거 URL·checkedAt·title·artist는 HTTP `/catalog.json`의 공개 근거와 모두 정확히 대조한다. 이 방법은 새로운 실제 제공사 조회나 제목·버전 심사에 해당하지 않는다. 실제 ledger 검증·엄격 numeric URL 검사는 기존 계약 하네스가 계속 맡는다.

읽기 계산에서 실제 충돌은 4 ID/8업체였다. 모두 confirm 조회 이력이 있으나 전체 중복 계산 결과 conflict true다. Hump Back만 검사하면 나머지 실제 충돌을 놓친다.

| 원자료의 ID | TJ / 금영 문자열 | 차트 순위 |
| --- | --- | --- |
| `hump-back-拝啓-少年よ-ヘ-ネッセ-進研セ-ミ-高校講座-cm` | `52756` / `76548` | 28 |
| `jpop-playlist-hump-back-拝啓-少年よ-29d81a757e` | `52756` / `76548` | — |
| `jpop-playlist-椎名林檎-丸ノ内サテ-ィスティック-da51e27aca` | `25837` / `40074` | — |
| `椎名林檎-丸の内サテ-ィスティック` | `25837` / `40074` | 37 |

번호는 원자료·공식 대조 이력에 계속 보존한다. 제품 셀과 복사 동작에서 보류할 대상이라는 뜻이며 번호 삭제나 ID 병합을 뜻하지 않는다. 대조군은 Pretender의 엄격 공식 확인, 원자료의 충돌 없는 관측값, SAM의 TJ 공식 확인/금영 관측 혼합, 前前前世(original ver.)의 두 업체 suppress, 미검토 unknown 업체를 가진 실제 playlist 곡이다. 실제 동결 자료에 한 업체만 충돌인 곡은 없다. 이 부재를 시험했다고 만들지 않고, 모든 업체를 각각 독립적으로 검사한다.

## 새 E2E 5개

1. staging/ledger/chart 파일 hash와 3317/84/4/8·차트28/37을 확인하고 실제 공개 catalog 전체의 ID·원제·가수·tags·availability·origin 및 6634 업체 상태/원번호/충돌/공개 근거/정정 사유를 원자료와 대조한다. 원자료에만 있는 `legacy`/`refs`를 공개 필드라고 오인하지 않는다.
2. 360px·JavaScript off에서 home 5행, chart 100행의 모든 업체, 실제 conflict 4곡 상세를 검사한다. 충돌 셀에는 숫자·data-copy·개별 복사·공식 배지가 없고 충돌/재확인 문구가 있어야 한다. 정상 셀의 문자열 복사값·엄격 공식 배지와 stable URL도 대조한다. 상세의 기존 공식 링크·확인일·사유·제보는 남아야 한다. chart-only confirmed 이력이 있는 곡을 playlist 공식 확인으로 확대하지 않는다. 전체 정적 행 검사만 최대 120초를 허용한다.
3. 실제 두 가수 검색 결과와 9곡 favorites에서 8개 conflict 셀을 전부 검사하고, SAM의 TJ/KY 개별 복사는 각각 원문 문자열을 clipboard mock으로 전달하는지 확인한다. bulk 결과에서도 conflict 4곡은 TJ/금영 `확인 필요`이고 충돌 번호는 없어야 한다. 저장값에는 object의 중첩 속성·선행 0·공백·unresolved ID가 들어 있으며 읽기·복사 후 원문 byte 문자열 그대로 남아야 한다.
4. 실제 정상/관측/혼합/suppress/unknown 상세, 기존 `/song.html?id=...` 리다이렉트·canonical URL, 360px 키보드 Enter/Space 복사 거부 fallback·readonly 전체 선택·Escape/닫기 후 focus 복귀·44px control·overflow·브랜드와 배경 palette를 검사한다. clipboard mock은 운영체제 clipboard 권한의 실제 승인을 뜻하지 않는다.
5. 실제 target metadata에 따라 home/chart/search/favorites/충돌 4상세의 HTTP robots/sitemap 경계와 preview 전체 noindex를 확인한다. 같은 경로를 브라우저로 열어 알려진 광고 domain 요청이 없는지 기록한다. 운영은 검색/favorites/충돌 상세 noindex이며 정상 페이지를 preview와 혼동하지 않는다.

기존 E2E를 삭제하거나 감점 회피용으로 변경하지 않았다. 전체 배포 게이트·재현·복구는 root의 기존 하네스와 연결한다. screenshot 옵션이 주어지면 `conflicted-chart-js-off-mobile.png`, `conflicted-detail-js-off-mobile.png`를 남긴다. screenshot 생성·시각 검토는 실제 root 실행 후에만 완료로 기록한다.

## 수행한 검사와 보류

2026-10-19 KST에 세 저장소 AGENTS, Issue41 실제 GitHub OPEN body/base/범위, spec Issue41 절, source 원자료·ledger·계약·템플릿/동적 생성·기존 E2E/config를 읽었다. `git status`/diff는 읽기만 실행했다. `gh issue view 41`는 도구 network 권한이 있는 읽기로 성공했다. Node fs/crypto의 독립 중복 집계는 4 ID/8업체·chart28/37을 산출했다. TypeScript `transpileModule(... reportDiagnostics:true)`는 새 파일 syntax diagnostics 0을 반환했다. 이것은 semantic type 검사·E2E 실행 성공이 아니다.

초기 파일 읽기에서 존재하지 않는 `contracts/provider-policy.mjs`를 cat한 명령은 exit 1이었고 실제 `provider-lookup.mjs`·`public-catalog.mjs`로 경로를 바로잡았다. 초기 대조 fixture 후보로 raw의 양쪽 unknown playlist 곡을 찾았으나 실제 자료에는 없어, 실제 한 업체 unknown 곡을 선택했다. 원자료가 가진 내부 `legacy`/`refs`와 공개 metadata allowlist의 차이를 확인해 전체 공개 metadata 비교는 명시 allowlist로 작성했다. 이 두 준비 교정은 브라우저 실행 결과가 아니다.

root에게 다음 기존 회귀의 실제 전제 충돌을 알렸고 소유 범위 밖 파일은 고치지 않았다.

- `e2e/title-artist-guide.spec.ts:77`의 날짜 고정 루프는 `tj-ky-numbers`도 10월 14일 편집으로 가정한다. 이번 관련 글의 실제 19일 편집과 나머지 5글의 불변 날짜를 구분해야 한다.
- `e2e/app.spec.ts`의 reviewed correction 회귀는 ledger의 첫 confirm(椎名林檎/TJ25837)을 copy 가능으로 가정한다. 실제 conflict true이므로 정상 strict 공식 확인 대조를 고르되 기존 suppression·공식 evidence 검사는 보존해야 한다.

담당자가 unit/type/build/artifact/server/browser/CI/preview/recovery/production/Release/Discord를 직접 실행한 것은 없다. root 초기 브라우저의 sandbox 실패도 통과로 바꾸지 않는다. 실제 결과와 최종 SHA는 [관리자 실행 기록](../operations/runs/manual-20261019-0933.json)을 읽어 확인한다. AdSense 계정·권리·문의 수신·ads.txt 일치·실제 신청/심사는 미확인이다.

## 읽은 동결 자료 hash

| 파일 | SHA256 |
| --- | --- |
| `data/staging/legacy-catalog.json` | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/reviewed/provider-corrections.json` | `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1` |
| `data/chart-snapshot.json` | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| `data/legacy-identity-baseline.json` | `5d882010993879420ac30bfcbbb1fcc3fe7e87a5bd00a56e5c379f41000aaec8` |
| `src/styles/global.css` | `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889` |

담당 범위 이후 다른 두 담당의 산출물을 실제 읽어 네 기준으로 평가하고 root에 구조화된 메시지로 전달한다. 여섯 평가나 팀 순위는 해당 실제 근거 수집·관리자 검증 전 완료로 주장하지 않는다.
