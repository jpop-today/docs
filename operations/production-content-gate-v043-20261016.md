# v0.4.3 차트 확인곡 탐색 · 교체 담당 읽기 감사

작업 [Issue27](https://github.com/jpop-today/jpoptoday/issues/27) · 상위 [Issue6](https://github.com/jpop-today/jpoptoday/issues/6) · `resume-20261016-0400` · DATA-001/CONTENT-001/SEO-001/DESIGN-001/HARNESS-001/RELEASE-001. 감사 담당 `/root/replacement04_gate`, 통합·Git·공용 파일·CI·배포는 root다. 이 담당의 유일한 쓰기 파일은 이 문서다.

source base `ce1b49f1ab6cedb64dca7cd2b215e72aafc76e50`, docs base `5d1964dfeacdad5c6b2c81d2522ce3aadb83de05`, 조직 base `b7aad18fe2f793453b6505bb150f359d05ae3554`, branch `codex/27-chart-confirmed-20261016`을 [실행 계약](runs/resume-20261016-0400.json)·[제품 스펙](../product/spec.md) 및 실제 연결된 Issue27 본문에서 읽었다. 세 저장소 AGENTS와 상태·roadmap·daily-development·continuous-development의 운영 경계를 읽었다. 원본 소스·동료 문서·평가·실행 증거를 보존하며 새 점수를 만들지 않는다.

## 교체와 검토 범위

[실제 여섯 평가](runs/resume-20261016-0400-reviews.json)와 각 원문을 읽었다. 원 담당 세 명은 두 동료 합계가 모두 40/40이고 정확성·증거·사용자 가치·유지보수성 합계도 각각 10으로 완전 동률이다. 사전 `run.tieBreakOrder`의 app/evidence/quality 순서로 앱 담당을 현행 명단에서 제외하고, root가 실제 interrupt의 previous state `completed` 및 본 교체 spawn을 기록했다. 이는 품질 우열을 입증하지 않으며 물리 삭제가 아니다. 원 구현과 평가를 지우거나 차이를 만들기 위한 감점을 하지 않았다.

첫 감사 읽기는 `2026-10-15T23:15:34Z`(16일08:15KST)까지의 파일·기존 로그·이미지를 대상으로 한다. 저장된 안전 metadata·ledger·원71 복사본·동결 입력을 읽기 전용 비교했고 영속 PNG12개와 수정 전 모바일 PNG를 각각 직접 열었다. 공식 제공사 추가 요청·raw HTML 재파싱·테스트/빌드/서버/브라우저/CI/배포·Git 조작은 이 담당이 실행하지 않았다. 연결된 GitHub 요청은 공개 Issue27 읽기뿐이다.

## 자료·콘텐츠 정확성

[자료 검토 본문](../data/provider-review-20261016-0400.md), [실제 요청·후보·별도 결정 JSON](../data/provider-review-requests-20261016-0400.json), [root 독립 대조](runs/resume-20261016-0400-evidence-audit.json)와 현재 ledger를 읽었다. 실제 요청8개는 HTTP200·정확한 공식 request/final URL·문자열 번호·실제 checkedAt·응답 bytes/hash·전체 표시 제목/가수를 가지며 후보8개는 `unreviewed`로 보존된다. 별도 reviewer/reviewedAt/reason이 있는 결정8개 중 confirm6개만 ledger에 대응한다. 저장된 candidate/decision/새 ledger evidence의 ID·provider·번호·URL·시각·전체 제목/명의·hash·이유를 직접 대조했다.

| 동결 순위·곡 | TJ 결정 | 금영 결정 | 수용 근거와 한계 |
| --- | --- | --- | --- |
| 15 · ルカルカ★ナイトフィーバー / samfree | 27031 confirm | 43185 defer | TJ 전체 feature 명의가 동결 차트와 일치. 금영 `SAM feat.巡音ルカ`의 SAM=samfree 동일성은 확보하지 않아 관측 유지 |
| 16 · 逆光 / Ado | 68868 confirm | 76509 defer | TJ 영화 OST 부제 전체가 동결 차트와 일치. 금영 표시 제목의 `..` 생략 부분을 재구성하지 않음 |
| 17 · KICK BACK / 米津玄師 | 68677 confirm | 44855 confirm | 양 업체의 전체 OP 부제를 보존하고 명시적 공백·인용부호 차이만 대조. 오디오 동일성은 추정하지 않음 |
| 18 · レオ / 優里 | 52773 confirm | 57826 confirm | 번호·전체 제목/가수 일치. 다른 편곡/기기별 지원까지 확대하지 않음 |

원 ledger71 전체 객체·순서·최상위 metadata와 닫기 전 bytes prefix는 `/tmp/jpop-0400-base-ledger.json`과 직접 비교해 보존됨을 확인했다. 결과는77항목(confirm64/suppress13)이며 새 suppress는 없다. original71 ordered canonical SHA는 `35487404af46c08e2bf0471e0bf05fff088c0545b69639a57d9c928390797689`, 결과 ledger bytes SHA는 `a2d8ea8e9eba737a38a1f2968a0a1687498bf04fb0f8742ea7f42eedaef8f708`이다. staging/chart/identity/helper/global CSS의 실제 파일 bytes hash는 저장된 시작 hash와 모두 일치했다. 기존 수집실패/부분결과/쓰기 실패의 마지막 정상 보존 계약과 helper는 변경하지 않았다.

새4 unit의 전체 소스를 읽었다. 원71·동결 staging/chart·3317 ID·100개 순위/ID, exact6개 근거, 이전/이후 cohort projection의 전체 ID/URL·비대상 provider/identity 전체 객체를 검사한다. 같은 제목의 다른 Ado 곡으로 근거가 전파되지 않고 unknown/null을 유지하며, 보류KY2는 관측 문자열 번호·근거null로 남는다. 보류를 suppress/미수록으로 바꾸지 않으며 후속 독립 검토의 append를 금지하지 않는다.

공식 raw 응답은 저장하지 않았다. 저장된 metadata 사이 대응과 파일 hash는 독립 대조했지만 실제 응답 원문 재파싱/응답 hash 재계산은 불가능하며 수행했다고 주장하지 않는다. 번호 공개검색의 읽기는 재배포 허락·가사/이미지/음원 권리의 근거가 아니다. 자료 권리·SAM 별칭·생략 부제·오디오 동일성·기기별 지원·실제 Google 계정/신청/심사는 미확인이다.

## 앱·사용자 가치·유지보수성

`src/pages/chart.astro`, 기존 `confirmedNumbers`/`SongRow`와 새 E2E2개 전체를 읽었다. 차트의 frozen canonicalId join·100개 고유 ID·원래 순위·요청 기간/fetchedAt·TJ 원문 출처·보관차트/최신 아님/요청기간과 응답 집계기간 구분을 보존한다. `current/playlist`·provider-confirmed·충돌 없음·실제 근거/정확한 공식 numeric URL인 업체만 기존 계약으로 선별한다. 업체별 ID Set은 TJ19/금영17, 전체 합집합19/분모100이며 19+17을36곡으로 표시하지 않는다. 0 또는 목록 부재가 공식 미수록이 아님을 설명한다. 금영 목록도 기존 TJ 순위 순서이며 별도의 금영 차트를 만들지 않는다.

SSR 업체 바로가기→`chart-${stableSongId}` anchor→원100행→기존 곡 상세/공식 근거로 연결된다. fragment는 순위에 종속되지 않고 기존 URL·저장 ID와 복사/저장 구현을 건드리지 않았다. 새 클라이언트 JS·의존성·공용 CSS·계약을 추가/변경하지 않았다. 기존 blue `#4f7cff`→pink `#ff7aa8` 및 pink/mint/blue 배경을 재사용한다.

E2E는 UI의 집계 helper를 import하지 않고 served catalog와 동결 chart의 hash/100행/순위/URL·업체별 자격·합집합을 독립 계산한다. 공식 numeric URL만 기존 `officialLookupMatches` 계약을 재사용해 www금영·port·중복/추가 query와 다른 oracle을 만들지 않는다. 360px JS off에서 마지막 TJ·첫 금영 바로가기를 키보드로 열고 stable hash·viewport·실제 제목 hit target·기존 상세/공식 URL를 확인한다. 기존 storage/legacy URL 회귀를 대체하지 않는다. 범위를 좁힌 소스/회귀와 명시적 DOM 속성·native anchor는 후속 유지보수에 적절하며 현 시점 필수 코드 보완을 발견하지 않았다.

## 실제 모바일 결함·수정·시각 증거

[앱 문서](../engineering/chart-confirmed-navigation-20261016-0400.md), [품질 문서](../engineering/chart-confirmed-quality-20261016-0400.md), [실패 기록](runs/resume-20261016-0400-local-failures.json)을 읽었다. 첫 QA navigation 대기 누락은 임시 script 오류였고 앱 수정 없이 대기를 보완했다. 이어 자동 overflow/동작 PASS였어도 [수정 전 실제 PNG](runs/resume-20261016-0400-ui-before-fix-mobile-anchor.png)에서 공식 확인 badge와 금영 라벨 겹침·badge 세로 줄바꿈이 실제로 보였다. 이를 통과로 숨기지 않았다.

페이지 한정 CSS는 양 업체를 label/button grid와 아래 한 줄 badge로 분리하고 번호/저장 hit target을44px로 키웠다. 최종 [QA JSON](runs/resume-20261016-0400-ui-qa.json)은 root 재빌드 후 `2026-10-15T19:15:18.515Z–19:15:24.787Z`에 실제 실행한 dirty preview다. 1440×1100 JS on·360×800 JS off·1280×768 JS on의 source base/dirty:true 및 catalog·ledger·chart semantic hash가 일치한다. 본 감사의 새 브라우저 실행이나 clean 원격 검증으로 확대하지 않는다.

직접 연 영속12PNG는 `engineering/design/chart-confirmed-20261016-0400-`의 desktop/mobile/short 각각 first·summary·last-shortcut·anchor다. 업체별19/17/전체19·100분모와 설명, 실제 기존81위 마지막 바로가기/원행·상세 연결, 원래 그라데이션을 읽었다. 수정 후 mobile-anchor는 양 업체 라벨·번호·badge가 분리되고 단일행으로 보이며 짧은 desktop도 마지막 link가 가려지지 않는다. 모바일 JSON 전체100행은 번호 버튼181개 최소94×44px·저장100개44×44px·badge44개 최대높이16px를 기록한다. 새 영속 E2E의100행 bbox 검사는 overflow만으로 놓쳤던 provider 내부 겹침/세로 badge를 직접 겨냥한다.

세 viewport의 pageerror/console warning·error/실패요청/광고요청0 및 가로 overflow false를 읽었다. [대비 영수증](runs/resume-20261016-0400-ui-contrast.json)의 실제 선택3표본은6.52/6.23/6.18:1이며 이 감사에서 새 pixel 대비 계산을 실행하지 않았다. 사이트 전체 대비/접근성·WebKit/Firefox·실제기기·스크린리더·필드 성능은 미실행이다.

## 실행 증거와 수용 단계

| 게이트 | 실제 읽은 근거·결과 | 감사 수용 경계 |
| --- | --- | --- |
| 전체 unit | `/tmp/jpop-0400-unit.txt`:314/314, fail/skip0,14,489.143116ms; 새 cohort4개 포함 | 현재 dirty local source의 root 실행 수용 |
| 타입 | `/tmp/jpop-0400-check-final.txt`:0 error/0 warning/0 hint | E2E 파일 수집까지 대신하지 않음 |
| 최초 E2E collection | `/tmp/jpop-0400-preview-e2e-final.txt`:JSON import에 type:json 누락, 브라우저28개 미시작 | 해결 전 실제 실패 보존;28개 브라우저 실패로 세지 않음 |
| 정정 후 preview | `/tmp/jpop-0400-preview-e2e-after-jsonfix.txt`:28 PASS/50.3s, 새2검사 각3.0s | readFileSync/JSON.parse 정정 후 실제 로컬 결과 수용 |
| production local | `/tmp/jpop-0400-production-e2e.txt`:28 PASS/50.5s, 새2검사 각2.9s | 실제 운영 origin 배포 증거와 구분 |
| artifact | run의 local preview3347files/3336HTML/0index, local production3347files/3336HTML/45index, 후보0.4.3 | root 로컬 기록 읽기. clean source M 전체 재현은 후속 |
| clean CI·전체 재현 | 아직 전달된 실제 후속 영수증 없음 | pending |
| 실제 preview HTTP·원격 브라우저 | 아직 전달된 실제 후속 영수증 없음 | pending; 로컬28을 원격 성공으로 복사하지 않음 |
| 실제 운영·이전 복구→새 복원 | 아직 전달된 실제 후속 영수증 없음 | pending; 기존v0.4.2 운영을 새0.4.3으로 표시하지 않음 |
| tag/GitHub Release·Release Discord·완료 Discord | 각각 실제 후속 영수증 없음 | 각각 pending; 시작 Discord sent와 구분 |

현 단계 결론은 **자료·앱·회귀·수정 후 로컬 UI를 통합하는 데 필수 보완 없음**이다. 이는 운영 배포/릴리스 게이트 전체 수용이 아니다. root가 후속 clean SHA/CI·두 target 전체 재현·실제 preview/운영·복구/복원·Release/두 알림의 실제 JSON을 전달하면 이 문서의 후속 항목에 각 증거를 실제 읽고 수용 또는 미확인을 추가한다. 계정 unknown을 기술 승인/Google 승인으로 바꾸지 않는다.
