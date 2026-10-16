# 표기 비교 가이드의 실제 판단 보류 사례 · 2026-10-16 14시

`resume-20261016-1400` · [Issue30](https://github.com/jpop-today/jpoptoday/issues/30) · CONTENT-001 / DATA-001 / SONG-001 / SEO-001 / DESIGN-001 · [제품 계약](../product/spec.md#판단-보류-사례와-곡-상세-연결--issue30-2026-10-16).

## 작업 계약과 범위

| 항목 | 기록 |
| --- | --- |
| 앱·디자인 담당 / 통합 | `/root/resume14_app` / root 단독 Git·package·lockfile·CI·빌드·서버·배포·공용 문서 |
| source base / branch | `312c6b687c504cf82090ad26bdd9d829b99a105f` / `codex/30-deferred-guide-20261016` |
| docs base / branch | `4b19570a1dbd4698777aaf461d77999c8ad5bbc6` / `codex/30-deferred-guide-20261016` |
| 조직 규칙 base | `b7aad18fe2f793453b6505bb150f359d05ae3554` |
| 단독 수정 파일 | source `src/content/guides.ts`, `src/pages/guides/[slug].astro`; docs의 이 문서 |
| 의존성 | 04시 공식 metadata 요청·개별 판단 기록, evidence 담당의 재대조, root 빌드·서버, quality 담당의 독립 회귀 |

세 저장소의 AGENTS와 spec의 Issue30, status/roadmap/daily-development/continuous-development를 읽고 담당 범위만 편집했다. Git은 상태·diff·기준 파일 조회만 사용했다. 하위 에이전트, 새 채팅, worktree, 서버, 의존성 설치, provider 재조회, 데이터 변경을 하지 않았다. 최종 결과 SHA·CI·preview·운영·복구 결과는 root 실행 기록을 따른다.

## 사용자에게 바뀌는 동작

기존 `/guides/title-artist-variants/` 제목과 URL을 보존하고 description과 본문 검토일을 2026-10-16으로 갱신했다. 기존 여섯 절 중 1·2·3·4·6은 그대로 유지했다. 5절의 첫 문단만 SPYAIR·JANE DOE의 10월 15일 03시 조회와 samfree·Ado의 10월 16일 04시 조회를 구분하도록 고쳤다. 현재 편집·검토가 새 번호 조회를 뜻하지 않는다고 명시했다. 다른 다섯 가이드의 전체 내용과 검토일은 그대로다.

7절은 금영의 `SAM feat.巡音ルカ`와 관리자료의 `samfree` 사이에서 같은 명의라는 근거를 확보하지 못한 사례다. 8절은 금영 제목 셀 자체가 `..`로 끝나 생략된 부제를 알 수 없는 사례다. 각각 TJ의 공식 확인과 금영의 관측·판단 보류를 구분한다. 조회 성공을 곡 연결 확정으로 표현하지 않으며, 보류가 미수록을 뜻하지 않는다고 설명한다. SAM과 samfree의 동일성, 숨은 부제, 양사의 음원·편곡 동일성을 만들지 않는다.

각 사례에는 기존 곡 상세로 가는 정적 링크 하나를 추가했다. 상세의 TJ 공식 대조 근거·실제 확인일과 금영 관측 상태를 읽고, 업체 공식 검색·현장 기기 확인·이 곡 정보 제보로 이어지는 순서를 본문에서 안내한다. 별도 JS나 라이브러리 없이 템플릿의 선택적 section links를 렌더한다. 새 링크에만 scoped CSS로 `min-height:44px`, 패딩과 기존 파랑 계열 표면·테두리를 적용했다. 전역 CSS와 원래 blue `#4f7cff` → pink `#ff7aa8`, pink/mint/blue 배경은 수정하지 않았다.

| 사례 | 안정 ID / 실제 상세 URL | 검증용 DOM |
| --- | --- | --- |
| samfree | `jpop-playlist-samfree-ルカルカ-ナイトフィ-ハ-f6722acd10` / [/songs/jpop-playlist-samfree-%E3%83%AB%E3%82%AB%E3%83%AB%E3%82%AB-%E3%83%8A%E3%82%A4%E3%83%88%E3%83%95%E3%82%A3-%E3%83%8F-f6722acd10/](https://jpop.today/songs/jpop-playlist-samfree-%E3%83%AB%E3%82%AB%E3%83%AB%E3%82%AB-%E3%83%8A%E3%82%A4%E3%83%88%E3%83%95%E3%82%A3-%E3%83%8F-f6722acd10/) | `section[data-guide-case="jpop-playlist-samfree-ルカルカ-ナイトフィ-ハ-f6722acd10"] .guide-section-links a` |
| Ado | `jpop-playlist-ado-逆光-c8c1ba8b20` / [/songs/jpop-playlist-ado-%E9%80%86%E5%85%89-c8c1ba8b20/](https://jpop.today/songs/jpop-playlist-ado-%E9%80%86%E5%85%89-c8c1ba8b20/) | `section[data-guide-case="jpop-playlist-ado-逆光-c8c1ba8b20"] .guide-section-links a` |

URL은 기존 `contracts/public-catalog.mjs`의 `songHref(id) = '/songs/' + encodeURIComponent(id) + '/'`와 일치하는지 확인했다. 기존 ID, URL, storage, 번호 문자열, ledger77, staging/chart, unknown·미수록 구분, 실패 시 마지막 정상 자료는 이 작업의 수정 대상이 아니다.

## 실제 인용 자료와 날짜

새 provider 요청을 실행하지 않았다. [04시 요청 JSON](../data/provider-review-requests-20261016-0400.json), [04시 개별 판단](../data/provider-review-20261016-0400.md), 실제 ledger77 중 두 TJ confirm 항목을 직접 읽었고 `/root/resume14_evidence`가 제공한 [재대조 자료](../content/guide-review-20261016-1400.md)와 확인했다. 기존 SPYAIR·JANE DOE 출처 네 개의 label/URL/checkedAt는 원형 보존했다. 아래 네 출처만 추가했으며 checkedAt는 요청 JSON의 실제 UTC 문자열 그대로다. 한국 날짜로는 모두 2026-10-16 04시 06분이다.

| 제공사·번호 / 판단 | 추가 source label 전체 | URL | 실제 checkedAt |
| --- | --- | --- | --- |
| TJ 27031 / confirm | `TJ 27031 · ルカルカ★ ナイトフィーバー / samfree(Feat.巡音ルカ)` | [공식 결과](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=27031&strWord=Y) | `2026-10-15T19:06:49.340877Z` |
| KY 43185 / defer | `금영 43185 · ルカルカ★ナイトフィーバー / SAM feat.巡音ルカ` | [공식 결과](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=43185) | `2026-10-15T19:06:53.142654Z` |
| TJ 68868 / confirm | `TJ 68868 · 逆光(映画 'ONE PIECE FILM RED' OST) / Ado` | [공식 결과](https://www.tjmedia.com/song/accompaniment_search?nationType=JPN&strType=16&searchTxt=68868&strWord=Y) | `2026-10-15T19:06:53.828995Z` |
| KY 76509 / defer | `금영 76509 · 逆光(ウタ from ONE PIECE FILM RED).. / Ado` | [공식 결과](https://kygabang.com/chart/search_list.php?mode=SongSearch&val=76509) | `2026-10-15T19:06:56.561637Z` |

금영 두 번호는 ledger confirm/suppress 항목을 추가하지 않은 `observed-unverified`다. 공식 검색에서 번호 행을 찾은 사실과 이 관리곡의 번호로 확정하는 판단은 구분한다. 가사·이미지·권리·기기 모델 지원·현재 수록·Google 계정/심사 상태를 이 자료에서 추정하지 않는다. 공개 본문에는 내부 보고나 광고 신청 준비 문구를 추가하지 않았다.

## 담당 검증과 실행 경계

검증 흐름: `/guides/` → 키보드로 표기 비교 가이드 → 두 사례의 상세 링크 → TJ 공식 확인 근거·금영 관측 상태 → 업체 공식 검색 링크·제보 링크의 키보드 초점.

| 검사 | 현재 실제 결과 |
| --- | --- |
| 기준 SHA와 현재 guide 객체 독립 비교 | PASS. 다른5개 객체 전체 deepEqual, 기존4 source 전체 deepEqual, 기존1·2·3·4·6절 전체와5절두번째문단 보존, 전체6가이드·8절·8source·상세링크2 확인 |
| 추가4 source metadata/URL/checkedAt | PASS. 기존 요청 JSON 첫4 candidate의 전체 제목·가수 포함, URL/checkedAt 정확 일치, 각 caseId·encodeURIComponent 상세 URL 일치 |
| 초기 검증 오류 | Node `execFileSync('git', ...)`이 sandbox `spawnSync git EPERM`을 보고했다. 직접 shell의 읽기 전용 `git show`로 `/tmp` baseline을 만든 뒤 동일 비교를 재실행하여 PASS. 초기 오류를 타입·빌드 오류로 기록하지 않는다. |
| 브라우저 경로 선택 | Browser 스킬은 목록에 있어 실제 읽었다. `ALL_TOOLS`의 browser/cua/js/tool_search 조회 결과 callable 도구가 0개여서 Browser 연결 경로를 실행할 수 없었다. 할당 작업에서 허용한 기존 Playwright fallback을 사용한다. 새 설치·다운로드·브라우저 우회는 하지 않는다. |
| 브라우저 준비 | 기존 `/workspace/jpoptoday/node_modules/@playwright/test/index.mjs`와 `/usr/bin/chromium` 존재 확인. `/tmp/jpop-resume14-app-qa.mjs` 작성. 서버 생성·빌드는 root 담당이다. |
| 렌더링·키보드·반응형 | 실제 preview PASS. 1440×1100·768×1024·360×800과 짧은360×320, 모바일 두 환경 JS-off. 가이드 목록→글→두 상세를 Tab/Enter로 이동하고 뒤로가기 복귀, TJ 근거·TJ/KY 공식검색·제보 링크까지 Tab 초점과 outline3px을 확인했다. 두 사례 모두 각4환경에서 검증. |
| root 로컬 type/build/E2E | root가 최종 privacy 교정을 포함한 preview check0/build/verify3347files 성공을 인계했다. 이 담당이 `/tmp/jpop-1400-preview-e2e.txt`의 실제 `31 passed (1.1m)`을 읽었다. root의 실행이며 이 담당이 명령을 실행한 것으로 기록하지 않는다. |
| root 로컬 production E2E | `/tmp/jpop-1400-production-e2e.txt`에서 신규3회귀 각각 PASS와 전체 `31 passed (57.7s)`를 직접 읽었다. `/tmp/jpop-v044-production-local-final/build-info.json`은 source base에 현재 수정분을 포함한 production/dirty:true/3317곡이다. root 실행 결과이며 새 clean SHA 원격 증거가 아니다. |
| clean 결과 SHA·sameSHA CI·실제 preview/운영·복구 | root의 별도 실제 실행·증거 대기. 이 담당의 로컬 dirty preview와 root 로컬 production 검사가 이를 대신하지 않는다. |

임시 스크립트 실행 형식은 `node /tmp/jpop-resume14-app-qa.mjs <root-owned-server-url> <output-prefix>`다. 새 외부 링크나 mailto는 실제 전송·예약하지 않고 URL·target과 키보드 초점만 검증한다. root의 최종 브라우저 결과와 이 담당의 실제 실행을 따로 남긴다. `/tmp` 스크립트와 PNG는 세션 임시 파일이며 원격에 보존됐다고 주장하지 않는다.

## 실제 preview 브라우저 실행

root가 준비한 `http://127.0.0.1:4321`에서 다음 명령을 기존 Playwright·시스템 Chromium으로 직접 실행했다. localhost 접근은 허용된 추가 network capability를 사용했다. 이 담당의 실행은 exit0이며 첫 실행에 PASS했다. root의 별도31 E2E와 서버의 기본 sandbox socket 실패 기록을 이 담당이 실행한 결과로 섞지 않는다.

```sh
node /tmp/jpop-resume14-app-qa.mjs http://127.0.0.1:4321 /tmp/jpop-resume14-app-preview
```

실제 완료 시각은 `2026-10-16T05:11:22.862Z`(14:11:22.862 KST)다. 대상은 source base `312c6b687c504cf82090ad26bdd9d829b99a105f`에 현재 수정분을 포함한 `dirty:true` 로컬 preview다. `build-info.json`의 songCount3317/currentSongCount3313, ledger SHA-256 `13bfed1c4382365f9dfdd7b548a8874cc680de196772f0767eaec87a5a6743c3`를 읽었다. 이를 clean 결과 SHA의 원격 배포 검증으로 표현하지 않는다.

| 실제 viewport / JS | samfree 링크 너비×높이 | Ado 링크 너비×높이 | 가로너비 / scrollWidth | errors / warnings / ads |
| --- | --- | --- | --- | --- |
| 1440×1100 / on | 800×54.296875 | 800×54.296875 | 1440 / 1440 | 0 / 0 / 0 |
| 768×1024 / on | 704×54.296875 | 704×54.296875 | 768 / 768 | 0 / 0 / 0 |
| 360×800 / off | 328×79 | 328×50.5 | 360 / 360 | 0 / 0 / 0 |
| 360×320 / off | 328×79 | 328×50.5 | 360 / 360 | 0 / 0 / 0 |

각 환경에서 guide title/h1·6개 목록·본문 날짜·8source·canonical·preview noindex, 두 상세의 h1·공식 TJ 번호/checkedAt·금영 관측 번호/공식대조링크없음, 상세와 가이드의 가로넘침 없음, framework overlay 없음, 의미 있는 main 본문을 확인했다. 제보 href에는 기존 곡 주소가 포함됐으며 외부 전송은 하지 않았다. 44px보다 큰 신규 링크와 각 링크의 키보드 focus outline이 유지됐다.

브라우저 증거는 `/tmp/jpop-resume14-app-preview.json`이며 SHA-256은 `53d339c64a2acf300b4a0ef444c67a0c5dc1b07af9b811b7ed82cc5271721a04`다. 실행 스크립트 SHA-256은 `cb505b678c1d07613545e3a9046f8275ecbf7c4619847b91463cf4d998f8f326`이다. 스크립트 문법·source/docs diff whitespace·문서 상대링크도 확인했다. 후속 diff 검사 한 번은 `/workspace`를 Git 저장소로 잘못 지정해 usage를 출력했으며, 두 실제 repo에 `git -C ... diff --check`로 다시 확인하여 PASS했다.

PNG는 `/tmp/jpop-resume14-app-preview-{1440x1100,768x1024,360x800,360x320}-{top,samfree-link,ado-link}.png` 총12개다. 이 담당은 desktop top, tablet samfree, mobile top·samfree·Ado, short Ado 이미지를 실제 열어 브랜드 그라데이션·읽기 폭·줄바꿈·링크 초점·내용 겹침 없음을 확인했다. 화면 밖 내용이 스크롤로 잘린 것을 레이아웃 넘침으로 해석하지 않았다. 이 임시 결과는 원격 영속 증거로 간주하지 않으며 root가 필요한 증거를 보존한다.

### 영속 보존된 로컬 UI 증거

root가 원본과 동일한 [QA JSON](../operations/runs/resume-20261016-1400-ui-qa.json)과 [12 PNG 경로·hash 매핑](../operations/runs/resume-20261016-1400-ui-artifacts.json)을 보존했다. 이 담당이 원본 JSON SHA-256 일치와 매핑의12개 실제 PNG hash 일치를 읽기 전용으로 확인해 PASS했다. 위 `/tmp` 원본·실행 경로는 그대로 남긴다. 대표 영속 파일은 [desktop 첫 화면](design/deferred-guide-20261016-1400-1440x1100-top.png), [tablet 사례](design/deferred-guide-20261016-1400-768x1024-samfree-link.png), [mobile 첫 화면](design/deferred-guide-20261016-1400-360x800-top.png), [mobile samfree](design/deferred-guide-20261016-1400-360x800-samfree-link.png), [mobile Ado](design/deferred-guide-20261016-1400-360x800-ado-link.png), [short Ado](design/deferred-guide-20261016-1400-360x320-ado-link.png)다. 이들은 로컬 dirty preview의 캡처이며 실제 원격 배포 캡처로 바꾸어 기록하지 않는다.

## 두 동료 산출물의 실제 검토

root의 평가 요청 뒤 두 담당의 소유 파일 전체, 실제04시 요청·판단/ledger 입력, root의 unit/type·preview/production E2E 로그, 정책 HTTP/ads.txt/analytics 설정 presence·첫실패/후속성공 영수증, root 개인정보 교정 소스를 읽었다. 확인 시각은 2026-10-16 05:16–05:17 UTC다. 아래는 할당된 수용 기준에 대한 각0..5 평가이며 자기 평가와 변경량 순위는 없다. 계정·권리·실수신 unknown이나 아직 실행하지 않은 root 원격 게이트를 담당 실패로 환산하지 않는다. 사전 동률 순서 quality/evidence/app은 변경하지 않는다.

근거 담당은 정확한 metadata/URL/실제시각·보류 판단을 앱에 제공했으며 공식 정책과 내부 기준을 분리했다. 일반 운영 HTML의 분석태그와 안내 문구가 다른 사실을 발견해 root의 정확성 교정으로 연결한 사용자 가치가 있다. 품질 담당은 구현 데이터를 import하지 않은 두 사례 기대값과 실제 ledger/공개 catalog 대조, SSR/44px·키보드·JS-off·제보·뒤로가기 회귀를 추가했다. 두 target 전체31 PASS 로그를 직접 확인했으며 신규3개도 각각 통과했다. 할당 범위에서 추가 필수 수정을 발견하지 않았다.

현재 상태: 담당 소스·문서 freeze, 직접 preview QA와 두 실제 동료 리뷰 완료. root의 clean 동일 SHA 검사·CI·실제 preview/production·복구는 별도 실제 기록이 필요하다.

```json
{
  "peerReviews": [
    {
      "reviewer": "/root/resume14_app",
      "candidate": "/root/resume14_evidence",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "guide-review 문서의 기존4·추가4 URL/checkedAt와 추가4 full title/artist가 실제04시 요청 JSON 및 TJ2 confirm/KY2 defer와 일치한다. SAM=samfree·생략부제·음원동일성·미수록을 추정하지 않으며 정책 문서는 공식/조건부/내부 기준과 계정 unknown을 구분한다.",
        "evidence": "직접 읽은 metadata JSON·ledger77·동결입력의 실제 bytes hash와 문서값이 일치했다. 정책 문서는 실제 HTTP403과 정상 Node 경로200을 각각 보존하며 checkedAt/URL/범위를 제시한다. root의 policy-http·adstxt·analytics-setting-presence·첫실패/strict후속 영수증을 대조했고 관측 범위를 과장하지 않았다.",
        "value": "명의 미확보와 제목셀 생략을 서로 다른 보류 사례로 설명할 정확한 편집 근거를 제공했다. 일반 운영 HTML의 Insights 태그와 분석 없음 안내 불일치를 찾아 root 개인정보 교정으로 연결했으며 실제 신청/권리/문의 후속과 독립 개발을 구분했다.",
        "maintainability": "배정된 두 docs 파일만 소유하고 source/ledger/배포를 조작하지 않았다. base SHA·실행주체·이전조회/이번재검토·정확URL·실행한 일과 미실행을 기록하고 source privacy 교정의 로컬 수용 및 원격 pending을 분리해 다음 실행이 이어갈 수 있다."
      },
      "reason": "콘텐츠 근거와 신청 준비 점검의 할당 기준을 충족한다. 실제 분석 안내 불일치를 찾아 필요한 제품 교정에 연결했고 출처·시각·정책·상태 경계를 보존했다. 필수 보완 없음.",
      "evidence": [
        "/workspace/docs/content/guide-review-20261016-1400.md",
        "/workspace/docs/operations/adsense-readiness-20261016.md",
        "/workspace/docs/data/provider-review-requests-20261016-0400.json",
        "/workspace/jpoptoday/data/reviewed/provider-corrections.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-policy-http.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-adstxt-observed.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-analytics-setting-presence.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-baseline-first-http.json",
        "/workspace/docs/operations/runs/resume-20261016-1400-baseline-http-retry.json",
        "/workspace/jpoptoday/src/pages/privacy.astro"
      ],
      "acceptance": {
        "status": "accepted",
        "requiredFixes": [],
        "remainingDependencies": [
          "root의 clean 동일 SHA CI·원격 preview/운영·복구 및 개인정보 실제 운영 본문 재확인",
          "운영자의 실제 계정·소유권/신청·전체자료 권리·문의 실수신 등 별도 사실 확인"
        ]
      }
    },
    {
      "reviewer": "/root/resume14_app",
      "candidate": "/root/resume14_quality",
      "scores": {
        "correctness": 5,
        "evidence": 5,
        "value": 5,
        "maintainability": 5
      },
      "criterionReasons": {
        "correctness": "신규 E2E는 실제04시 기록의 두 ID·원문·번호·시각을 독립 기대값으로 두고 공개 catalog/ledger의 TJ provider-confirmed와 KY observed-unverified/evidence:null/correction:null/string을 구분한다. SSR 링크·44px·360px JS-off Tab/Enter·상세 근거·제보·뒤로가기·오류/광고0을 검증하며 기존 E2E는 날짜/개수 두 줄만 수정했다.",
        "evidence": "두 소스 E2E 전체와 quality 문서, 기존 파일 diff를 실제 읽었다. root preview 로그31 passed(1.1m)와 production 로그31 passed(57.7s)에서 새3개 회귀 각각 PASS를 확인했다. unit314 및 check0error/0warning/0hint도 읽었으며 quality 자신의 test collection과 root 실제 실행을 혼동하지 않는다.",
        "value": "공식 검색이 성공해도 KY 확정으로 승격되지 않아야 하는 제품 경계를 catalog·상세·가이드 동선으로 검증한다. JS가 없어도 이용자가 정확한 기존 곡 주소와 근거/제보에 도달하는 회귀를 제공해 이번 콘텐츠 보강을 실제 사용 흐름으로 보호한다.",
        "maintainability": "가이드 구현이나 UI projection을 import하지 않고 metadata 기대값을 별도 정의했으며 공유 URL 보안 계약만 재사용한다. 안정ID 기반 section scope와 작은 helpers, 환경 baseURL/선택적 screenshot을 쓰고 배정 소스 두 개·소유문서만 변경해 기존 테스트와 데이터/설정을 보존했다."
      },
      "reason": "품질 하네스의 할당 기준을 충족하며 실제 두 target 전체 회귀 로그로 검증됐다. fake provider 조회·외부 메일 발송 없이 중요한 상태 차이와 SSR·모바일 키보드 경로를 보호한다. 필수 보완 없음.",
      "evidence": [
        "/workspace/jpoptoday/e2e/deferred-guide.spec.ts",
        "/workspace/jpoptoday/e2e/title-artist-guide.spec.ts",
        "/workspace/docs/engineering/guide-deferred-quality-20261016-1400.md",
        "/workspace/docs/data/provider-review-requests-20261016-0400.json",
        "/workspace/jpoptoday/contracts/provider-corrections.mjs",
        "/tmp/jpop-1400-preview-e2e.txt",
        "/tmp/jpop-1400-production-e2e.txt",
        "/tmp/jpop-1400-unit.txt",
        "/tmp/jpop-1400-check-final.txt"
      ],
      "acceptance": {
        "status": "accepted",
        "requiredFixes": [],
        "remainingDependencies": [
          "root의 clean 동일 SHA CI·원격 preview/운영·복구 증거 연결",
          "임시 로그·캡처의 영속 인계와 최종 결과 SHA 기록"
        ]
      }
    }
  ]
}
```

## 같은 실행 후속 보완 · 운영 JavaScript-off 이메일 링크

2026-10-16 10:16 UTC, root가 [추가 제품 계약](../product/spec.md#issue30-운영-브라우저-실패-후-보완--javascript-없는-문의)을 먼저 기록하고 실제 운영 실패 해결을 배정했다. 이 단락은 이전 소스 freeze·로컬/preview 성공·원래 두 peer JSON을 수정하지 않는 후속 범위다. 기준은 제품 merge M `86090cf8ac9fca79ebe1d890c8f1bb3c592f1aa6`이다.

[실제 운영 실패 영수증](../operations/runs/resume-20261016-1400-remote-production-failure.json)을 직접 읽었다. run38885429674/job116921918724에서 실제29 PASS/2 FAIL이며, samfree·Ado의360px JavaScript-off 제보 href를 `new URL`로 읽는 단계가 `TypeError: Invalid URL`로 실패했다. root가 확인한 Cloudflare 이메일 난독화는 기존 mailto를 상대 `/cdn-cgi/l/email-protection#…`로 바꾸며 복원 decoder에는 JavaScript가 필요하다. 로컬31·preview31·strict27 PASS는 이 실제 운영 제보 실패를 대신하지 않는다. 메일의 실제 발송·수신을 조사한 결과는 아니다.

root는 [Cloudflare 공식 이메일 난독화 문서](https://developers.cloudflare.com/waf/tools/scrape-shield/email-address-obfuscation/)를 `2026-10-16T10:14:45.197Z`에 실제 HTTP200으로 읽고 특정 HTML 범위를 `<!--email_off-->…<!--/email_off-->`로 제외하는 안내를 인계했다. 이 담당은 해당 문서를 새 HTTP로 조회한 것이 아니며 root의 인용 범위와 추가 스펙을 읽었다.

후속 소유권은 `src/pages/songs/[id].astro`, `src/pages/contact.astro`, `src/pages/privacy.astro`의 기존 이메일 anchor 부분과 이 문서 끝의 후속 기록뿐이다. 개인정보 페이지는 기존 root 소유에서 이메일 anchor 부분만 명시 인계받았다. 각 기존 anchor 바로 앞뒤에 정적 `email_off` HTML 주석 한 쌍을 추가했다. 범위가 세 곳뿐이므로 새 컴포넌트·의존성·스크립트·wrapper 요소를 추가하지 않았다. 곡 상세의 `report` 생성식, 기존 공개 주소·label, subject/body·곡 원제/가수·안정 URL·HTML entity 의미는 바꾸지 않았다. 다른 본문·날짜·번호·URL·브랜드/CSS·데이터도 수정하지 않았다. zone 전체 이메일 설정이나 계정·광고·메일 발송을 조작하지 않았다.

이 담당이 실제 수행한 확인은 세 템플릿과 새 스펙·실패 영수증 읽기, 수정 소스의 주석 쌍과 기존 `report` 생성식 재읽기다. Git·type/check·빌드·서버·테스트·원격 HTTP·브라우저·CI·배포는 이 후속 범위에서 실행하지 않았다. 소스에 주석이 있다는 사실만으로 최종 HTML이나 Cloudflare 처리의 성공을 단정하지 않는다.

현재 후속 상태는 **최소 소스 패치 완료·root 검증 대기**다. root가 두 target 빌드의 실제 마커·동일 mailto/subject/body를 확인하고, 확장 E2E와 실제 새 SHA preview/운영에서 JavaScript-off 직접 제보·문의 링크 및 키보드 동선을 검증해야 한다. 같은 새 SHA의 CI·실제 운영·복구를 통과하기 전 v0.4.4 릴리스 완료로 올리지 않는다. evidence·교체 감사의 추가 읽기 수용도 실제 결과 이후 별도 기록한다.
