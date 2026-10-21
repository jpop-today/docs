# 즐겨찾기 파일 보관·가져오기 · 앱 인계

[Issue51](https://github.com/jpop-today/jpoptoday/issues/51) · run `resume-20261020-2204` · 담당 `/root/r51_app` · source base `e52f0566b13bb7879e4c44eed3dd81c326956f70` / docs base `60e40985d7fdd2d3f0c7ed1f2c4246a275d1260a`.

FAVORITES-001/DATA-001/PRIVACY-001/DESIGN-001과 [ADR-008](../adr/008-favorites-file-backup.md), [ADR-002](../adr/002-stable-identity.md)를 기준으로 한다. 쓰기는 소스 `src/scripts/app.ts`, `src/pages/favorites.astro`와 본 문서에 한정한다. Git/package/lock/CI/공용 계약/배포는 root 소유다. 기존 미커밋 변경을 보존했고 별도 서버·worktree·사용자 소유 채팅·하위 agent를 생성하지 않았다.

## 구현한 사용자 흐름

`/favorites/` → native details 「즐겨찾기 파일 보관·가져오기」 → 「파일로 보관」 또는 「보관 파일 선택」 → 파일 항목 수 확인 → 「현재 목록에 합치기」다. 기존 번호 목록 복사는 현재 검색·필터 결과의 번호 메모이고, JSON 파일 보관은 필터와 관계없는 전체 저장/유효 임시 목록이다. 현재 목록에서 찾지 못한 ID와 기존 객체 부가 정보도 포함할 수 있어 보관·공유 전 확인을 안내한다. 파일의 provider 확인 상태는 신뢰하지 않는다.

파일 읽기는 최대256KiB 검사 후 `TextDecoder('utf-8', {fatal:true})`와 root의 parse 계약으로 검증한다. 선택·검증은 읽기 전용이며 별도 합치기 버튼에서만 root의 원자적 merge 계약을 호출한다. 확인 시점 원문을 expectedRaw로 전달하고 충돌 시 쓰지 않은 채 재선택을 요구한다. 현재 탭의 저장/해제, 다른 탭의 localStorage 변경·clear는 대기 중 확인을 취소한다. sessionStorage의 별도 clear 이벤트는 즐겨찾기 임시 목록을 버리지 않는다. 느린 파일 읽기 중 새 파일 선택/취소가 일어나면 generation 값으로 이전 결과가 화면을 덮지 못하게 한다. 취소 버튼은 읽기 중에도 접근 가능하다.

브라우저 파일 다운로드는 고정 이름 `jpoptoday-favorites-v1.json`으로 요청하고 URL을 해제한다. 성공 안내는 실제 다운로드 폴더 확인을 요청하며 브라우저가 파일을 보관했다고 단정하지 않는다. 초기 손상/읽기 실패 상태의 빈 placeholder 배열을 빈 정상 백업으로 내보내지 않는다. 이후 명시적으로 만든 유효한 임시 목록을 보관할 때에는 읽지 못한 원본 데이터가 포함되지 않음을 알린다. 저장 실패의 임시 상태와 영속 성공을 구분하며 rollback 실패·동시 변경으로 원본 복구를 확인하지 못한 경우에는 보존 성공을 주장하지 않는다.

모든 변경은 DOM의 textContent/고정 문자열로 안내하며 사용자 파일명·파일 내용·항목을 로그에 쓰지 않는다. 앱이 파일을 서버에 업로드하는 경로를 추가하지 않았다. JSON 읽기·전체 저장 목록 판별은 기존 catalog 로딩 성공 뒤에만 가능하며, 로딩 실패 시 disabled 상태와 새로고침 안내를 유지한다.

## 화면과 접근성

패널은 기존 즐겨찾기 본문에서 선택적으로 펼친다. summary/버튼/파일 선택은 최소44px, 키보드 focus-visible은 기존 전역 스타일을 이용한다. 모바일에서는 합치기/취소를 각각 한 줄로 배치한다. scoped 스타일만 추가했고 기존 blue→pink 브랜드·pink/mint/blue 배경을 수정하지 않았다. JavaScript가 없으면 파일/다운로드/합치기 제어는 disabled이며 noscript 안내를 제공한다. 기존 URL·localStorage 키·원래 번호 복사 범위·unknown 표시와 수동 해제 흐름은 보존한다.

주요 selector: `#favorites-backup`, `#download-favorites-backup`, `#favorites-backup-file`, `#favorites-backup-preview`, `#favorites-backup-counts`, `#merge-favorites-backup`, `#cancel-favorites-backup`, `#favorites-backup-status` (`role=status`, `aria-live=polite`).

## 실제 검증과 한계

- 2026-10-20T17:59UTC: `ASTRO_TELEMETRY_DISABLED=1 npm run check`는26파일/오류0/경고0/힌트0으로 통과했다. 그 전 일반 `npm run check`는 telemetry의 `/home/agent/.config/astro` 생성 ENOENT로 실패했으며 타입 실패로 바꾸지 않았다.
- 자식은 서버·배포를 시작하지 않았다. root가 preview artifact를 다시 빌드하고 최신 count 문구를 확인한 뒤 제공한 `http://127.0.0.1:4321`에서 아래 실제 브라우저 검증을 수행했다. 로컬 QA를 운영 배포나 Google 심사 성공으로 해석하지 않는다.
- 연결 Browser skill은 읽었으나 Browser의 실행 `js` 도구가 노출되지 않았음을 root가 확인했다. 이 작업은 기존 Playwright 검증을 명시 허용하므로 시스템 Chromium으로 검증하며 연결된 사용자 브라우저 실행으로 주장하지 않는다.
- 다른 브라우저 엔진·실제 OS 파일 보관/공유·사용자 디스크 삭제 후 복구·Google 계정/신청/심사는 검증하지 않았다. 파일 보관은 사용자 선택이며 자동 동기화가 아니다.

## 실제 브라우저 QA · 2026-10-20T18:04:51Z

시스템 Chromium/Playwright, desktop1440×1100과mobile360×800, 합성 저장 항목만 있는 별도 context를 사용했다. 첫 context에서 검색 결과가0곡이어도 전체 객체·unknown·선행0 metadata가 실제 다운로드에 유지됐고 저장 원문이 불변이었다. 이 다운로드를 다른 context에서 선택해 확인·취소 때 쓰기 없이 보존됨을 확인한 뒤 키보드 Enter로 합쳤다. 기존 객체 metadata를 덮지 않고 새2ID만 추가했으며 unknown은 다시 확인할 항목에 남았다. 실제 개인 파일이나 실제 개인 저장 목록은 사용하지 않았다.

| 확인 | 실제 결과 |
| --- | --- |
| 페이지·본문·오류 overlay | 내 즐겨찾기 title/h1·유효본문, Astro/Vite overlay없음 |
| 실제 다운로드/합치기 | 필터 무관 전체 JSON·객체 보존, 별도context에서 읽기전용확인/취소 후 명시적 합치기 PASS |
| 모바일·키보드 | 360px 수평넘침없음, Space로 native details/Enter로합치기 PASS |
| 제어 높이 | summary48px·다운로드50px·파일56px·합치기50px·취소50px |
| JavaScript 없음 | native details 열기, 다운로드/파일/합치기 disabled, noscript안내 PASS |
| 콘솔 | pageerror/console error·warning0 |
| 실제 화면 | desktop와mobile PNG를 직접 열어 읽고 겹침/잘림/원래 gradient·palette 유지 확인 |

실행은 `/tmp/r51-app-browser-qa.mjs`이며 PASS 결과 `/tmp/r51-app-browser-qa.json`(18:04:43Z~18:04:51Z)을 남겼다. 최초 실행은 핵심 흐름을 통과한 뒤 JSoff의 포괄 `noscript` 선택자가 Layout·본문2개와 일치해 실패했다. 이 첫 실패를 `/tmp/r51-app-browser-qa-failed-first.json`에 보존하고 QA 선택자만 본문에 한정해 재실행했다. 제품 코드를 고쳐 통과한 것으로 쓰지 않는다.

화면은 `/tmp/r51-app-desktop.png`, `/tmp/r51-app-mobile-preview-clean.png`다. 최초 mobile fullPage 캡처는 이미 스크롤한 위치에서 화면 밖의 fixed skip-link까지 중간에 그리는 캡처 artifact가 있어 원본을 보존했다. 소스 수정 없이 scrollY0으로 다시 캡처(18:05:37Z)해 모바일 화면을 확인했다. 원격 CI/preview/production·전체58회귀는 root 후속이며 이 자식의 위 QA로 대신하지 않는다.
