# 원본 UI 기준 검증

2026-10-13 KST · 관련 SEARCH-001, FAVORITES-001, SEO-001, RELEASE-001.

## 대상과 환경

개인 원본 dev 커밋 6dbe5a65d1bd729c09393f746f715f4363089a43을 /tmp/jpop-original-audit에 읽기 전용으로 보존하고, 별도 /tmp/jpop-build-audit 복사본에서 npm ci → npm run build → npm run check:js → npm test를 실행했다. Node 24.19.0, 시스템 Chromium. 실제 jpop.today 운영 응답·배포 SHA를 검증한 것은 아니다.

Browser plugin not available이므로 기존 Playwright를 사용했다. /tmp/jpop-audit.playwright.cjs 외부 설정으로 webServer.cwd를 복사본에 지정하고 desktop/mobile Chromium의 executablePath=/usr/bin/chromium을 사용했다. 기존 테스트 본문과 기대값은 변경하지 않았다. WebKit은 실행하지 않았다.

## 결과

| 검사 | 결과 |
| --- | --- |
| 고정 원본 빌드 | 16 정적 페이지, sitemap 12 URL 생성 |
| JavaScript 문법 | 25 파일 통과 |
| 원본 단위/산출물 테스트 | 12 통과, 실패·건너뜀 0 |
| Chromium E2E | 36 등록, 33 통과, 3 의도적 건너뜀, 실패·flaky 0; 60.7초 |
| desktop/mobile 전용 건너뜀 | desktop의 mobile dialog 검사, mobile의 중복 링크 검사·viewport sweep |
| 운영 대표 흐름 | desktop/mobile Chromium2개 통과(15.6초), 실제 광고 script stub 사용 |
| 실제 광고·WebKit·실기기 | 미실행 |

기존 E2E는 한글 복합 검색→곡 상세→번호 복사→저장→이전 favorites route→재조회, 필터 URL·새로고침·뒤로가기, 최근 필터, 저장 용량/손상 복구, 가사 실패 재시도, 키보드·접근성, 내부 링크·404·.html query 유지, JS 비활성 안내 본문을 검사했다. 기본 광고 script stub은 원본 설정의 UI 테스트 전략이며 광고 게재·권리 확인을 의미하지 않는다.

## 별도 화면 스모크

흐름: 홈 → ‘유우리 피터팬’ 입력 → 결과 1곡 → 즐겨찾기 선택 → aria-pressed=true와 저장 안내.

desktop 1440×1000, mobile 390×844에서 수행했다. 초기 추천 결과와 검색 debounce를 혼동하지 않도록 결과 1곡을 기다린 후 상태를 확인했다. /tmp/jpop-smoke.cjs를 사용했고 기록은 /tmp/jpop-smoke-results.json에 있다.

| 확인 | desktop / mobile 결과 |
| --- | --- |
| 페이지 URL·title·h1 | 의도한 홈과 ‘JPOP 노래방 번호 검색’ 확인 |
| 빈 화면 | main 본문·검색 결과·번호 존재 |
| 프레임워크 오류 화면 | 없음, 정적 앱 |
| 앱 런타임 오류 | pageerror 0 |
| 콘솔 | TJ 외부 앨범 이미지 요청이 net::ERR_TUNNEL_CONNECTION_FAILED; 네트워크 제한과 fallback을 확인 |
| 화면 | 가로 넘침 없음, 번호·저장 UI 확인 |
| 상호작용 | 결과 1곡·저장 상태 true, 양쪽 화면 동일 |

스크린샷은 /tmp/jpop-audit-desktop.png와 /tmp/jpop-audit-mobile.png다. repo에 이미지·trace·원문 가사를 커밋하지 않았다. 클라우드 외부 이미지 접근이 차단되어 실제 앨범 이미지는 이번 결과로 검증하지 못했다. 이 baseline은 새 UI의 회귀 기준이며 현재 앱이나 광고를 다시 설계한 결과가 아니다.

## 운영 대표 흐름 재검증

초기 production Playwright 2개는 ERR_CERT_AUTHORITY_INVALID로 탐색에 실패했다. 플랫폼 CODEX_PROXY_CERT의 공개 CA fingerprint가 기존 Chromium NSS 저장소 OpenAI-nebula-dns 항목과 같음을 확인하고 공식 Chromium 인증서 관리 문서를 읽었다. CA를 추가하거나 TLS 검증을 끄지 않았다. 표준 사용자 NSS 저장소 접근이 가능한 허용된 실행으로 동일 테스트를 재실행해 desktop/mobile2개가 모두 통과했다. 실패는 앱 회귀가 아닌 샌드박스의 인증서 저장소 접근 제약이었다.

실행은 /tmp/jpop-production.playwright.cjs의 baseURL=https://jpop.today, webServer 없이 원본 기존 첫 사용자 흐름 테스트를 선택했다. 한글 복합 검색→저장→상세→TJ번호 복사→이전 fragment favorites→새로고침 유지→번호 목록 복사를 확인했다. 결과 JSON은 /tmp/jpop-production-results.json이다. 원본 테스트의 광고 stub은 유지했으므로 실제 광고 게재/동의/승인은 검증하지 않았다.

운영 /와 공개 js/site-meta.js, js/songs.js, js/chart.js, robots.txt, sitemap.xml은200, /data/jpop-playlist.raw.json과 없는 경로는404였다. 공개 JS3개는 조사 dev의 바이트와 같았다. 이는 관찰 artifact 비교이며 실제 Cloudflare deployment ID/SHA와 연결한 증거는 아직 없다.
