# 즐겨찾기 번호 목록의 상태 보존 · 앱 구현 인계

작업은 [소스 Issue38](https://github.com/jpop-today/jpoptoday/issues/38), [제품 스펙의 Issue38](../product/spec.md#즐겨찾기-번호-목록의-상태-보존--issue38-2026-10-18), FAVORITES-001/DATA-001/CONTENT-001에 연결한다. 실행 ID는 `resume-20261018-0700`, 담당은 `/root/resume38_app`이다. 소스 기준 SHA는 `3ca14f80a1e0dbdf055882d195751c4d889a0db6`, 문서 기준 SHA는 `dbc3196441c01368d621c6b65b29c1ddede1a1e2`, 통합 브랜치는 `codex/38-favorites-export-status`다. 최종 결과 SHA·CI·배포는 root의 통합 기록을 따른다.

## 파일 범위와 실제 변경

- 소스 `contracts/favorites-export.mjs`: 순수 함수 `formatFavoritesExport(songs): string`을 추가했다. 원 배열을 바꾸거나 저장소·클립보드에 접근하지 않으며, 전달한 모든 곡을 같은 순서로 줄바꿈해 내보낸다. 빈 배열은 빈 문자열이다.
- 소스 `src/scripts/app.ts`: 기존 복사 버튼이 페이지 표시분과 별개인 전체 `filtered` 배열을 formatter에 전달한다. 빈목록의 기존 알림, 성공 시 알림, clipboard 거부 시 수동 선택 dialog, 저장원문·미해결 ID·URL·필터·더보기 동작은 기존 경로를 사용한다.
- 소스 `src/pages/favorites.astro`: 현재 검색·필터에 맞는 모든 저장곡과 더보기 전 곡까지 포함한다는 안내, 업체별 상태 의미, JavaScript 필요 안내를 작성했다. 기존 버튼 이름 `번호 목록 복사`를 보존하고 두 안내 ID를 `aria-describedby`로 연결했다. 기존 색상·레이아웃 CSS는 수정하지 않았다.
- 이 문서는 위 세 파일의 설계·인계 근거다. 타입 선언·기존 E2E 호환 기대값·공용 파일·Git·CI·배포는 root가 통합하며, 가이드 본문은 자료 담당의 별도 파일 범위다.

## 내보내기 계약

기존 제목·가수·업체 순서 `제목 · 가수 | TJ … | 금영 …`를 보존한다. 업체마다 독립적으로 판정한다.

| 입력 상태 | 출력 |
| --- | --- |
| 기존 `hasConfirmedNumber(song, provider)`가 참 | `TJ 00123 (공식 확인)` 또는 `금영 00123 (공식 확인)` |
| `observed-unverified`, `conflict === false`, 기존 계약의 `/^\d+$/` 문자열 번호 | `TJ 00123 (관측·미확인)` 또는 금영 같은 형식 |
| 번호 없음·unknown·suppress 후 상태·충돌·잘못된 번호·근거가 부족한 provider-confirmed | `TJ 확인 필요` 또는 `금영 확인 필요`, 번호 비노출 |

표의 `00123`은 형식을 설명하는 합성 예시이며 실제 제공사 확인 자료가 아니다. 관측 번호에 새로운 자릿수·0 제외 제약을 도입하지 않고, 선행 0과 원 문자열을 보존한다. 공식 확인은 기존 엄격 판정의 current/playlist·공식 근거 조건을 그대로 따른다. 한 업체의 근거를 다른 업체로 확대하거나 확인 필요를 공식 미수록으로 바꾸지 않는다. 과거 관측 번호 `correction.previousNumber`는 읽거나 내보내지 않는다.

기존 browser-safe `hasConfirmedNumber`와 `provider-lookup`만 재사용하며 Node 모듈이나 근거 전체 객체의 새 복사를 만들지 않는다. 제목·가수·번호·근거·날짜·권리 정보를 새로 채우지 않는다. 동결 원본·ledger·안정 ID·브랜드 CSS를 변경하지 않는다.

## 검증과 인계 상태

담당자는 세 저장소 AGENTS와 Issue38 스펙, 기존 복사·필터·저장 코드, `hasConfirmedNumber`와 제공사 조회 규칙을 읽고 시작 시 소스의 미커밋 diff가 없는 것을 확인했다. formatter API·출력 형식을 품질 담당과 자료 담당에게 공유했고, root 결정으로 관측 번호는 기존 `/^\d+$/` 문자열 계약을 보존했다.

이 인계 시점에는 담당자가 새 서버·브라우저·전체 테스트·CI·배포를 실행하지 않았다. 품질 담당은 독립 raw/ledger oracle의 상태·잘못된 입력·원문 불변과 실제 clipboard/거부·수동선택·필터 전체·30곡 초과·빈목록·키보드/360px/JavaScript 없음 검증을 맡는다. 최종 type·artifact·브라우저·동일 SHA CI/preview·재현·복구·실제 운영 판정은 root의 실행 증거로만 확정한다. 동료 두 명의 읽기 전용 평가 후 필요한 보완을 인계한다. AdSense 신청·승인 상태는 이 구현으로 확인되지 않는다.
