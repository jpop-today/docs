# 즐겨찾기 번호 목록의 상태 보존 근거

2026-10-18 KST · run `resume-20261018-0700` · [jpoptoday#38](https://github.com/jpop-today/jpoptoday/issues/38) · FAVORITES-001/DATA-001/CONTENT-001 · 담당 `/root/resume38_evidence`.

소스 base `3ca14f80a1e0dbdf055882d195751c4d889a0db6`, 문서 base `dbc3196441c01368d621c6b65b29c1ddede1a1e2`, 작업 브랜치 `codex/38-favorites-export-status`. 소유 범위는 소스 `src/content/guides.ts`의 `build-your-setlist` 객체와 이 문서다. [제품 계약](../product/spec.md)의 Issue38 절을 따른다. root가 Git·공통 formatter·통합 검증·CI·배포를 수행하며 이 기록의 코드 읽기와 순수 자료 계산은 실제 브라우저·배포 검증을 대신하지 않는다.

## 실제 문제와 수용 동작

base의 `src/scripts/app.ts`는 현재 검색·필터에 맞는 `filtered` 전체를 순서대로 복사하지만 업체 상태 없이 `observedNumber ?? '확인 필요'`만 적는다. 따라서 공식 확인 번호와 관측 번호가 메모에 같은 형식으로 남고 충돌 번호도 그대로 포함된다. 화면의 페이지·더 보기로 자르는 부분은 목록 렌더의 `filtered.slice(...)`이며 복사 대상과 별개다.

root가 2026-10-17T22:05:21.896Z에 고정된 v0.4.5 운영 artifact M `ed22f924536d28fa990d7688b6ba352446984140`을 로컬 브라우저로 실행한 [기준선](../operations/runs/resume-20261018-0700-baseline-export.json)도 읽었다. 360×900 화면에서 Ado를 저장하고 clipboard 거부 후 열린 실제 수동 창은 `逆光 · Ado | TJ 68868 | 금영 76509`를 담았고 두 상태 표기가 빠져 있었다. 이 기록은 기존 제품 artifact의 로컬 재현이며 새 코드나 원격 운영의 브라우저 통과 결과가 아니다.

새 `contracts/favorites-export.mjs`의 `formatFavoritesExport(songs)`를 2026-10-17T22:05Z 이후 작업 트리에서 읽고 실제 3317곡 projection에 적용했다. 이 formatter는 전달받은 곡의 원제·원문 가수·순서와 번호 문자열을 유지하고 업체별 상태를 붙인다. 앱은 기존 `filtered` 전체를 전달하므로 더 보기 전의 곡과 다른 페이지의 일치하는 저장곡도 포함한다. 검색·필터를 해제해야 조건 없는 전체 저장곡이 대상이 된다. 저장된 미해결 ID는 원문을 보존하고 별도 안내하며, 현재 카탈로그의 곡 정보가 없는 ID를 새 번호 행으로 만들지 않는다.

| 업체별 입력 | 내보내는 표기 | 판정 경계 |
| --- | --- | --- |
| `hasConfirmedNumber(song, provider)`가 참 | `TJ <번호> (공식 확인)` 또는 `금영 <번호> (공식 확인)` | current/playlist, provider-confirmed, conflict:false, 실제 근거 필드와 해당 번호에 정확히 일치하는 공식 numeric URL 조건을 그대로 재사용한다. 한 업체의 확인을 다른 업체로 확대하지 않는다. |
| observed-unverified, conflict:false, 숫자로만 된 문자열 | `<업체> <번호> (관측·미확인)` | 기존 관측 계약의 `/^\d+$/`를 유지하며 선행 0을 바꾸지 않는다. 공식 확인의 길이·비영(非零) 조건을 관측값에 새로 적용하지 않는다. |
| null/unknown, 정정으로 suppress된 값, 충돌 또는 공식 확인 조건 불충족 | `<업체> 확인 필요` | 번호를 내보내지 않는다. 근거 누락·불일치의 provider-confirmed를 관측으로 낮춰 번호를 재노출하지 않는다. 확인 필요는 공식 미수록이 아니다. |

공식 확인 predicate는 `contracts/public-catalog.mjs`, 공식 URL 일치는 `contracts/provider-lookup.mjs`, 원본 projection은 `buildPublicCatalog`와 `contracts/provider-corrections.mjs`의 `applyProviderCorrections`를 사용한다. 날짜·제목·가수의 실제 대조는 기존 reviewed ledger의 책임이며 formatter가 새 조회·심사·권리 판단을 만들지 않는다.

## 동결 자료의 실제 예

새로운 제공사 요청 없이 `data/staging/legacy-catalog.json`과 `data/reviewed/provider-corrections.json`을 현재 코드로 읽었다. 84개 ledger 중 suppress는 업체별 13개/고유 9곡이며, 공개 projection의 충돌은 4개 ID다. 아래는 현재 작업 트리 formatter를 순수 실행한 실제 출력이다.

```text
逆光 · Ado | TJ 68868 (공식 확인) | 금영 76509 (관측·미확인)
ルカルカ★ナイトフィーバー · samfree | TJ 27031 (공식 확인) | 금영 43185 (관측·미확인)
W / X / Y · Tani Yuuki | TJ 52444 (공식 확인) | 금영 확인 필요
アンコール · YOASOBI | TJ 68359 (관측·미확인) | 금영 44636 (관측·미확인)
チューリングラブ feat.Sou · ナナヲアカリ | TJ 52536 (공식 확인) | 금영 확인 필요
拝啓、少年よ · Hump Back | TJ 확인 필요 | 금영 확인 필요
```

Ado의 금영 76509는 생략된 공식 부제의 버전 판단 보류, samfree의 금영 43185는 SAM 명의와의 동일성 판단 보류를 유지한다. 이 작업에서 두 번호의 제목·가수나 checkedAt를 다시 확인하거나 명의·버전을 확정하지 않았다. Tani Yuuki의 금영은 원래 null, 튜링 러브의 과거 금영 57803은 suppress이므로 각각 같은 확인 필요 표기라도 근거가 다르다. Hump Back의 playlist ID `jpop-playlist-hump-back-拝啓-少年よ-29d81a757e`와 chart-only ID는 서로 합치지 않으며 양 업체의 번호 충돌 때문에 둘 다 번호를 생략한다.

현재 관측 번호의 부재나 정정 사유를 제공사 전체의 공식 미수록 판정으로 바꾸지 않는다. 기존 공개 원문은 소스 `src/content/guides.ts`의 `title-artist-variants` 객체와 ledger를 기준으로 읽었다.

## 선곡 가이드 수정과 보존

`build-your-setlist`의 ‘번호 목록을 메모로 남기기’ 절만 위 행동에 맞췄다. 같은 객체의 본문 편집·검토일만 실제 KST 날짜 `2026-10-18`로 갱신했다. 이는 번호의 제공사 조회일이 아니다. 앞의 두 절·제목·설명·slug, 다른 다섯 가이드의 전체 객체와 각 본문 확인일·공식 출처 시각은 그대로다.

안내는 현재 검색·필터 전체 일치곡, 업체별 세 상태, 충돌·제외 번호 생략, unknown과 미수록의 구분, JavaScript 필요, 클립보드 거부 시 선택 가능한 텍스트, 메모·메신저 사용과 자동 재등록 백업의 차이를 설명한다. `src/scripts/app.ts`의 `copy()`와 `src/layouts/Layout.astro`의 `copy-dialog`가 실제 수동 선택 경로다. 새 콘텐츠에 가사·이미지·추천·난이도·권리 허락·Google 심사 결과를 추가하지 않았다.

## 수행한 확인과 인계

Node v24.19.0에서 가이드 모듈을 읽고 base의 다른 다섯 객체와 JSON 직렬화 값을 비교해 동일함을 확인했다. 변경 slug는 `build-your-setlist` 하나이며 앞의 두 절도 base와 동일했다. 다른 다섯 객체의 SHA256은 `e2e0c83c3d407423abdea49a18f51bede9ec9c1ea0c069d92318baa30688ec99`, 수정 후 가이드 파일 SHA256은 `fffb68fed9612d497e7781a31a2ee6ca07d23da14e68d7bb17f45786512585c6`이다.

원본 바이트를 읽은 SHA256:

| 파일 | SHA256 |
| --- | --- |
| `data/staging/legacy-catalog.json` | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| `data/reviewed/provider-corrections.json` | `dbef4ae6abc202b90e7e709f24d5654cee6840ec9f8ac7998ac0e43da0356cc1` |
| `data/chart-snapshot.json` | `7ade29897828967b98b2bca64b285b9694356ad3d2b97498f06270217c91da97` |

첫 비교 명령은 Node의 `execFileSync('git', ...)`에서 EPERM으로 실패했다. 같은 Git 읽기를 셸에서 수행해 임시 base 모듈로 전달한 뒤 위 비교를 완료했다. 실패를 테스트 통과로 계산하지 않는다. 이 담당자는 browser/server·canonical test·타입·artifact·CI·preview·운영·복구를 실행하지 않았다. root의 실제 결과와 두 동료의 읽기 평가를 최종 통합에 연결한다.
