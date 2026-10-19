# 공식 조회 이력과 미해결 번호 충돌의 구분

- 작업: [Issue41](https://github.com/jpop-today/jpoptoday/issues/41), `manual-20261019-0933`.
- 스펙: [충돌 번호의 화면·개별 복사 보류](../product/spec.md#충돌-번호의-화면개별-복사-보류--issue41-2026-10-19), DATA-001/SONG-001/FAVORITES-001/CONTENT-001/DESIGN-001.
- 담당: `/root/run41_evidence`. Git·공용 타입·package/lockfile·CI·배포는 root 소유다.
- source base: `1fd6ece7afb4ea7ebddf318f3ef6fa9dafa57951`; docs base: `ea1b6cac498ada996b5ed6dc6a001d0270a37f98`; 브랜치: `codex/41-conflicted-number-controls`.
- 쓰기 범위: source `src/content/guides.ts`의 `tj-ky-numbers` 관련 절·실제 편집일 및 이 문서만. 다른 담당의 미커밋 앱·스펙·기준선·검증 증거를 보존했다.
- 자료 실독: 2026-10-19T00:39:46.820Z(한국 시간 09:39), 보존 대조: 00:41:01.234Z(09:41). 아래의 10월 14일 시각은 기존 번호 조회 이력이며 오늘 새로 조회한 시각이 아니다.

## 원본과 현재 공개 모델에서 직접 확인한 사실

`data/staging/legacy-catalog.json`과 `data/reviewed/provider-corrections.json`을 읽고 기존 `buildPublicCatalog` → `applyProviderCorrections`로 공개 모델을 구성했다. 전체 3317 ID·ledger 84건이다. Hump Back의 다음 두 항목은 각각 TJ/KY에 `status=provider-confirmed`, `conflict=true`를 함께 갖고 있다. 기존 `hasConfirmedNumber`는 두 항목의 두 업체 모두 false다. `status`만으로 복사 가능한 공식 확인을 표시하면 충돌을 놓친다. 전체 충돌은 Hump Back만이 아니며, 전체 모델의 충돌 업체 셀은 8개다. 제품 글은 그중 기존 근거를 확인한 Hump Back 사례 하나를 설명한다.

| 기존 안정 ID | 원제·가수 | origin | TJ / 금영의 보존된 값 |
| --- | --- | --- | --- |
| `jpop-playlist-hump-back-拝啓-少年よ-29d81a757e` | `拝啓、少年よ` / `Hump Back` | playlist | 문자열 `52756` / `76548`; 모두 conflict true |
| `hump-back-拝啓-少年よ-ヘ-ネッセ-進研セ-ミ-高校講座-cm` | `拝啓, 少年よ('ベネッセ 進研ゼミ 高校講座' CM)` / `Hump Back` | chart-only | 문자열 `52756` / `76548`; 모두 conflict true |

이는 같은 곡/버전/항목으로 통합해도 된다는 새 판단이 아니다. 원제·구두점·CM 부제와 별도 ID/URL을 그대로 둔다. 조회 이력은 삭제하지 않지만 미해결 매핑은 사용할 번호의 확정과 구분한다. 보류는 공식 미수록 확인이 아니다. 이번 작업에서 ledger를 보정하거나 새 ID·번호·확인일을 만들지 않았다.

## 재사용한 기존 공식 대조 기록

두 ID의 관련 ledger 4건은 업체별로 다음 동일 이력을 참조한다. 자료 담당은 제공사에 새 요청을 보내지 않았다.

| 업체 | 실제 응답 제목 / 가수 | 기존 checkedAt | 기존 responseSha256 |
| --- | --- | --- | --- |
| TJ | `拝啓, 少年よ('ベネッセ 進研ゼミ 高校講座' CM)` / `Hump Back` | `2026-10-14T14:17:54.373164+09:00` | `3c15821b6215c0bcf9d8ec4eb2b61512352064fb8fc0cf8c574e603a9e47c0dc` |
| 금영 | `拝啓、少年よ` / `Hump Back` | `2026-10-14T14:17:57.828211+09:00` | `118110b1e8f38bc6dba1bce8a74704e8764e29fa1fd317b54a745f00ae0f5583` |

- [TJ의 기존 numeric 검색 URL](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=52756&strWord=Y)
- [금영의 기존 numeric 검색 URL](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=76548)

가이드의 새 출처 2건은 ledger의 URL·checkedAt·제목·가수에 각각 일치함을 계산으로 확인했다. 과거 공식 제목을 오늘의 버전·수록 보장이나 사용자 자료 권리·Google 심사 결과로 확대하지 않는다.

## 제품 변경과 수용 기준

기존 `/guides/tj-ky-numbers/`의 `같은 번호를 가진 여러 항목` 절을 보강했다. 앱 담당과 같은 `번호 충돌 · 재확인 필요` 문구, 해당 업체의 번호·개별 복사 보류, 미수록 비추정을 설명한다. Hump Back의 관리 목록·TJ 차트 항목으로 연결해 사용자가 충돌 안내와 기존 대조 이력을 직접 비교할 수 있게 했다. 원번호를 예약용 값처럼 본문에 다시 제시하지 않는다. 번호를 추측해 채우지 않고 공식 검색·현장 표시를 대조하거나 곡 주소와 근거를 제보하도록 안내한다.

이 글의 실제 본문 편집일만 `2026-10-19`로 바꿨다. 글 안에서 10월 14일 조회와 10월 19일 편집·새 조회 없음의 차이를 명시한다. 첫째·셋째 절, slug/title/description, 기존 다른 5개 가이드와 모든 기존 출처·checkedAt는 그대로다. 위 2개 기존 근거만 해당 글의 sources로 재사용했다.

자료 범위 수용: 기존 원제·가수/checkedAt와 일치하는 간결한 사례, 색 외 상태 문구, 충돌/미수록 구분, 기존 상세 URL 2개, 나머지 5개 가이드 전체 객체·원문 바이트 보존. 실제 UI·복사·360px/키보드/JS-off·전체 빌드·원격 CI·preview·복구·운영 검증은 root/품질 결과가 있어야 완료다. 이 문서 작성만으로 완료 처리하지 않는다.

## 실행한 읽기·계산과 결과

1. 각 repo의 `AGENTS.md`, docs `operations/collaboration.md`와 `product/spec.md` Issue41 절, source `contracts/public-catalog.mjs`, `contracts/provider-corrections.mjs`, `src/content/guides.ts`, `src/pages/guides/[slug].astro`를 읽었다. `git status --short`, `git rev-parse HEAD`, 관련 파일 `git diff`는 읽기만 실행했다.
2. `gh issue view 41 --repo jpop-today/jpoptoday --json number,title,body,state,url`: 최초 sandbox 네트워크 권한 없는 실행은 socket EPERM으로 실패했다. 도구의 network 권한을 붙인 읽기 요청은 성공해 OPEN Issue의 base/범위/수용 기준을 실제 확인했다. 비밀 값·credential 파일·전체 환경은 읽거나 출력하지 않았다.
3. `node --input-type=module`의 fs/crypto와 기존 두 public/correction 계약을 사용해 원본·ledger 전체를 읽고 위 3317/84·Hump Back 2ID의 4셀·strict false·충돌 총8셀·URL을 확인했다. 쓰기·제공사 요청·서버 프로세스는 없다.
4. 처음 Node child-process의 `execFileSync('git', ['show', '<base>:src/content/guides.ts'])` 방식은 spawnSync EPERM으로 실패했다. 이 결과는 보존 비교 성공이 아니다. 셸의 읽기 전용 `git show 1fd6ece7afb4ea7ebddf318f3ef6fa9dafa57951:src/content/guides.ts | node --input-type=module -e '…'` 파이프로 기준 원문을 stdin에 넘기는 방식으로 바꿔 성공했다. Node는 기준 원문을 data-URL 모듈로 import하고 현재 가이드와 `JSON.stringify` 전체 객체 및 slug별 source chunk 바이트를 각각 대조했다. 두 비교 모두 나머지 5개 전부 같음, 전체 글6개, 기존 ID를 가진 상세 링크2개, 출처/checkedAt 일치2개, 편집일19일을 확인했다.

성공한 09:41 계산의 SHA256은 다음과 같다. source chunk는 각 `  { slug: '…'`부터 다음 같은 가이드 시작 직전, 마지막 글은 최종 `.map` 직전까지의 UTF-8 원문이다. 객체 hash는 기본 편집자/날짜를 적용한 guide 객체의 `JSON.stringify`다.

| 보존한 가이드 | reviewedAt | 전체 객체 SHA256 | source chunk SHA256 |
| --- | --- | --- | --- |
| find-japanese-songs | 2026-10-14 | `a7ba1fb3647930fd42116efb83b7b8029c84ca42031c4fe0a7f3138cc55d2673` | `48fe2cf1c7d1304b745e6afefcb6d4010bc52009cd0769e43e01442ebdd873ee` |
| build-your-setlist | 2026-10-18 | `42df98f8716128ccf7adca026641962ced062d12a33dcb264c57b4f504c26319` | `3ed3678b5fa6244410ae90c3d0a619997360a9432cafe976906a2ec219cf2fa4` |
| read-tj-chart | 2026-10-14 | `36c48b7d1241b0a377cb4c5c559e4ddcc7ff7b7a6b45dcd49db827aebd020515` | `0d8915af32a43a24fb2385f2f27fee2134dc3a5017b1d73749325dcbab90dd2f` |
| checking-song-numbers | 2026-10-14 | `d68bd8e60c1cf7cbdde292f18ee05df6627b0d8e14d9bed1cbf25d7160cc7c5f` | `c6b705508b21ee762d463b1fbefd9545a586179b4fff2e2721bb3313c9dc289f` |
| title-artist-variants | 2026-10-16 | `3d15b8cd2bebafe13050644de7d7ff79207fdb39658c2edd05e77f8fcf65bbd5` | `2279934486ba488e5c60a77277bb8eff7b0c46315b58da11b602143cb2a5aae4` |

원자료/산출물:

- staging raw SHA256 `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3`.
- ledger raw SHA256 `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1`.
- base `guides.ts` SHA256 `fffb68fed9612d497e7781a31a2ee6ca07d23da14e68d7bb17f45786512585c6`.
- 편집 후 `guides.ts` SHA256 `227f848b70a9a5077f8cc8486dc1998cb4f6ba70cec2e6a469854506af61ee62`.

## 미실행과 인계

자료 담당은 unit/type/브라우저/빌드/CI/preview/운영/복구/Release/Discord를 실행하지 않았다. 부모 root의 기준선이나 후속 검증을 자신의 실행으로 기록하지 않는다. Git 쓰기·package/lockfile·공용 타입·데이터·다른 앱 파일은 변경하지 않았다. 앱 담당의 UI와 품질 담당의 회귀를 다른 두 산출물로 읽기 전용 리뷰할 예정이며, 이 문서만으로 6개 리뷰 완료나 전체 순위를 주장하지 않는다. AdSense 신청·승인, 계정·권리·문의 수신·ads.txt 계정 일치는 별도 미확인 사항이다.
