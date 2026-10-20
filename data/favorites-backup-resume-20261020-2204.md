# 즐겨찾기 파일 보관 안내의 근거 · Issue51

실행: `resume-20261020-2204` · 담당 `/root/r51_evidence` · 2026-10-20. [Issue51](https://github.com/jpop-today/jpoptoday/issues/51), FAVORITES-001/DATA-001/CONTENT-001/PRIVACY-001 및 [ADR-008](../adr/008-favorites-file-backup.md)을 따른다. 소스 기준 `e52f0566b13bb7879e4c44eed3dd81c326956f70`, 문서 기준 `60e40985d7fdd2d3f0c7ed1f2c4246a275d1260a`. 공유 체크아웃 `feat/51-favorites-backup`, Git·공용 계약·통합·배포는 root 소유다.

## 실제 문제와 변경 범위

기준 소스의 `/favorites/`는 번호 목록 복사만 제공했다. 기존 선곡 가이드는 브라우저 데이터 삭제 시 즐겨찾기를 잃을 수 있으며 번호 복사는 다른 브라우저에 자동 등록하는 백업 파일이 아니라고 명시했다. 파일 보관으로 해결할 문제는 곡 식별자·미해결 항목·기존 저장 객체를 사용자가 다시 가져올 수 있게 하는 것이다. 번호·제목을 새로운 ID 근거로 쓰지 않는다.

자료 담당의 소스 변경은 `src/content/guides.ts`의 `build-your-setlist` 객체뿐이다. 초안은 한국 시간20일에 편집했으며, 같은 run의21일 재개에서 실제 최종 검토·문구 보완을 수행해 본문 확인일을 `2026-10-21`로 갱신했다. 이는 번호의 공식 조회일이 아니다. 사용자 흐름은 전체 목록 파일 내려받기 → 사용자가 보관 → 다른 브라우저에서 파일 선택 → 개수 확인 → 별도 합치기다. 기존 번호 목록 복사는 현재 검색·필터에 맞는 전체 곡의 메모이며 복원 파일과 구분하는 문장을 그대로 유지했다. 파일에 기존 항목의 추가 정보가 들어갈 수 있음을 명시하고 임의 공유를 유도하지 않는다.

## 내용별 근거와 확인 범위

| 안내 | 근거 | 확인 상태 |
| --- | --- | --- |
| 기존 ID·객체 metadata·unknown을 보존 | ADR-002/008、`formatFavoritesBackup`/`parseFavoritesBackup`/`mergeFavorites` | 실제 구현 읽기와 합성 항목 roundtrip·명시적 merge 대조 통과 |
| 전체 목록 파일과 필터된 번호 메모를 구분 | `src/scripts/app.ts`의 `favorites.values` 내보내기와 `filtered` 번호 복사, `contracts/favorites-export.mjs` | 두 입력 범위와 실제 화면 안내 읽기 확인 |
| 파일 선택은 읽기 전용, 별도 확인 후 추가 병합 | `favorites-backup-file` change / `merge-favorites-backup` click, ADR-008 | 읽기 확인 및 Node parse시 storage 쓰기0 확인; 브라우저는 통합 검사 별도 |
| 256KiB/형식/version/ID 거부, 동시 변경 재확인 | 파일 size·fatal UTF-8 decode·parser검증, generation 무효화·expectedRaw 비교 | 실제 구현 읽기 확인; 전체 반례 하네스 결과는 통합 담당 기록을 따름 |
| 손상·쓰기 거절 시 원본 보존, 임시 세션과 영속 성공 구분 | 기존 후보 저장·rollback 재사용, merge 결과 mode별 화면 안내 | 코드 읽기 확인; 손상 원문을 빈 파일로 오해할 수 있는 내보내기 범위 문제는 아래 기록 |
| 파일 로컬 처리·추가 정보 보관/공유 주의 | Blob 다운로드·선택 File.arrayBuffer, 개인정보 안내의 신규 파일 문단 | 해당 처리에 파일 업로드 호출이 없고 다운로드 파일 별도 삭제/사용자 공유 선택 안내가 있음을 읽기 확인 |

이 문서는 제공사 신규 조사·확인일·음원/편곡 동일성·자료 권리·Google 계정/신청/승인을 입증하지 않는다. 공식 원장99항목, raw·동결 차트·3317ID는 이 담당 범위에서 수정하지 않았다. 미해결 ID·확인 필요를 공식 미수록이라고 쓰지 않는다.

## 실제 수행한 검증

각 저장소 AGENTS, 제품 스펙 Issue51절, ADR-002/003/008, 기준 가이드와 즐겨찾기 화면, 기존 저장 모듈을 읽었다. Python으로 기준 SHA의 가이드 파일을 읽어 `slug` 객체 경계별 UTF-8 bytes를 대조했다. 다른 다섯 객체는 날짜·본문·출처를 포함해 모두 정확히 같았다.

| 보존 객체 | SHA-256 |
| --- | --- |
| find-japanese-songs | `f33c6dc099745dc46a134dcd09c51a35e672b6a1eb8f1162d6451de741d5dc25` |
| tj-ky-numbers | `db41fd75b53292ff43e9c40fce28f1d0082335b8f4cc7097274ad09fb6f1945e` |
| read-tj-chart | `727d71f2cb72f917056b01bcf815731b3b848bb30396ca1183cdea10a7e5d888` |
| checking-song-numbers | `383268219d5d7359288b7caf2847d7426c06403c2a4d1cf3fd262a275e51ec83` |
| title-artist-variants | `b8659e39a78f2ed7b4a189d9e1ac04639c2d9a244e01298d0ae4c1d8a1f977a3` |

추가로 `node --input-type=module`에서 실제 `guides.ts`를 import해 모듈 로드, 전체6가이드·대상5절, 대상 편집일, 파일 선택 무변경·기존 항목 보존·256KiB·기존 번호복사와 구분·미수록 부정·업로드 없음 안내, `/favorites/`와 `/privacy/` 링크를 assertion으로 확인했다. 초기20일 검사와 최종21일 문구 보완 후 검사를 구분했으며 결과는 모두 통과했다. 이는 실제 브라우저 동작 검사가 아닌 콘텐츠 모듈 검사다.

한국 시간 21일의 같은 run 재개에서 실제 `favorites.astro`, `app.ts`, 파일 계약과 atomic merge, 개인정보 파일 문단을 다시 읽었다. UI의 `즐겨찾기 파일 보관·가져오기` → `파일로 보관` / `보관 파일 선택` → 개수 확인 → `현재 목록에 합치기`가 안내와 일치한다. counts의 known은 현재 곡 자료와의 ID 대조이며 이미 즐겨찾기에 저장된 ID와의 교집합이 아니다. 앱의 최종 `현재 곡 자료에서 찾은 ID` 문구에 맞춰 가이드도 이 의미를 명시했다. root가 기존 날짜 검사를21일과 맞추기로 인계받았다. 현재 곡 자료 로딩이 실패하면 기능 초기화도 중단되므로 오프라인 복구를 보장하는 안내는 추가하지 않았다.

독립 Node 합성 대조에서는 metadata의 선행0, unknown, 기존 중복을 담은 roundtrip을 `deepEqual`로 확인했다. 파일 해석만으로 storage 쓰기는0이었다. 명시적 merge는 기존 객체·순서·중복을 보존하고 새 unknown 객체와 새 known ID 두 항목만 추가했으며 setItem은1회였다. 지원하지 않는 version2 거부 후 원문 bytes와 쓰기 횟수도 그대로였다. 이는 제공사 번호/자료를 확인한 것이 아니라 합성 항목의 저장 계약 검사다.

발견·인계한 문제: `readFavorites`가 손상 JSON을 읽을 때 `status=invalid-json, values=[]`를 반환하며 초기 내보내기 핸들러는 values만 포맷했다. 원문은 보존되지만 빈 정상 파일을 전체 보관 성공으로 오해할 수 있다. Node에서 이 경계 사례를 실제 재현하고 root·앱 담당에게 전달했다. 앱 담당의 보완 뒤 실제 소스를 다시 읽어 `invalid-json/invalid-shape/invalid-storage/read-failed` 초기 상태의 다운로드를 guard하고 원본을 빈 파일로 대신하지 않았다고 안내함을 확인했다. 유효한 세션 목록을 내보낼 때에도 읽을 수 없는 원본 데이터는 포함하지 않는다고 안내한다. 이 보완은 코드 읽기 확인이며 실제 브라우저의 다운로드 거부·세션 안내 검사는 통합 검증을 따른다. 단위 formatter가 손상 원문 자체를 읽거나 복구했다고 기록하지 않는다.

자료 담당은 브라우저·전체 unit·타입·artifact·CI·배포를 아직 실행하지 않았다. 빌드 결과를 실제 운영·Release·AdSense 승인으로 확대하지 않는다. 수용 기준은 다른5가이드 보존, 안내와 실제 파일 동작의 일치, 실패/임시 상태·개인 보관 범위의 정확한 설명, 내부 실행 보고의 공개 글 비노출이다.
