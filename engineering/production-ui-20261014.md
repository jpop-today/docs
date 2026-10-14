# Issue#13 · 운영 전환 UI와 색인 검증

2026-10-14 KST. 담당 `/root/resume_20261014_ui`. source base `9d8cfb716e3ab5194e6711d19ec2b10a297089ad`, docs base `d7ccc98bb20c8d997c0ac20e8264528c63b77aab`, branch `codex/13-production-transition-20261014`. [ADR-006](../adr/006-reviewed-corrections-and-production.md)의 DATA-001/SEARCH-001/SONG-001/FAVORITES-001/SEO-001/DESIGN-001을 검증한다.

## 범위와 흐름

소유 파일은 source `e2e/app.spec.ts`, `tests/publication.test.mjs`, 이 문서다. 스타일·앱·공용 package/CI·Git·배포는 관리자가 맡는다. Browser plugin과 browser skill이 현재 도구 목록에 없어서 frontend-testing-debugging skill의 허용 경로에 따라 기존 Playwright와 `/usr/bin/chromium`을 사용한다. TLS·프록시·CA 우회는 하지 않는다.

검증 흐름은 홈 → 검색·번호 필터 → 저장·새로고침·기존 주소 → 잘못된 번호의 복사 제외와 공식 대조 근거 → 최신 고정 차트의 canonicalId 링크·순위다. 요청 본문·응답hash를 공개 카탈로그에 넣지 않고, unknown을 미수록으로 표시하지 않는지 확인한다.

원본 `#4f7cff → #ff7aa8` 135도 브랜드 gradient와 분홍 rgba(255,122,168,.18)·민트 rgba(77,199,183,.16)·파랑 `#eef5ff` 배경을 실제 브라우저 computed style로 확인한다. 360px 화면에서 차트와 기본 검색의 가로 넘침도 검사한다. 같은 번호가 다른 곡을 가리키는 경우 ID를 합치지 않는다. 차트 canonicalId는 검증된 불변 observation 대응을 사용하며, 제공사 OST 부제와 기존 목록의 제목 표기가 다를 수 있어 표시 문자열 동일성으로 재결합하지 않는다.

## 추가한 게이트

`tests/publication.test.mjs`는 preview의 전면 noindex, production의 archived/관측값/unknown/번호 충돌/근거 누락 상세 noindex, 검색·개인 저장·호환·404 noindex를 검증한다. 확인된 현재 곡과 검토 가이드의 production 색인만 허용한다.

기존 12개 E2E에 원본 gradient·차트 모바일 overflow와 실제 sitemap/metadata 경계 2개를 더했다. 실제 `/catalog.json`에서 공식 확인·충돌 없는 현재 곡을 선택하고 preview에서는 sitemap `<loc>`가 없는지, production에서는 해당 상세와 홈만 허용된 경로 목록에 포함되는지 확인한다. 미확인/보관 상세·검색·즐겨찾기·호환 페이지는 두 target 모두 sitemap에서 제외한다.

## 실행 결과

최종 reviewed ledger 43개 갱신 후 관리자가 새 preview 빌드와 artifact 검사(3,333 HTML·3,341 files·3,317 ID·색인 상세0개)를 완료했고, 그 artifact로 로컬 E2E를 실행했다. 대상은 `http://127.0.0.1:4321`, Node24.19.0, 시스템 Chromium, desktop1440×1100·mobile360×800이다. 작업 중인 base SHA와 dirty=true artifact의 로컬 검증이며 clean release SHA·원격·실제 운영 배포 증거는 관리자가 별도로 연결한다.

```sh
node tests/publication.test.mjs
PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium \
JPOP_SCREENSHOT_DIR=/tmp/jpop-ui-20261014-preview npm run test:e2e
```

명시된 추가 네트워크 권한으로 로컬 브라우저를 실행했으며 proxy·CA·TLS를 그대로 유지했다. publication 테스트 3/3, Playwright 14/14가 통과했다(브라우저15.0초). 기본 `node --test` 격리 실행은 이 환경에서 파일1개 통과로만 표시되어 개별3case의 이름별 결과가 출력된 direct 실행을 증거로 사용했다. 전체 하네스의 실제 서브테스트 수는 관리자의 명시적 격리 설정 실행으로 확인한다.

| 검증 | 실제 결과 |
| --- | --- |
| 페이지 식별·본문·framework overlay | 홈 title·h1·검색 입력을 확인했고 framework 오류가 없었음 |
| 콘솔 | 검색/뒤로가기 pageerror 없음, gradient→mobile차트 흐름의 pageerror·console.error 없음 |
| 검색·저장·호환 | AND 검색·필터/URL/뒤로가기, 문자열/객체/미해결 ID, 손상 저장값 보존·쓰기 실패 session 복구, 기존 detail/filter/favorites URL 통과 |
| 억제/확인 번호 | 억제 번호 복사 없음·이유/미수록 아님·공식 근거 표시, 제공사 필터에서 해당 ID 제외, 기존 ID의 즐겨찾기와 bulk copy에 유효 번호/unknown 적용 |
| 차트 | 100행, 실제 snapshot의 1/20/50/100순위 canonicalId 링크와 기록 기간 일치 |
| 색인·공개 경계 | preview 홈/상세/utility noindex·sitemap0개, 가사/raw/staging/ledger/AGENTS/scripts/reports/releases 경로 실제404·광고 요청 코드 없음 |
| 시각 검토 | desktop/mobile·확인/억제 상세·모바일 차트 PNG5개를 직접 열어 읽었음. gradient·본문·버튼 식별 가능, 글/메뉴/버튼 겹침과 문서 가로 넘침 없음 |

스크린샷은 저장소 밖 `/tmp/jpop-ui-20261014-preview/desktop.png`, `mobile.png`, `reviewed-detail.png`, `suppressed-detail.png`, `chart-mobile.png`다. 확인 상세의 중복 번호 conflict 경고와 억제 상세의 original/movie 차이 근거가 보존되어 있다. Firefox·WebKit·실제 모바일 기기·운영 배포 브라우저는 이 실행에서 검사하지 않았다. production mode의 정책 부정 사례는 unit으로 검증했고, 실제 production HTML/sitemap과 원격 origin은 후속 artifact/브라우저 실행 전까지 완료로 쓰지 않는다.
