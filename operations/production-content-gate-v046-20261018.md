# v0.4.6 콘텐츠·공개 범위 게이트

Issue38 / FAVORITES-001·DATA-001·CONTENT-001·SEO-001·RELEASE-001·ADS-001. 제품 M은 `110c583ec3460b4ca5e3b153deef7278d856f8a3`이며 실제 결과는 [실행 영수증](runs/resume-20261018-0700.json)에 연결한다. 이 문서의 작성은 배포·복구·Google 심사 통과를 뜻하지 않는다.

## 이번 공개 변경과 보존

즐겨찾기 번호 목록의 업체별 공식 확인·관측·미확인·확인 필요를 내보낸다. 기존 `hasConfirmedNumber`만 공식 확인을 허용하고, 충돌·제외·없는 번호는 번호 없이 남긴다. 현재 검색·필터의 모든 저장곡을 복사하며 저장 원문·미해결 ID·기존 URL과 번호 문자열을 보존한다. 기존 선곡 가이드의 복사 절과 실제 본문 편집일만 바꿨다. 다른 다섯 가이드의 본문·출처·확인일은 보존했다.

새 제공사 요청·번호 확인일·가수/버전 동일성·가사·이미지·권리 허락·난이도·심사 결과를 만들지 않았다. ledger84·동결 staging/chart·3317 ID·원본 identity와 global CSS의 다섯 입력 해시는 [최종 보존 검사](runs/resume-20261018-0700-main-preservation.json)에서 base와 일치했다. 수집·승격 경로는 바꾸지 않았으며 마지막 정상 자료 보존 계약은 기존 전체 단위 검사의 범위다. 이번 변경을 최신 차트 수집이나 전체 번호/권리 검토 완료로 확대하지 않는다.

## 실제 확인한 게이트

| 영역 | 확인한 증거와 한계 |
| --- | --- |
| 계약·타입 | M의 canonical332/332, Astro26파일0error/0warning/0hint. 독립 raw+ledger+번호빈도 oracle의 전체3317곡 문자열·반례와 저장 원문 불변을 포함한다. |
| 같은 M CI | [main CI](runs/resume-20261018-0700-main-ci.json) app/contracts success. [선택 로그](runs/resume-20261018-0700-main-ci-logs.json)는 단위332·타입0·preview44/production44를 확인한다. |
| 전체 산출물 | 두 target 각각3347파일/3336HTML을 두 번 clean 빌드해 파일 집합과 모든 파일 hash 일치. preview index0, production index49. [재현](runs/resume-20261018-0700-reproducibility.json)의 범위이며 전체 CI ZIP과의 바이트 비교는 미실행이다. |
| 광고·색인 | verifier가 공개 artifact의 허용 경계·광고 없음과 target별 robots/sitemap을 검사했다. favorites는 양 target 모두 noindex다. 실제 광고를 실행하거나 클릭하지 않았다. 배포 후 외부 설정 전체를 증명하는 검사는 아니다. |
| 사용자 흐름 | 최초 H의 전체44는42통과/2실패였다. noscript 부모 추출과 의도적으로 바뀐 편집일의 검사 가정을 보완해 H1 focused7을 통과했고 M CI는 전체44씩 통과했다. [수정 전후 PNG](../engineering/design/favorites-export-20261018-0700/)와 [영수증](runs/resume-20261018-0700-ui-evidence.json)을 보존한다. OS 실제 clipboard 권한·Firefox/WebKit·필드 성능을 측정했다고 하지 않는다. |
| 독립 검토 | 실제6peer·4기준 합계 evidence40/app40/quality38, root의20파일 hash 대조와 기존 evaluateTeam 검사. 품질 문서의 잘못된 fragment를 두 리뷰어가 구체적으로 지적했다. 원문 snapshot·점수·실패는 보존하고 실제 교체 담당이 보완·감사한다. [원 평가](runs/resume-20261018-0700-reviews.json)와 [교체 감사](../engineering/favorites-export-replacement-20261018-0700.md). |
| 실제 preview·운영·복구 | API pointer, strict 본문27, 같은 M 원격44, 이전 v0.4.5 실제복구→새 M 재적용의 성공 여부는 실행의 개별 영수증으로 최종 판정한다. API 성공이나 이 표로 pending 단계를 통과로 만들지 않는다. |

## AdSense와 운영자 단계

이 변경의 사용자 가치는 다른 사람에게 전달한 번호 메모에서도 신뢰 수준을 잃지 않는 것이다. 글 수·번호 수·버전1.0·Actions 자동배포 binding을 Google의 보편적 신청 조건이나 승인 보장으로 사용하지 않는다. 정책 해석은 [기존 공식 조사](adsense-readiness-20261016.md)와 [심사 기록 기준](adsense.md)을 따르며 이번에 새 Google 정책 확인일을 만들지 않았다.

실제 AdSense 계정·소유권·신청·Google 통지/승인은 unknown이고 기술 준비는 preparing이다. 운영자는 실제 사이트 상태와 필요한 본인정보·약관/법적 동의, 자료 이용 범위·권리, 문의 실수신·처리, ads.txt의 실제 계정 일치를 확인해야 한다. 공개 파일의 존재·판매자 형식은 계정 일치나 승인 증거가 아니다. 실제 광고/CMP 도입은 대상 지역·설정·동의/거절/철회를 별도 확인하며, 이번 preview는 전체 noindex·무광고다. Actions CF binding의 안전한 인계는 내부 운영 후속이며 관리자 managed 배포와 구분한다.
