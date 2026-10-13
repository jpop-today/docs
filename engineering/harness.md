# 개발·검증 하네스

상태: `HARNESS-001`과 원본 ID 기준선 구현. 현재 `jpop-today/jpoptoday`는 프레임워크와 독립적인 계약·스냅샷·이관 조사 하네스이며 운영 사이트가 아니다. 원본 앱 이관과 사이트 미리보기·배포 검증은 후속 단계다.

## 현재 실행 계약

Node.js 22 이상을 사용한다. 런타임 외 설치 패키지가 없으며 설치 과정은 생략한다. 클라우드 작업은 이미 격리되어 있으므로 기존 체크아웃에서 작업하며 사용자의 요청 없이 worktree를 생성하지 않는다.

```bash
cd /workspace/jpoptoday
npm test
npm run validate:catalog -- tests/fixtures/catalog.json
```

CLI 승격을 별도로 확인할 때는 테스트 출력 디렉터리를 새로 만든다. 아래 입력은 가상의 fixture이며 운영 파일에 승격하지 않는다.

```bash
snapshot_check_dir=$(mktemp -d /tmp/jpoptoday-snapshot-check.XXXXXX)
npm run promote:catalog -- tests/fixtures/catalog.json "$snapshot_check_dir/catalog.json"
npm run validate:catalog -- "$snapshot_check_dir/catalog.json"
```

수집기는 후보 파일을 별도 위치에 완성한 뒤 종료 상태를 확인하고 승격기를 호출해야 한다. 수집 명령이 실패했는데 이전 후보를 재사용하거나, 전체 대상 수집에 실패했는데 부분 후보로 기존 전체 스냅샷을 교체하지 않는다. 이 수집기 연결은 아직 구현하지 않았다.

## 자동 검사

| 검사 | 현재 보장 |
| --- | --- |
| v1 카탈로그 | 비어 있지 않은 배열, 안정 ID 유일성, 아티스트 참조, 제공사 상태, 번호 문자열·선행 0, 제공사 번호 충돌, 근거 URL 형식·날짜 |
| 기존 검증 CLI | 다른 작업 디렉터리에서 실행, 누락 입력·깨진 JSON·빈 데이터의 실패 종료 |
| 정상 승격 | 후보 원래 바이트 보존, 첫 스냅샷 생성, 입력 파일 보존, 상태·근거를 자동 수정하지 않음 |
| 잘못된 후보 | 누락 파일, 빈 출력, 깨진 JSON, 빈 배열, 지원하지 않는 스키마, 깨진 UTF-8, 누락 근거에서 기존 파일 보존 |
| 경로 보호 | 입력=출력, 정규화 별칭, 심볼릭 링크·하드 링크 별칭 거부, 출력 링크·디렉터리 거부 |
| 파일 I/O 실패 | exclusive open, 부분 write, sync, close, rename 실패 시 기존 스냅샷 보존, 소유한 임시 파일 정리 |
| 완성 파일 공개 | rename 직전 독자는 이전 정상 파일을 읽고, 교체 후 완성 후보를 읽음 |
| 승격 CLI | 다른 작업 디렉터리 실행, 성공 메시지·실패 종료 상태, 기존 파일 보존 |
| 원본 ID 조사 | 순수 JSON literal 파싱, 실행 가능한 JS·깨진 입력·중복 ID 거부, 원본 필드·파일 해시·ref 보존, 번호 공유를 미해결 상태로 유지 |

쓰기 오류는 실제 임시 파일에 일부 바이트를 쓴 뒤 실패를 주입해서 검사한다. chmod로만 오류를 만드는 테스트는 root 또는 다른 실행 권한에서 결과가 달라질 수 있어 사용하지 않는다. 실패한 exclusive open으로 생성하지 못한 임시 파일은 삭제하지 않으며 이 상황도 검사한다.

실제 파일 시스템 호출에 대한 장애 주입은 테스트 전용 `io` 인자로 수행한다. 운영 CLI는 Node의 파일 시스템을 그대로 사용한다. 테스트 fixture의 URL·확인일·곡은 가상이며 실제 제공사 검증 자료가 아니다.

## 스냅샷 구현 범위

`contracts/catalog-snapshot.mjs`는 입력과 출력 경로가 같거나 같은 inode를 가리키는지 확인하고, 출력 링크를 거부한다. 후보를 읽어 검증한 다음 출력 디렉터리의 임시 파일에 원래 바이트를 쓰고 `sync`, `close`, `rename` 순서로 진행한다. 동일 파일 시스템의 atomic rename을 제공하는 Linux 로컬 디스크가 기준이다.

교체 전 실패는 기존 스냅샷을 건드리지 않는다. 쓰기 중 프로세스가 강제 종료되면 임시 파일이 남을 수 있지만 최종 경로는 부분 쓰기로 노출되지 않는다. 정리 자체가 실패해도 원래 오류를 숨기지 않으며, 남은 임시 파일은 운영 점검 대상으로 삼는다. 디렉터리 fsync·전원 장애 시 영속성, 여러 게시자의 실행 순서, 여러 파일을 한 번에 교체하는 트랜잭션은 현재 보장 범위에 없다. 수집·게시 작업은 한 게시자씩 실행한다.

구조가 유효한 작은 부분 카탈로그도 v1을 통과할 수 있다. 전체 스냅샷 수집기를 연결할 때 대상 범위와 ID 차이, 대규모 감소, 번호 상태 변화와 근거 변경을 검토하는 게이트가 필요하다. 현재 CLI를 실제 카탈로그에 바로 연결하지 않는다.

## 원본 ID 기준선 재현

`scripts/audit-legacy-source.mjs`와 `contracts/legacy-identity.mjs`는 로컬 원본 Git 체크아웃과 두 개의 서로 다른 full commit SHA를 입력받는다. `git show <sha>:js/songs.js` 및 `js/chart.js`를 shell 없이 argv로 호출하고, `GIT_NO_LAZY_FETCH=1`로 누락 객체의 네트워크 다운로드를 막는다. 원본 작업 파일이나 브랜치를 변경하지 않는다.

원본 JavaScript를 VM·eval·import로 실행하지 않는다. 생성 주석과 `const songs = JSON 배열;` 또는 `const tjJpopChart = JSON 배열;` 형태만 받아 `JSON.parse`한다. JSON 밖 실행문·식·다른 변수·빈 배열·중복 ID·문자열이 아닌 번호·깨진 UTF-8은 거부한다. 동일 번호의 다른 ID는 원본 KY 충돌을 보존하기 위해 자동 병합하지 않는다.

아래 두 SHA는 조사 당시 원본 `main`과 `dev`다. 실행 시 이동하는 브랜치명 대신 불변 SHA를 사용한다. 체크아웃 경로는 현재 환경의 읽기 전용 조사 복사본이며, 다른 환경에서는 로컬 원본 경로로 바꾼다. Git와 로컬 객체가 필요하지만 GitHub 연결이나 제공사 수집은 필요하지 않다.

```bash
cd /workspace/jpoptoday
npm run audit:legacy -- /tmp/jpop-original-audit c57a7ae1be0c6cff06c847bb7dbc5c572e631edd 6dbe5a65d1bd729c09393f746f715f4363089a43 data/legacy-identity-baseline.json
npm run audit:legacy -- /tmp/jpop-original-audit c57a7ae1be0c6cff06c847bb7dbc5c572e631edd 6dbe5a65d1bd729c09393f746f715f4363089a43 data/legacy-identity-baseline.json --check
```

출력 디렉터리는 미리 존재해야 한다. 출력은 원본 체크아웃 밖에 저장하고 심볼릭 링크·디렉터리 출력은 거부한다. 쓰기 모드는 별도 임시 파일을 완성·동기화한 뒤 교체한다. `--check`는 쓰지 않고 기준선의 바이트가 동일한지 확인하며 차이가 나면 실패한다. 원본 origin은 자격증명 없는 GitHub 저장소 주소로 기록하고 자격증명을 포함한 URL은 거부한다.

`data/legacy-identity-baseline.json`은 `baselineVersion: 1`, `purpose: legacy-identity-audit`인 이관 조사 산출물이다. v1 공개 카탈로그와 별도 형식이며 운영 데이터 승격에 사용하지 않는다. 저장소·commit SHA·파일 경로·원본 바이트 SHA-256을 기록하고, 각 원본 ID에 제목·가수·TJ/KY 관측 번호와 source/index 참조만 연결한다. 원본의 가사·이미지 URL·순위·기타 공개 콘텐츠·provider 근거·확인일은 복사하거나 생성하지 않는다. 서로 다른 source에서 같은 ID의 내용이 달라지면 각각의 observation을 보존한다.

실제 실행 결과(2026-10-13):

| 대상 | 등록 목록 ID | 차트 ID | 병합 ID |
| --- | ---: | ---: | ---: |
| 원본 `main` / `c57a7ae…` | 3,299 | 100 | 3,313 |
| 원본 `dev` / `6dbe5a6…` | 3,299 | 100 | 3,313 |
| 두 SHA 합집합 | 3,299 | — | 3,317 |

합집합의 차트 전용 ID는 18개다. 공유 번호 후보는 TJ 4그룹·KY 7그룹이며 `unresolvedAliases`로 남겼다. 같은 번호만으로 URL redirect나 새 canonical ID를 만들지 않았으며 모든 원본 ID를 유지했다. 원본 URL·즐겨찾기 이관은 이 기준선을 사용하되 실제 동일 곡·버전 검토와 호환성 테스트가 추가로 필요하다.

| SHA의 원본 파일 | 원본 SHA-256 |
| --- | --- |
| `main` / `js/songs.js` | `9a024c4259f74001215a36c61e0b1cf37199a6fcb57d9b0cfd596c2c842fd9c9` |
| `dev` / `js/songs.js` | `9a024c4259f74001215a36c61e0b1cf37199a6fcb57d9b0cfd596c2c842fd9c9` |
| `main` / `js/chart.js` | `4724b9fb6d1526e633577410c84f92a774d9adb1a9a63cafe6b7b0cb56bff12f` |
| `dev` / `js/chart.js` | `5722774c7047f6314d2726389692f09578111f4752296001881c4d1a2eeab50e` |

기준선 생성, 위 개수 assertion, 필드 제한 검사, `--check` 재현 일치 확인을 수행했다. 원본 Git 작업 상태는 변경 없이 유지했다. 이 성공은 원본 카탈로그의 출처·확인일·권리 검토가 끝났다는 뜻이 아니며 공개 v1 검증이나 운영 배포 성공으로 기록하지 않는다.

## CI

소스 저장소 `.github/workflows/contracts.yml`에서 `main`, `codex/**` push, PR, 수동 실행에 Node.js 22 검사를 수행한다. 권한은 `contents: read`이고 테스트, 검증 CLI, 임시 경로에 합성 데이터 승격 CLI를 실행한다. 운영 데이터 다운로드, 사이트 빌드, 배포, 광고 코드를 실행하지 않는다.

원본 ID 동작 테스트는 작은 합성 문자열로 다른 refs의 합집합, 번호 공유의 미해결 상태, 같은 ID의 변화 보존, 중복·실행문·못 파싱하는 입력 거부를 확인한다. 추가로 커밋된 실제 기준선을 읽는 integrity test가 두 SHA·4개 파일의 pinned hash metadata, 3,317개 고유 ID·등록 목록 3,299개·차트 전용 18개, ref의 배열 범위·원본 행의 누락/중복, 미해결 그룹 11개 보존을 검사한다. Expected hash는 원본에 독립적으로 `git show <sha>:<path> | sha256sum`을 실행해 계산했다.

Integrity test는 원본 파일 본문을 재검증하거나 제공사 출처의 진위를 확인하지 않는다. CI는 외부 원본 체크아웃이 없어도 실행되며 실제 원본 바이트와 기준선의 재현 일치 여부는 위 `--check` 명령으로 별도 확인한다.

검증 기록(2026-10-13, Node.js 24.19.0): `npm test` 55개 통과, 실패·건너뜀 0. `npm run validate:catalog -- tests/fixtures/catalog.json` 통과. 실제 기준선 생성·`--check`도 통과. Node.js22 GitHub Actions push와 PR checks가 success임을 API로 확인했다: https://github.com/jpop-today/jpoptoday/actions/runs/38339116656. 원시 로그 다운로드 host는 이 환경에서 차단되어 로그 본문은 읽지 않았으며 테스트 수55는 로컬 실행 결과다. 로컬 테스트 성공을 운영 데이터 검증 또는 운영 사이트 배포 성공으로 설명하지 않는다.

## 원본에서 확인한 실패 위험

조사 기준: `kim75503/todayjpop` 커밋 `6dbe5a65d1bd729c09393f746f715f4363089a43`.

`scripts/build-jpop-playlist-data.js`는 입력 TSV가 없으면 기존 `data/jpop-playlist.raw.json`에 빈 `songs`를 쓰고 정상 종료한다. `scripts/data-utils.js`의 `writeJson`·`writeText`는 최종 파일에 직접 `writeFileSync`를 호출한다. 입력 누락 또는 중간 쓰기 실패가 마지막 정상 데이터를 대체할 수 있다.

`scripts/fetch-tj-chart.js`는 빈 응답·90개 미만 차트·번호 중복을 거부하지만 최종 쓰기는 같은 직접 쓰기 유틸리티를 사용한다. 새 하네스는 안전 승격을 별도로 구현했으며 아직 원본 수집기·빌드기에 통합하지 않았다. 원본 전체 앱 구조와 데이터 provenance를 조사한 뒤 이관 작업에서 연결하고 실제 카탈로그로 회귀 검증한다.

## 이관 이후 필요한 검사

1. 원본의 TSV·JSON·기존 ID·URL·저장 데이터를 보존한 staging 검증. 근거 부족과 KY 번호 충돌은 [DATA-001](../data/catalog-contract.md)의 보강 절차 적용.
2. 원본 정상 빌드와 새 빌드 비교. 카탈로그 누락·ID 변경·오래된 정적 출력·비어 있는 사이트 공개 방지.
3. 검색·제공사 필터·곡 상세·즐겨찾기와 기존 링크 회귀를 브라우저로 확인. 작은 화면, 키보드, 오류·빈 결과 복구, 접근성 포함.
4. 실제 공개 산출물에서 개인정보·운영자·문의·콘텐츠 출처와 권리 검토. 광고 준비 완료와 AdSense 실제 승인 구분.
5. Cloudflare 미리보기에서 대표 경로·헤더·색인·정적 리소스 검사. 운영 전 마지막 정상 artifact와 되돌리기 경로 확인.

각 작업 기록에는 스펙 ID, 범위, 변경 파일, 실행 명령·결과, 검증한 SHA, 남은 차단 사항을 남긴다. 합성 테스트·원본 검사·실제 카탈로그·미리보기·운영 배포 결과를 각각 명시한다.
