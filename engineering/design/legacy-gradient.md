# 원본 그라데이션 복원

2026-10-13 · [Issue #9](https://github.com/jpop-today/jpoptoday/issues/9) · SEARCH-001, FAVORITES-001 · [제품 스펙](../../product/spec.md).

## 작업 계약과 기준

| 항목 | 확인한 값 |
| --- | --- |
| 담당 | `/root/daily_20261013_brand` — 색상 구현·원본 대조·대비 계산; 통합·브라우저·CI·배포는 `/root` |
| 브랜치 | `codex/daily-development-20261013` |
| 앱 기준 SHA | `6cbdcc3f63132b9abe948a5edfe67727aec9fe03` |
| 문서 기준 SHA | `5c3faaca5a77d3fd16cef865db98c68a7c204eed` |
| 소유 파일 | 앱 `src/styles/global.css`, 문서 `engineering/design/legacy-gradient.md` |
| 원본 | 읽기 전용 `/tmp/jpop-original-audit`, dev 커밋 `6dbe5a65d1bd729c09393f746f715f4363089a43`, `css/style.css` |
| 원본 CSS Git blob | `b537c8cc9986e17826f6f3924f74309b0bd9ba14` |
| 원본 CSS SHA-256 | `43565ca0247914cae66afe5a403887a599115ae9153d2748c7cbc68ece6a7fd2` |
| 결과 SHA | 작업자 커밋 없음. 통합 담당자가 최종 커밋·CI·배포 SHA를 별도 기록한다. |

사용자가 원래 그라데이션 사용을 명시했다. [기존 검색 화면 계약](search-ui.md)의 구성·타입·컨트롤 크기·동작은 유지하며, 흰 배경과 진홍색 강조를 원본 배경·브랜드 그라데이션과 파란색 상태로 바꾼다. 기존 콘셉트와 달라지는 색상은 이 요청에 따른 의도한 변경이다. 새 콘셉트 이미지·공개 이미지·가사·곡 정보는 생성하지 않는다.

완료 기준은 원본 CSS 값 재확인, 두 시그니처 그라데이션과 파란색 상태 연결, 기존 배치 유지, 주요 글자·컨트롤 대비 계산, 기존 10개 E2E의 통합 담당자 회귀 검증이다. 데이터·ID·URL·저장 키·검색 흐름·공유 파일·Git 작업은 담당 범위 밖이다.

## 적용한 색상

원본 `css/style.css:23`의 `body` 배경을 공백 서식만 바꾸어 사용한다.

```css
background:
  radial-gradient(circle at 12% 10%, rgba(255, 122, 168, 0.18), transparent 30%),
  radial-gradient(circle at 90% 6%, rgba(77, 199, 183, 0.16), transparent 28%),
  linear-gradient(180deg, #fbfcff 0%, #f7f9fc 48%, #eef5ff 100%);
```

| 적용점 | 구현 |
| --- | --- |
| 페이지 바탕 | 위 세 레이어를 `body`에 적용, 루트 fallback은 원본 `#f7f9fc` |
| 헤더·footer의 `today` | 원본 `.brand-mark`의 `linear-gradient(135deg, #4f7cff, #ff7aa8)`을 텍스트에 clip. 미지원 브라우저는 `#284fc7` |
| 검색·선택된 필터·저장·focus | 원본 `--accent: #315ee8`; 기존 focus 두께·offset 유지 |
| 검색 hover·본문 링크·링크 hover | 원본 `--accent-dark: #284fc7` |
| 저장된 별 내부 | 원본 `--accent-soft: #edf2ff` |
| 보조 글자·placeholder·관측 안내 | `--muted: #535d6d`; 배경 레이어 위 가독성을 위한 의도한 보정 |
| 검색 입력 배경 | 원본 검색 입력처럼 흰색. placeholder와 입력 글자 대비 확보 |

본문 검정·표면·구분선은 기존 화면의 값으로 유지한다. 브랜드 그라데이션은 로고에만 쓰며 기능 문구에는 단색을 쓴다. 검색 버튼은 원본의 단색 파란색 형태다. 배경과 브랜드에 연결된 두 그라데이션의 색·각도·위치·alpha·stop을 바꾸지 않는다.

`Layout.astro`의 `theme-color`는 작업자가 수정하지 않았다. 원본 바탕 `#f7f9fc` 적용을 제안했고, 통합 담당자가 적용한 값을 파일 읽기로 확인했다.

## 실행한 확인

2026-10-13 06:23:56 UTC까지 작업자가 아래 결과를 확인했다. 시작 시 앱·문서 저장소의 `git status --short`는 모두 비어 있었으며 실제 HEAD는 위 기준 SHA와 같았다.

| 명령·확인 | 실제 결과 |
| --- | --- |
| `git rev-parse <원본 SHA>` 및 `git rev-parse <원본 SHA>:css/style.css` | 원본 커밋·blob 재확인 |
| `git show <원본 SHA>:css/style.css \| sha256sum` | 위 원본 SHA-256과 일치 |
| `node --input-type=module -`에서 PostCSS parse·선언 대조 | CSS parse 성공; 색·배경·clip·fill·outline·색상 변수 외 398개 선언은 기준 앱과 같음. 쉼표 주변 공백은 정규화 |
| 같은 PostCSS 대조 | `body` 배경은 공백 정규화 후 원본과 일치; 브랜드 배경과 파란색 세 토큰은 원본 값과 일치; 이전 진홍색 세 값 없음 |
| `git diff --check -- src/styles/global.css` | 통과 |
| `rg -n '^test\(' e2e/app.spec.ts` 대조 | 기존 10개 E2E 시나리오 확인; 작업자가 실행하지는 않음 |
| 도구 목록에서 Browser 검색 | Browser plugin 도구 없음. 통합 담당자가 시스템 Chromium·Playwright fallback으로 확인 예정 |

일회성 대조 실행에서 Node `execFileSync('git', …)`가 환경의 `spawnSync git EPERM`으로 중단되어, 셸 `git show`로 원본과 기준 CSS를 `/tmp`에 읽은 후 Node가 해당 파일을 읽도록 바꿨다. 첫 raw 값 대조는 font-family 쉼표 공백 차이를 보고했으며, 공백 정규화 후 위 398개 선언 대조가 통과했다. 이 과정에서 앱 동작이나 배치 선언은 바꾸지 않았다.

이 시점의 결과 `global.css` SHA-256은 `ff5c98214a712c285f3f211300e82843bd532c36b57b87259abb9c8ab8c6a134`다.

## 색 대비와 남은 검증

WCAG 상대 휘도식으로 계산했다. 배경은 선형 그라데이션의 세 stop 각각에 민트 alpha `0/0.16`, 핑크 alpha `0/0.18`의 조합을 모두 합성해 보수적으로 비교했다. 실제 화면 위치에 따른 측정값과 구분한다.

| 전경 / 배경 | 계산 대비 |
| --- | --- |
| 본문 `#20242c` / 보수적 배경 조합 최저 | 11.18:1 |
| 보조 글자 `#535d6d` / 보수적 배경 조합 최저 | 4.78:1 |
| 본문 링크 `#284fc7` / 보수적 배경 조합 최저 | 4.97:1 |
| 흰색 / 검색 `#315ee8` | 5.39:1 |
| 흰색 / 검색 hover `#284fc7` | 6.92:1 |
| 파란색 `#315ee8` / 저장 별 `#edf2ff` | 4.81:1 |
| 파란색 focus / 보수적 배경 조합 최저 | 3.87:1 |

단색 UI 대비를 확인했다. 로고 그라데이션은 위 일반 문구 대비 표에 포함하지 않는다. 렌더링·모바일 overflow·키보드 사용·실제 클릭 동작은 작업자가 실행하지 않았으며, 브라우저 screenshot 대조와 기존 10개 E2E는 통합 담당자 검증에 남는다. 두 동료 평가도 최종 통합 결과를 대상으로 요청 후 실시한다. CI·미리보기·운영 배포 완료를 이 기록으로 주장하지 않는다.
