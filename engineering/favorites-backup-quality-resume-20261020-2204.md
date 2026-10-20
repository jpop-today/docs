# 즐겨찾기 파일 보관·합치기 독립 품질 검사

실행: resume-20261020-2204 · [Issue51](https://github.com/jpop-today/jpoptoday/issues/51) · FAVORITES-001/DATA-001/PRIVACY-001/HARNESS-001/DESIGN-001. 계약: [ADR-008](../adr/008-favorites-file-backup.md), [ADR-002](../adr/002-stable-identity.md). 담당 `/root/r51_quality`, 통합/Git/공유계약/CI/배포 root. source base `e52f0566b13bb7879e4c44eed3dd81c326956f70`, docs base `60e40985d7fdd2d3f0c7ed1f2c4246a275d1260a`. 새 실행을 만들지 않고 사용량 중단 후 같은 역할·범위로 재개했다.

## 소유 범위와 오라클

소스 쓰기는 새 `tests/favorites-backup.test.mjs`, `e2e/favorites-backup.spec.ts`에 한정했다. 제품 구현/가이드/기존 검사/CI/package는 수정하지 않았다. 합성 stable ID·검토 alias·unknown·중첩 metadata·선행0 문자열을 명시적 기대 배열과 대조한다. 결과 formatter나 merge를 호출해 기대값을 만들지 않는다. 브라우저는 동결 staging의 실제 stable ID 두 개를 선택하고, 로컬 저장 원문과 실제 다운로드 JSON을 독립적으로 읽어 비교한다. 합성 메모를 실제 사용자 정보나 공식 근거로 기록하지 않는다.

## 실제 단위 검사

2026-10-20T18:02~18:04Z 관측: `node --test --test-isolation=none tests/favorites-backup.test.mjs` **45 pass / 0 fail**, 실제 부모19·중첩 반례26개다. 마지막 로그 `/tmp/jpop51-quality-unit-confirmed.log`는 임시 실행 증거이며 저장소 파일로 공개하지 않는다. 정확한 최종 통합 SHA/전체 검사·CI/배포는 관리자의 실행 영수증에 연결한다.

파일 전체 values의 순서/중복/객체/unknown/metadata, 검토 alias만 중복 제거, 읽기 전용 파싱, `__proto__` metadata의 비실행, UTF-8 256KiB 정확 경계/초과, malformed JSON·잘못된 envelope/version/values·invalid ID, 잘못된 내보내기의 조용한 누락 금지를 확인했다. 가져오기는 기존 객체/중복을 그대로 두고 새 exact ID만 추가하며 후보 저장 1회다. 중복/빈 가져오기는 원문 공백까지 그대로 유지하고 쓰기0이다.

동시 탭의 확인 뒤 변경·저장 직전 race, 손상 원문·읽기 거절·쓰기 거절·후보를 쓴 뒤 throw·사후 read-back 실패와 rollback, 후속 동시 작성자 비덮어쓰기, 기존 세션 결과에 두 번째 합치기를 검사했다. root가 요청한 **확인 후 읽기 권한 상실** 반례도 포함하며, 이미 정상적으로 읽은 항목이 임시 결과에서 사라지지 않고 실제 저장 원문/쓰기0을 유지함을 확인했다.

현재 환경의 기본 `node --test <file>`은 내부 45개 대신 파일 자체1개만 집계했다. 같은 파일을 직접 실행하거나 `--test-isolation=none`으로 실행하면 실제 개별45개가 등록/실행된다. 이 차이는 관리자에게 인계했고 파일단위1개를45개 통과 증거로 쓰지 않았다. CI의 실제 로그는 별도다.

## 브라우저 검사 · 계획 당시 상태

2026-10-20T18:04Z 현재 `npx playwright test e2e/favorites-backup.spec.ts --list`가 **16개를 등록**했으며 아직 브라우저16개 통과를 의미하지 않는다. root가 최신 앱/공유계약 build를 마치고 단일4321 서버 준비를 확인한 뒤 품질 전용 `/tmp/jpop51-quality-e2e-results` 출력으로 실행한다. 브라우저 plugin의 조작 `js`가 노출되지 않아, 사용자가 허용한 기존 Playwright/system Chromium 검증을 사용한다. 서버/build/새 worker/sidebar는 만들지 않았다.

검사 흐름은 `/favorites/ → 파일로 보관 → 실제 다운로드 파일 선택 → 읽기 전용 개수 확인 → 별도 현재 목록에 합치기 → 원문/unknown/metadata 검증`이다. 필터0곡에서도 전체 파일, 취소/잘못된 JSON·형식·version·ID·UTF8·과대 파일, 실제 다른 탭/같은 탭 변경의 확인 무효화, 느린 파일 읽기 역전/취소, 영속 쓰기 거절/확인 뒤 읽기 거절, 초기 손상·접근 거절의 빈 정상 백업 오인 방지, 손상 원본 대신 유효 임시 목록을 보관할 때 범위 고지, sessionStorage clear event의 세션 보존을 검사한다. 360px/키보드/44px/수평 넘침/unknown HTML 입력 비실행·원래 gradient/palette/JS-off 한계·noindex/noads·콘솔/다운로드 업로드 여부를 확인한다.

이 문서 시점의 미실행: 브라우저16개, 전체 suite/type/artifact/재현/동일SHA CI/실제preview/복구/운영/Release/Discord. 공식 provider 조회·ledger·raw/chart·권리·Google계정/신청/심사 결과를 새로 만들지 않았다. 실제 브라우저 결과가 나오면 아래에 실패와 수정·재검증을 구분해 추가한다.

## 실제 브라우저 결과 · 2026-10-20T18:05:18Z 확인

root의 최신 통합 build gate 뒤 기존 단일 서버 `http://127.0.0.1:4321`에서 아래 명령을 실행했다. **16/16 pass, 38.2초, 첫 실행 실패0**이다. 다운로드 파일을 실제 디스크에서 읽어 전체 배열을 대조했고 파일 선택/확인만으로 저장소 쓰기가 없고 합치기 때 후보1회만 쓰는 것을 계측했다. 실제 두 번째 탭의 localStorage 변경도 사용했다. 페이지 identity/의미 있는 본문·브라우저 pageerror/console error0·target interaction·보안 반례를 확인했다. Chromium 한 엔진의 로컬 빌드 검증이며 실제운영/동일SHA CI가 완료됐다는 뜻은 아니다.

```sh
JPOP_TEST_BASE_URL=http://127.0.0.1:4321 \
PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium \
JPOP_SCREENSHOT_DIR=/tmp/jpop51-quality-screenshots \
npx playwright test e2e/favorites-backup.spec.ts --output=/tmp/jpop51-quality-e2e-results
```

로그 `/tmp/jpop51-quality-e2e-first.log`, 격리 출력 `/tmp/jpop51-quality-e2e-results`를 사용했다. `NO_COLOR`/`FORCE_COLOR` 동시 설정과 npm 업데이트 공지는 제품 오류가 아니며 환경/package를 바꾸지 않았다. screenshot2개를 직접 열어 360px에서 버튼/상태/부가설명과 unknown HTML 문자열이 실제 텍스트로만 보이고 가로 넘침 없이 줄바꿈되는 화면을 확인했다. 이 캡처는 상호작용 후 스크롤 위치의 부분 화면이며 전체 페이지/첫 viewport를 모두 보여준다고 주장하지 않는다.

| 임시 캡처 | SHA256 |
| --- | --- |
| `/tmp/jpop51-quality-screenshots/favorites-backup-mobile-import.png` | `71100ae5eab596c0357b2b529f277749ff569948e758d708d4ac11378de7283c` |
| `/tmp/jpop51-quality-screenshots/favorites-backup-js-off-mobile.png` | `29b3a3ecc601f9fd5c036a0a10af3f64bbd1bd20d55ce914bdfbb8b0688ba37a` |

검증 대상 파일: unit SHA256 `60606391f5a091504a7b7e7df6c3f9bcc22bcf5a4be3553b11e90ffb51764744`, E2E SHA256 `b41e851d2a154679bd6a45519c750b506a250e8bab87f059a88f775fe611432f`. `git diff --check`도 통과했다. 소스/문서 변경은 root에게 인계하며 이 품질 담당은 Git/CI/배포/새 worker/Release/Discord를 실행하지 않았다. 최종 실제 운영/복구·CI/다른 담당 평가·교체는 관리자 영수증을 따른다.
