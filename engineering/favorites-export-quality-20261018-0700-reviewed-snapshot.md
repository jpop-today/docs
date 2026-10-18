# 즐겨찾기 번호 내보내기 품질 인계 · 2026-10-18 07시 재개

- Issue: [jpoptoday#38](https://github.com/jpop-today/jpoptoday/issues/38), run `resume-20261018-0700`.
- 스펙: [FAVORITES-001/DATA-001/CONTENT-001/DESIGN-001](../product/spec.md#즐겨찾기-번호-목록의-상태-보존--issue38).
- 담당: 실제 collaboration `/root/resume38_quality`. 기준 source `3ca14f80a1e0dbdf055882d195751c4d889a0db6`, docs `dbc3196441c01368d621c6b65b29c1ddede1a1e2`.
- 소유: 신규 `tests/favorites-export.test.mjs`, 신규 `e2e/favorites-export.spec.ts`, 기존 `e2e/app.spec.ts` bulk 복사 기대문자열 한 행, 이 문서. Git·package/lock·CI·공용계약·서버·브라우저 실행·배포는 root만 수행한다.
- 초기 인계 시 소스 diff 없음. 이후 다른 담당/root 소유 변경은 읽기만 했으며 덮어쓰지 않았다. 새 외부조회나 Google 심사 확인을 수행하지 않았다.

## 검증할 사용자 흐름

`/favorites/` → 저장곡/검색·업체 조건 선택 → 번호 목록 복사 → 텍스트만 받은 사람도 각 업체의 공식 확인·관측·확인 필요를 구분한다. 복사를 거부한 브라우저에서는 전체 텍스트를 수동선택할 수 있고, 빈 결과는 이전 클립보드를 덮지 않아야 한다. 화면의 30곡·두 번째 페이지와 관계없이 현재 조건의 모든 저장곡을 내보내야 한다.

## 독립 기대값과 반례

unit과 E2E의 실제자료 기대값은 formatter·`hasConfirmedNumber`·`matchesSong`을 호출하지 않는다. 원본 staging, reviewed ledger의 confirm/suppress, 업체별 correction 적용 후 번호 빈도에서 상태를 계산한다. formatter의 실제 입력을 만드는 기존 projection은 unit의 대상 입력에만 사용한다. 실제자료 oracle은 metadata title/artist/version의 사람 검토나 새 공식조회가 아니며 기존 ledger 검토 결과를 소비한다.

2026-10-17 22:07:11 UTC 읽은 동결 입력은 3,317곡/ledger84, 공식 확인 TJ32·금영27·교집합27, 업체 중 적어도 하나가 충돌인 실제 ID4개였다. 이 수치를 미래 고정 기대값으로 넣지 않고 현재 입력에서 정확한 ID·문자열·순서를 비교한다. TJ 확인 전체가30개보다 큰지 명시적으로 확인하여 페이지 잘림 검사가 빈 성공이 되지 않게 한다.

- 순서·원제·원가수·선행0 `00123`/`00517`/`00042`의 exact 문자열과 deep-freeze 불변성을 확인한다. 인위적인 날짜/공식 URL은 `synthetic-export` 합성 fixture이며 실제 조회 증거로 기록하지 않는다.
- 공식 확인은 완전한 업체별 증거·엄격한 공식 numeric URL·current/playlist·conflict:false가 필요하다. 누락/공백/타입 오류·다른 번호/업체·HTTP·가짜 호스트·credentials·fragment·추가/중복 query·숫자 타입 오류·archived/chart-only를 반례로 둔다. 잘못된 confirmed를 관측 번호로 다시 노출하지 않는다.
- 관측은 기존 문자열 숫자 계약을 보존하여 `0`, `0000`, 11자리 숫자를 관측·미확인으로 남긴다. 공식 확인의1~10자리/비allzero 조건을 관측에 새로 강요하지 않는다. null/숫자타입/문자·공백·전각/음수는 확인 필요다.
- suppress의 `previousNumber`, 충돌 번호, unknown/unlisted/기타 상태를 출력에 섞지 않는다. 실제 SAM/Ado의 TJ 확인·금영 관측과 original-ver 양업체 suppress를 각각 확인한다. 출력에 미수록을 만들어 붙이지 않는다.
- readFavorites 원문 공백·object metadata·미해결 ID를 보존하고 내보내기가 쓰기를 발생시키지 않음을 확인한다.

## 브라우저 계약 5개

1. 실제 mixed 저장곡(확인/관측·보류/없는 업체 번호/suppress/충돌)과 미해결 ID를 seed한다. 실제 `/catalog.json`의 관련 provider projection을 raw+ledger oracle과 대조하고 성공한 `navigator.clipboard.writeText` 호출/전체 text/성공 안내/비모달/저장 원문을 검증한다.
2. 360px clipboard 거부에서 readonly textarea의 전체 선택·focus, Enter/Space 실행, Escape와 닫기 버튼 Enter 후 원래 버튼 focus,44px 양축/가로 넘침/모달 경계를 검증한다. 거부된 경우 이전 mock clipboard 값도 보존한다.
3. 공식 확인 TJ 저장곡>30에서 `page=2`의 정확한 visible ID를 확인하되 복사는 첫 페이지 포함 전체다. 금영 공식 확인을 추가하면 page가 해제되고 AND 교집합만 내보낸다. 조건 전후 호출 전체와 저장 bytes를 비교한다.
4. 검색으로 Pretender 한 곡, 없는 검색어0곡, 미해결 ID만 남은 저장목록을 검사한다.0곡의 복사는 clipboard를 쓰지 않고 안내하며, 이전 성공 내용이나 초기 sentinel을 덮지 않는다.
5. 안내와 `aria-describedby` 연결·전체/더 보기 포함 설명·세 상태·unknown≠미수록·원래 gradient/palette를 확인한다.360px JS-off에서 버튼 비활성·실제 제한 설명·넘침 없음, favorites noindex/광고 코드 없음도 검증한다.

기존 bulk-copy 회귀는 첫 reviewed suppress 곡의 두 업체를 `확인 필요`로 exact 비교한다. 기존 번호복사·근거·미수록 비추정·원번호 비노출 assertion은 유지했다. 실제 OS clipboard 허용 여부를 자동클릭해 추정하지 않으며, browser API 성공/거부를 결정적으로 제어해 앱 호출·fallback을 검사한다. 실제 광고 요청/노출 검사는 하지 않는다.

## 실행 사실과 미실행

품질담당은 root에 명령을 먼저 알리고 `/workspace/jpoptoday`에서 다음만 실행했다.

| 명령/관측 | 실제 결과 |
| --- | --- |
| `node --test tests/favorites-export.test.mjs` | exit0, 격리 runner의 파일 aggregate1pass. 내부7개를 확인했다고 계산하지 않음 |
| `node --test --test-isolation=none tests/favorites-export.test.mjs` | 7개 test/7pass/0fail, 약168ms. 전체3317곡 독립 export 문자열 포함 |
| root 인계 메시지 | canonical `npm test`332/332, Astro26files0error/0warning/0hint라고 root가 실제 관측하여 전달함. 품질담당 직접 실행은 아님 |

E2E5개는 최초 인계 시 root 실행 대기였다. root가 서버/port·canonical browser·target build/CI·실제preview/production·rollback 검증을 단독 수행한다. Screenshot 옵션은 root의 외부 `JPOP_SCREENSHOT_DIR`에 desktop/mobile-fallback/mobile-guidance/JS-off4종을 남기게 했다. 이 단계에서 최종 브라우저 통과·배포 성공·AdSense 신청/승인을 주장하지 않는다.

### 최초 실제 브라우저 실패와 수정

root가 source H `6edc05c512f999f38c07c1ba0d3e38892198a133`에서 실행한 전체44개는42pass/2fail이었다. 실제 로그 `/tmp/jpop38-head-e2e.log`, [실행 JSON](../operations/runs/resume-20261018-0700-head-browser.json)을 읽었다. 새 clipboard 성공/거부·키보드/전체선택/44px/360px·>30/page2·AND·empty/raw 보존4개는 통과했지만, 새 JS-off 안내 검사는 `<noscript>` parent의 `toContainText`가 빈 문자열을 받아 실패했다. 기존 비교 가이드 날짜 검사는 이번에 실제 갱신한 setlist까지 과거날짜로 고정해 별도로 실패했다. 후자는 root의 추가 소유범위이며 품질담당은 수정하지 않았다.

실제 compiled `dist/favorites/index.html`에는 `<noscript><p>개인 저장 목록을 읽고 번호 목록을 복사하려면 JavaScript가 필요합니다.</p></noscript>`가 있었다. 설치된 Playwright `shouldSkipForTextMatching`은 `NOSCRIPT` 자체를 text matching에서 명시적으로 제외하므로 parent 선택이 적절하지 않았다. 품질담당은 JS-off context를 유지하고 `noscript > p`의 **정확한 전체 문구와 실제 visible**을 요구하도록 좁혀 수정했다. 검사를 HTML 문자열 존재만으로 대체하거나 안내/비활성/넘침 검사를 제거하지 않았다. root의 [실제 JS-off 관측](../operations/runs/resume-20261018-0700-js-off-observed.json)은 childVisible/bodyIncludesNotice=true와 정확한 문구를 확인했다. 수정 후 test rerun은 별도로 기다리며 최초 실패를 통과로 덮어쓰지 않는다.

프런트엔드 품질 skill은 읽었으나 품질담당은 Browser/Playwright를 직접 실행하지 않았다. root는 현재 Browser 도구 가용 여부와 실제 선택 경로를 별도 증거에 남긴다.

## 인계와 평가 경계

최종 result SHA·root의 실제 브라우저/전체검증·6개 동료평가·교체·배포는 통합 기록을 따른다. 이 문서는 아직 수행하지 않은 peer 평가나 최저 순위를 만들지 않는다. 테스트 변화량과 테스트 수는 worker 순위 근거가 아니다.
