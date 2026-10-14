# 차트 동결 스냅샷과 안전한 갱신

Issue [jpoptoday#13](https://github.com/jpop-today/jpoptoday/issues/13) · [ADR-006](../adr/006-reviewed-corrections-and-production.md) · HARNESS-001/DATA-001/SEO-001. 소스 작업 기준은 `9d8cfb716e3ab5194e6711d19ec2b10a297089ad`, 문서 기준은 `d7ccc98bb20c8d997c0ac20e8264528c63b77aab`이다. 이 기록은 2026-10-14 한국 시간 재개 세션에서 로컬 자료와 코드를 실제 검증한 결과이며, 운영 배포 완료나 제공사 실시간 수집 성공을 뜻하지 않는다.

## 현재 자료와 출처

공개 차트는 [TJ Media TOP100](https://www.tjmedia.com/chart/top100)의 기존 개인 저장소 수집 결과를 별도 동결 스냅샷 `data/chart-snapshot.json`으로 관리한다. 실제 원문 자료 출처는 `kim75503/todayjpop`의 `94d615f5e355f4b00ebbb824547ac6dfa034ad18:data/chart.raw.json`이다. 원문에는 공개하지 않는 필드가 있으므로 표에 필요한 ID·제목·가수·문자열 TJ 번호·순위만 투영한다. 제공사 응답 원문을 공개 artifact에 넣지 않는다.

| 항목 | 검증한 값 |
| --- | --- |
| 기존 수집 시각 | `2026-10-13T10:26:45.525Z` (한국 시간 2026-10-13 19:26:45.525) |
| 요청 기간 | 2026-09-29–2026-10-13 |
| `periodVerified` | `true`: 기록한 요청의 시작·종료 날짜와 metadata가 일치함. 제공사가 실제 집계 기간을 별도로 확인했다는 주장은 아님 |
| 전체 행·별칭 ID·canonical ID | 각 100개, 중복 없음 |
| 원문 자료 SHA256 | `9f98db5c7929ef04f3183994abae122dd8c7101e3635fc7e45f5c95effa22ee0` |
| 고정 staging revision | `6dbe5a65d1bd729c09393f746f715f4363089a43` |
| staging 바이트 SHA256 | `a3d482ca2da082852883960d200841b4ad4ff26336eaf1174c55677881eecbf3` |
| 이전 차트와 차이 | 같은 100개 별칭 ID, 추가·삭제 없음, 46개 순위 변경 |

원본 staging의 3,317개 안정 ID를 유지한다. 스냅샷의 `legacyRevision`은 해당 차트 원문 수집 커밋이며 앱의 Git SHA나 staging revision과 다르다. 빌드 시각으로 `fetchedAt`를 다시 만들지 않는다. 이 기록을 작성한 날과 자료 수집일도 구분한다.

## ID 연결과 갱신 계약

이전 차트 별칭 ID가 항상 앱의 안정 ID와 같지는 않다. 최초 한 번만 고정 staging의 `js/chart.js` 출처 hash·원본 참조 위치와 `data/legacy-identity-baseline.json`에 보존된 제목·가수·TJ 번호 관측 tuple을 모두 대조하여 `canonicalId`를 넣는다. 이 시드 작업은 staging revision과 같은 원본 스냅샷에만 허용한다.

이후의 순위 변경은 별칭 ID로 이전 스냅샷을 찾고 보존된 canonical ID를 이어간다. 현재 순위나 새로운 배열 위치로 앱 ID를 추정하지 않는다. 번호만 같은 다른 곡, 새 별칭, 제목·가수·번호 변경, 누락·중복·100개 미만 입력은 자동 반영하지 않고 검토로 돌린다. 일련의 순위 변경 뒤 staging 배열 순서까지 바꾼 합성 테스트로 index에 의존하지 않는 연결을 검증했다.

재실행 명령은 다음과 같다. `<sourceSHA>`에는 실제 원문 자료 커밋의 전체 40자리 SHA를 전달한다. 파일을 내려받았다는 사실만으로 그 SHA를 추정하지 않는다.

```sh
node scripts/import-chart-snapshot.mjs <raw.json> <snapshot.json> <staging.json> <sourceSHA>
```

수집기 자체는 이 명령에 포함하지 않았다. 갱신기는 UTF-8 JSON만 읽고 JavaScript 입력을 실행하지 않으며 공식 TJ source/API URL·JPOP 요청·실제 달력 날짜·요청 기간 일치·미래 수집 시각·이전보다 오래된 자료·문자열 번호·1부터 100까지의 순위를 검증한다. 검증 clock은 테스트에서 명시할 수 있고 실제 실행은 현재 시각을 사용한다. 미래 입력을 거부해 아직 수집하지 않은 자료를 수집 완료로 받아들이지 않는다.

기존 snapshot과 raw/staging이 같은 파일 또는 hard link인 경우와 출력 symlink를 거부한다. 모든 검증이 끝난 뒤 같은 디렉터리에 독점 생성한 임시 파일에 쓰고 `sync`·닫기를 마친 다음 rename한다. open·부분 쓰기·rename 실패는 이전 snapshot 바이트와 raw/staging을 보존하고 생성한 임시 파일을 제거한다. 원문/private 필드는 스냅샷에서 제거한다.

## 실제 검증 기록

2026-10-14 한국 시간 14시대 재개 세션에서 Node.js 24.19.0으로 다음을 실행했다.

```sh
node --test --test-reporter=spec tests/chart-snapshot.test.mjs tests/provider-corrections.test.mjs
```

차트 45개와 정정 계약 8개, 총 53개가 통과했다. 차트 테스트는 합성 fixture이고 실제 제공사 심사 결과가 아니다. 기본 실행 sandbox에서는 Node의 CLI 자식 프로세스가 제한되어 최초 실행이 실패했으며, 명시된 실행 권한으로 다시 실행한 결과를 통과 기록으로 사용했다. TLS나 proxy 설정을 우회하지 않았다.

별도 실제 자료 검증은 source 기준 SHA의 이전 snapshot을 임시 디렉터리에 복사한 뒤 최신 고정 raw에 대해 importer를 실행했다. 결과는 현재 후보 snapshot과 deep equality로 같았고, raw와 staging 바이트 hash가 실행 전후 일치했다. 46개 순위 변경과 100개 고유 canonical ID를 확인했다. 현재 후보에 같은 raw를 재적용한 결과 역시 동일했다. 이 검증은 원본 staging이나 실제 배포 자료를 덮어쓰지 않았다.

함께 읽은 정정 ledger는 43개 entry(공식 일치 30, 기존 번호 억제 13)였다. 실제 3,317개 공개 song 투영에 적용해 ID 유지·원본 hash 유지·`responseSha256`/`refs`/가사/이미지 필드 비공개를 확인했다. 제공사 제목·가수·버전의 편집 판단은 [정정 근거](provider-corrections.md)에 따르고, shape/hash 검증만으로 공식 곡 정체성이 입증됐다고 주장하지 않는다.

`contracts/publication.mjs`, `scripts/verify-dist.mjs`, `scripts/manage-production.py`와 두 배포 script는 읽기 전용으로 검토했다. production은 현재곡 중 충돌 없는 공식 근거가 있는 상세만 색인하며 preview는 모두 noindex다. 같은 SHA·실제 CI·preview/production HTTP·복구 확인은 관리자의 통합/배포 증거에 별도로 기록한다.
