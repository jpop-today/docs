# 충돌 번호 화면·개별 복사 보류 인계

- 작업: [Issue41](https://github.com/jpop-today/jpoptoday/issues/41), `manual-20261019-0933`
- 담당: `/root/run41_app` (앱·디자인), 통합 담당: `/root`
- 스펙: [product/spec.md](../product/spec.md)의 「충돌 번호의 화면·개별 복사 보류 · Issue41 (2026-10-19)」, DATA-001/SONG-001/FAVORITES-001/DESIGN-001
- source base: `1fd6ece7afb4ea7ebddf318f3ef6fa9dafa57951`, docs base: `ea1b6cac498ada996b5ed6dc6a001d0270a37f98`
- 공유 브랜치: `codex/41-conflicted-number-controls`; 담당자는 Git 쓰기·package/lockfile·공용 타입·CI·서버·브라우저·배포를 수행하지 않았다.

## 관찰과 변경

기준 코드의 `SongRow.astro`, 상세 템플릿, 동적 목록 생성 `row(song)`은 `observedNumber`만 있으면 충돌 번호도 복사 버튼으로 만들었다. 공식 확인 표시 역시 `status === 'provider-confirmed'`로 결정해 기존 엄격 판정과 달랐다. 공식 조회 이력이 존재하는 것과 현재 개별 복사에 쓸 수 있는 번호인지를 구분할 필요가 있다.

세 생성 지점은 이제 업체별 `conflict`를 먼저 확인한다. 충돌 업체에는 번호·`data-copy`·개별 복사 버튼을 만들지 않고 `번호 충돌 · 재확인 필요`를 표시한다. 상세는 `다른 항목과의 번호 충돌이 해결될 때까지 번호 표시와 복사를 보류합니다. 미수록을 뜻하지 않습니다.`를 덧붙인다. 기존 공식 대조 결과 링크·제목/가수·실제 확인일·정정 사유·제보 경로는 유지한다. 이 설명은 새 번호 조회나 곡 ID 병합을 의미하지 않는다.

공식 확인 배지·접근성 라벨·상세 상태 문구에는 기존 browser-safe `hasConfirmedNumber(song, provider)`를 직접 재사용한다. 이 판정은 current/playlist, `provider-confirmed`, `conflict:false`, 문자열 번호와 일치하는 공식 조회 URL 및 근거 필드를 요구한다. 충돌이 없고 번호가 있는 업체는 기존 문자열을 그대로 복사하며, 엄격 판정을 만족하지 않으면 관측 번호로 설명한다. 한 업체의 확인 상태를 다른 업체로 전파하지 않는다. 기존 unknown 설명과 즐겨찾기 전체 복사 formatter는 변경하지 않았다.

## 실제 파일 범위

| 파일 | 변경 |
| --- | --- |
| `src/components/SongRow.astro` | 엄격 판정 import, 정적 업체 셀의 충돌 보류 및 공식 확인 배지 판정 |
| `src/pages/songs/[id].astro` | 엄격 판정 import, 상세 업체별 충돌 보류·미수록 비추정 설명·공식 상태 판정 |
| `src/scripts/app.ts` | 엄격 판정 import와 `row(song)`의 업체 번호 생성 부분만 변경 |
| 이 문서 | 범위·관찰·행동·읽기 검증·미실행 단계 인계 |

스타일 파일·DOM의 기존 업체 셀/저장 버튼 구조·브랜드 palette는 유지했다. 검색/페이지/history·URL/storage·clipboard 성공/거부 처리·bulk formatter·raw/ledger·안정 ID를 수정하지 않았다.

## 수행한 읽기 검증

2026-10-19 KST 작업 중 실제 로컬 파일을 읽고 기존 계약을 호출한 집계에서는 3,317곡 중 충돌 곡이 4개, 해당 업체 셀이 8개였다. Hump Back의 拝啓、少年よ 및 椎名林檎의 丸ノ内/丸の内サディスティック 각각 playlist/chart-only 두 ID가 있다. 여덟 업체는 모두 `provider-confirmed` 조회 이력을 유지하지만 `conflict:true`이며 엄격 판정은 전부 false였다. Hump Back만을 전체 대상으로 삼지 않는다. 이 집계는 앱 정책 이해를 위한 기존 공개 모델 호출이며 품질 담당의 독립 raw/ledger 오라클 검증을 대신하지 않는다.

`git diff`와 `rg`로 세 소유 파일의 생성 분기·엄격 판정 호출·기존 evidence/correction/report 부분 보존을 읽어 확인했다. 초기 `public-catalog.d.mts` 경로 읽기는 그 파일이 없어 exit 2였고, 실제 `.mjs`와 기존 `src/lib/catalog.ts`의 타입을 읽어 보완했다. 이를 타입 검사 성공으로 기록하지 않는다.

읽은 기존 바이트의 SHA256:

| 파일 | SHA256 |
| --- | --- |
| `data/staging/legacy-catalog.json` | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/reviewed/provider-corrections.json` | `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1` |
| `data/chart-snapshot.json` | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |
| `src/styles/global.css` | `64cab788adde9940627b3bb61f50bf99b0191ed984c2114ee1b2bad0c115b889` |

## 인계와 미실행 단계

자료 담당에게 같은 충돌 문구·공식 이력/현재 복사 가능 상태의 구분·미수록 비추정 설명을 전달했다. 품질 담당에게 세 생성 지점과 업체별 상태 분기·정확한 안내 문구를 전달했다. 자료 담당은 기존 가이드의 관련 절만 소유하고 품질 담당은 독립 unit/E2E 및 별도 인계문서를 소유한다.

작성 시점에 담당자가 직접 실행한 unit/type/build/artifact/browser/CI/preview/실제 복구/운영 검사는 없다. root가 수행하는 전체 검증·최종 SHA·실패와 보완·실배포 결과는 [실행 기록](../operations/runs/manual-20261019-0933.json)에서 연결한다. 360px 텍스트 줄바꿈·키보드·JS-off를 실제 렌더로 확인하기 전 완료로 주장하지 않는다. 화면 설명과 번호 보류는 AdSense 계정·권리·제출·실심사 결과의 확인을 뜻하지 않는다.

## 실제 동료 지적과 문구 보완

자료 담당의 첫 읽기에서는 새 상세의 `다른 곡·버전과의 충돌`과 기존 하단 안내의 `같은 번호를 가진 다른 곡·버전이 목록에 있습니다`가 서로 다른 곡/버전이라는 판단으로 읽힐 수 있다고 지적했다. 이 작업은 별도 ID 항목 사이의 미해결 번호 매핑을 보류하는 것이며 곡·버전의 동일성이나 차이를 확정하지 않는다. root의 명시적 보완 지시에 따라 새 문구를 `다른 항목과의 번호 충돌`, 기존 안내를 `같은 번호를 가진 다른 항목이 목록에 있습니다`로 고쳤다. 최초 지적을 숨기거나 정상 렌더 검증으로 바꾸지 않는다. 동료 최종 평가는 이 보완 후 파일을 읽고 진행한다.

보완 전 상세 파일 SHA256은 `040d2e07cf4eba286a9cad2cdee3d7143ccce1626482ea5d4b7dce6ab77aa94c`였다. 번호·업체별 분기·URL·저장·근거 데이터는 이 문구 보완에서 변경하지 않았다.
