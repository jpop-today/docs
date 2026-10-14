# 운영 전환 콘텐츠·정책 게이트 · 2026-10-14

`ADS-001` · `CONTENT-001` · `DATA-001` · `SONG-001` · `PRIVACY-001` · `SEO-001` · `RELEASE-001`. [소스 Issue #13](https://github.com/jpop-today/jpoptoday/issues/13), [제품 스펙](../product/spec.md), [ADR-006](../adr/006-reviewed-corrections-and-production.md), [심사 기록 기준](adsense.md)을 따른다. 검토 담당은 `/root/replacement_20261014_production_gate`이며 이 문서만 편집한다. 코드·Git·CI·배포는 root가 수행한다.

작업 base는 소스 `9d8cfb716e3ab5194e6711d19ec2b10a297089ad`, 문서 `d7ccc98bb20c8d997c0ac20e8264528c63b77aab`, branch `codex/13-production-transition-20261014`이다. 검토는 한국 시간 2026-10-14 14:31–14:32에 실제 파일과 로컬 산출물, 공식 Google 문서를 읽으며 진행했다. 후속 같은 SHA 검증·실제 운영 반영은 root의 실행 근거를 받은 뒤 별도로 갱신한다. 문서 검토가 Google의 정책 준수 판정이나 심사 승인을 뜻하지 않는다.

## 현재 판정과 증거 범위

광고 없는 앱의 로컬 콘텐츠·자료 경계는 아래 검사를 통과했다. 운영 전환 완료는 아직 이 문서에서 확정하지 않는다. 이 작업의 처음 인계받은 운영 기준은 개인 저장소 `kim75503/todayjpop/dev`의 `94d615f5e355f4b00ebbb824547ac6dfa034ad18`이며, 이 담당자가 새 운영 배포를 실행하거나 직접 Cloudflare canonical deployment를 조회한 결과가 아니다.

| 게이트 | 직접 읽거나 실행한 결과 | 남은 검증 |
| --- | --- | --- |
| 사용자 가치·탐색 | 홈/검색/상세/가이드/즐겨찾기·정정 문의가 명확히 연결됨. 네 가이드는 제목 찾기, 업체 번호 차이, 개인 저장, 차트 기간을 설명함 | 모바일·키보드·검색/복사/저장 실제 브라우저 결과는 root의 동일 SHA 기록으로 연결 |
| 번호 정확성·unknown | ledger 43entry 중 confirm30/suppress13. 확인된 번호가 최소 하나인 ID16개. 원본 3,317 ID와 staging revision6d 유지. suppressed 값은 null/unknown이며 미수록으로 쓰지 않음 | 실제 번호·정체성 근거는 [행별 검토](../data/provider-corrections.md), 전체 목록 검증은 미완료 |
| 정정한 번호 복사 | 튜링 러브 상세에 잘못된 KY `57803`의 `data-copy` 없음, 확인한 TJ `52536`의 복사 버튼 있음. 공식 결과·확인일·정정 이유와 버전 경고 렌더 확인 | 동적 검색·필터·전체 목록 복사와 기존 저장값 회귀는 root 브라우저 하네스 결과로 확인 |
| 운영 색인 설계 | production 허용 경로는 일반12개와 current·공식근거·무충돌 상세12개, 합계24개. 3,317 상세 중 나머지3,305개는 noindex/sitemap 제외 설계. confirmed ID16개 중4개는 남은 공유번호 conflict로 제외 | 최종 clean production artifact·실제 jpop.today의 meta/header/robots/sitemap 재검증 |
| preview 경계 | 아래 clean preview 로컬 검사에서 3,333 HTML 전체 noindex, sitemap URL0, robots 전체 차단, 광고 코드 없음 | 새 최종 SHA의 preview 배포 HTTP/브라우저 증거 필요 |
| 가사·이미지·내부 자료 | public catalog는 곡 표기·번호 상태·검토 근거만 사용. verifyDist가 가사/외부 이미지 payload·광고 코드·raw/staging/ops/reports/releases 경계를 검사해 통과 | 번호/메타데이터의 재배포 허용 범위 전체는 미확인. 비공개 처리는 권리 허락의 증거가 아님 |
| 개인정보 안내 | 계정 없음, 로컬 검색, 검색 URL의 전달 가능성, local/sessionStorage, 클립보드, Cloudflare 요청 처리, 외부 링크, 문의자료 처리, 광고/분석 없음이 구현 설명과 대응함 | 실제 광고를 도입하기 전에 수집·쿠키·공급업체·거부/철회·적용 지역 동의 흐름을 다시 반영 |
| 운영·문의 신뢰 | 소개는 독립 서비스와 TJ/KY 비제휴를 명시. 기존 공개 이메일과 mailto, 정정 제보에 필요한 곡 주소·업체·제목·번호·출처 안내를 확인함 | 이메일을 실제 발송하지 않았고 수신함 소유·운영자의 응답/보관 기간 실행은 확인하지 않음 |
| 같은 SHA·복구·자동 덮어쓰기 | root의 단일 통합·복구·개인 Git 자동배포 pause 계약을 읽음 | 실제 동일 SHA CI/preview/운영, 가역 pause, production rollback과 HTTPS 검증은 실행 증거를 기다림 |
| AdSense | 실제 계정·소유권 확인·검토 요청·결과를 읽지 않음. 상태 `unknown` | 운영자가 실제 계정 절차를 진행하고 Google 통지/상태를 확인해야 함 |

## 직접 실행한 로컬 검사

소스 `0be17904174e673b56a021873085aceee09243f3`의 clean preview 로컬 산출물에 대해 `node --input-type=module`로 `verifyDist()`를 호출했다. `build-info.json`의 검사 시작/종료 바이트가 같은지도 확인했다.

| 항목 | 실제 결과 |
| --- | --- |
| target / dirty | preview / false |
| 파일 / HTML / 상세 | 3,341 / 3,333 / 3,317 |
| current / archived | 3,313 / 4 |
| indexable | 0 |
| catalog SHA-256 | `73b50974c365bc3accf552beae6a41d0c565c7f80da8281b11672d6f26a4a1fe` |
| ledger SHA-256 | `0644fc5e74b1d3b456a0fc5d5e939fa55eafee7f238e91d2730bb609e215628f` |
| chart SHA-256 | `971bacf9cb65e5b7176e8a5aa59394d2c46ce00d0676aa033178dc11ae7540b9` |

처음 공유 `dist`에서 dirty production 산출물의 sitemap24를 읽었으나 root가 preview 빌드로 전환하면서 `expectedTarget:production` 검사는 target 불일치로 한 번 중단됐다. 이는 잘못된 운영 산출물 판정이나 배포 실패 증거가 아니다. 공유 빌드 중간 관측을 제외하고 이후 안정된 clean preview 검사만 위 확정 결과로 기록했다. production 후속 검사는 root가 고정 산출물 경로와 SHA를 제공한 뒤 수행한다.

## 자체 콘텐츠 보강

첫 파일 검토에서 네 가이드에 공개 편집 담당·본문 검토일이 없고, 일부 설명이 모든 표시번호와 목록 복사를 관측 번호로 표현했다. root는 실제 프로젝트 편집 표기와 오늘 본문 검토일을 추가하고 공식 확인/관측/정정한 번호 및 차트 요청기간과 응답 검증을 구분하는 수정을 진행한다고 통지했다. 이 문서 작성 시 수정 완료로 계산하지 않으며 최종 파일/산출물 재검토 후 갱신한다. 가이드 본문 확인일은 전체 제공사 자료 확인일을 뜻하지 않는다.

가이드 수나 색인 URL24개라는 수치는 Google이 충분한 고유 콘텐츠로 인정했다는 증거가 아니다. 검색·번호 비교·정정 근거·저장 기능의 실제 효용과 검토된 자체 설명을 계속 개선하며, 가이드의 반복 표현이나 수천 개 정형 상세를 고유 편집물로 포장하지 않는다. 가사·앨범 이미지·YouTube 캡처·근거 없는 난이도/음역을 추가하지 않았다.

## 오늘 다시 읽은 공식 Google 근거

한국 시간 2026-10-14 14:31–14:32에 아래 여섯 공식 도움말, 14:37에 게시자/프로그램 정책을 연결된 웹 도구의 직접 `open` 결과와 본문으로 확인했다. 로그인·실제 계정 화면은 확인하지 않았다. 상세 정책 적용 기준은 [전일 정책 조사](adsense-policy-review-20261013.md)도 참조한다.

| 공식 근거 | 직접 확인한 의미 |
| --- | --- |
| [페이지 준비](https://support.google.com/adsense/answer/7299563?hl=ko) | 고유 콘텐츠·방문자 관련성·사용하기 쉬운 탐색을 평가해야 함. 외부 자료를 가져온 사실이나 양만으로 가치가 생기지 않음 |
| [사이트 연결](https://support.google.com/adsense/answer/7584263?hl=en) | 코드/ads.txt/meta 소유권 확인 방법, 실제 검토 요청, 공개 운영 사이트와 크롤러 접근을 확인해야 함 |
| [사이트 관리](https://support.google.com/adsense/answer/12131223?hl=en) | 소유권 확인 이후 정책 검토를 수행하고 실제 AdSense 사이트 상태가 Ready인 뒤 광고 게재 가능 |
| [계정 설정](https://support.google.com/adsense/answer/7402256?hl=ko) | 결제 정보·사이트 연결·전체 사이트 검토가 별도 절차. 2026년6월부터 본인 확인이 요구될 수 있고 실제 요청은 운영자가 처리 |
| [개인정보 필수 콘텐츠](https://support.google.com/adsense/answer/1348695?hl=ko) | Google 광고 도입 시 쿠키/공급업체/개인 맞춤 광고·거부 방법을 실제 설정에 맞게 공개해야 함 |
| [ads.txt 가이드](https://support.google.com/adsense/answer/12171612?hl=ko) | ads.txt 사용은 적극 권장. 실제 계정의 정확한 publisher 값을 사용하고 공개 파일·계정의 상태를 별도 확인 |
| [Google 게시자 정책](https://support.google.com/adsense/answer/10502938?hl=ko) | 저작권 침해·허위 제휴, 가치 부족/미완성/부가 가치 없는 복제 화면, 광고로 인한 사용자 동작 방해를 제한. 현재 무광고·공개 경계 검사가 전체 정책 준수 판정을 대신하지 않음 |
| [AdSense 프로그램 정책](https://support.google.com/adsense/answer/48182?hl=ko) | 본인 광고 클릭·클릭 유도·자동 클릭/노출·인위적 트래픽 금지. 기능 검증은 광고 없는 artifact로 수행하며 실제 광고를 클릭하거나 노출 실험을 하지 않음 |

사이트 연결 문서의 한국어 URL 직접 요청은 Google `sorry` 리디렉션으로 도구 오류가 났다. 같은 공식 문서 ID의 영어 URL은 본문을 읽는 데 성공했다. 한국어 요청을 성공한 것으로 기록하지 않았고 제한을 우회하거나 계정에 로그인하지 않았다.

## 운영자가 처리할 계정 단계와 기술 후속

| 단계 | 필요한 실제 정보·행동 | 진행 경계 |
| --- | --- | --- |
| 계정·법적 동의 | 실제 운영자 계정, 본인정보·지급/세금·지역 자격·약관 및 요청된 신원 확인 | 에이전트가 추측하거나 허위로 대신 제출하지 않음. 공개 문서나 artifact에 개인 신분/결제 자료를 남기지 않음 |
| 사이트 소유권 | AdSense에 등록된 정확한 jpop.today URL과 계정이 제공한 검증 방법·publisher 값 확인 | 원본에 값이 있었다는 이유로 현재 계정 소유권을 확정하지 않음. 검증값을 받은 뒤 광고 없는 meta 경로 등 구현 가능 |
| ads.txt | 실제 계정의 행과 현재 root 파일·계정 상태 대조 | 임의 publisher 값 생성 금지. ads.txt 존재·Authorized는 사이트 Ready와 별개 |
| 콘텐츠/권리 | 번호·메타데이터 재배포 허용 범위와 문의 처리, 자체 콘텐츠·제공사 근거 갱신 | 권리 확인이 안 된 가사/이미지는 공개하지 않음. 번호 검색 확인을 전체 재배포 허락으로 해석하지 않음 |
| 검토 요청·결과 | 제출 시각·운영 SHA·Google 실제 통지/정책센터/사이트 상태 | 실제 Ready를 본 뒤에만 승인으로 기록. 거절 이유는 실제 통지와 당시 artifact에 연결 |
| 광고 활성화 | 실제 광고 범위, 개인정보/CMP/동의·거부·철회, 모바일 오인 클릭 방지 검토 | 검색/개인 목록/미확인·빈 상세/404·preview는 기본 광고 제외. 실제 광고 자동클릭·인위적 노출 테스트 없음 |

계정 정보가 아직 없다는 이유로 번호 정정·콘텐츠 개선·광고 없는 앱의 검증/PR/복구/운영 전환을 중단하지 않는다. 현재 계정 단계의 unknown과 기술적으로 독립적인 배포 게이트를 분리해 보고한다.
