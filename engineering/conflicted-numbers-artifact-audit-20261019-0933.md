# Issue41 교체 담당의 산출물 검증 감사

- 작업: [Issue41](https://github.com/jpop-today/jpoptoday/issues/41), 실행 `manual-20261019-0933`.
- 계약: [제품 스펙의 Issue41 절](../product/spec.md#충돌-번호의-화면개별-복사-보류--issue41-2026-10-19), DATA-001/SONG-001/FAVORITES-001/CONTENT-001/DESIGN-001.
- 담당: `/root/run41_replacement`. 원래 세 담당의 여섯 읽기 평가 후 총점40/40/40 동률이라는 root의 인계에 따라, 사전 순서 quality/evidence/app로 원품질 담당을 active에서 교체한 후속 담당이다. 물리 삭제나 품질 열등을 뜻하지 않는다. 최초 세 담당의 파일·평가·실행 증거를 보존한다.
- source base: `1fd6ece7afb4ea7ebddf318f3ef6fa9dafa57951`; docs base: `ea1b6cac498ada996b5ed6dc6a001d0270a37f98`.
- branch: `codex/41-conflicted-number-controls`. 이 감사는 미커밋 작업 파일 기준이며 병합·운영 SHA의 증거가 아니다.
- 쓰기 범위: source `tests/conflict-artifact.test.mjs`, 이 문서만. 공유 `scripts/verify-dist.mjs`, Git/package/CI/배포/서버/브라우저는 root 소유이며 이 담당은 수정·실행하지 않았다. 다른 담당의 신규 E2E 다섯 개도 수정하지 않았다.

## 실제 읽기와 첫 실패

2026-10-19 UTC에 조직·source·docs AGENTS, Issue41 스펙, 실제 상세 템플릿, `contracts/public-catalog.mjs`/`provider-lookup.mjs`, 기존 publication 테스트, 공유 검증기와 [첫 working 검사 영수증](../operations/runs/manual-20261019-0933-working-browser.json)을 읽었다. 첫 전체 artifact 실패는 Hump Back의 충돌 TJ 번호를 정적 본문에 반드시 요구하던 이전 검사 때문이다. 이 실패를 UI 완료나 브라우저 통과로 바꾸지 않는다.

root의 새 `verifyDetailNumbers` export를 실제 읽고, 앱의 렌더러나 `hasConfirmedNumber`를 기대값 산출에 호출하지 않는 합성 HTML 반례를 작성했다. fixture의 번호·제목·가수·2001년 날짜는 합성이며 실제 공식 조회·확인일이 아니다. 테스트는 번호/URL/근거의 입력 객체도 변경하지 않는다.

최초 standalone 실행:

1. 기본 sandbox의 `node --test tests/conflict-artifact.test.mjs`는 파일 자식 테스트 자체가 실패했다. 출력은 tests1/pass0/fail1이고 구체 원인은 출력으로 확정할 수 없다. 이를 실제 네 제품 반례의 결과로 세지 않는다.
2. `node tests/conflict-artifact.test.mjs`는 실제 합성 검사를 실행하여 아래 네 우회를 재현했다.
3. 일반 network permission이 있는 `node --test tests/conflict-artifact.test.mjs`도 동일한 네 우회를 재현했다. 출력은 **tests44/pass37/fail7**이며 실패 노드7은 독립 leaf4와 그 부모 그룹3이다. 네 우회를 일곱 결함으로 부풀리지 않는다.

2026-10-19T00:52:37.049121+00:00에 읽은 최초 실패 파일 hash:

| 파일 | SHA256 |
| --- | --- |
| `tests/conflict-artifact.test.mjs` | `735040c98350543e545d3c9befd48ef26bafbc427183c35476193e6095dec2b8` |
| 당시 root 공유 `scripts/verify-dist.mjs` | `bcd7bbe69a049e06abbdb5843c0e0e1894b66ec0031c69ac330d1f31e4c57261` |

| 독립 반례 | 실제 결과 | 필요한 경계 |
| --- | --- | --- |
| conflict 보류 안내 뒤 `<span data-copy>복사</span>` | 거부 예외가 없어 실패 | 값 없는 속성도 복사 동작 표식으로 차단 |
| unknown 안내 뒤 동일한 boolean 속성 | 거부 예외가 없어 실패 | unknown도 동일한 복사 권한 경계 |
| unknown 안내 뒤 단독 `<p>00123</p>` | 거부 예외가 없어 실패 | 번호가 없는 업체의 숫자 표시를 허용하지 않음 |
| 정상 번호의 `button`을 `span`으로 변경 | 거부 예외가 없어 실패 | payload만 있는 비키보드 요소를 정상 복사 버튼으로 수용하지 않음 |

위 실제 실패를 root에게 전달했다. 공유 검증기의 수정은 root가 수행하며, 이 문서는 수정 전 결과를 보존한다.

## 수용 범위와 오탐 방지

정상 허용 사례는 TJ conflict/KY 공식 확인, 반대 업체 혼합, 양쪽 conflict, unknown/suppress, 정상 관측/공식 확인, 선행0 문자열과 과거 사유/URL/날짜의 원번호 보존이다. conflict의 현재 보류 문구와 과거 공식 대조 이력은 서로 다른 의미로 검사한다.

부정 사례는 충돌 번호 재표시, 숫자가 없는 복사 버튼, 값 있는/없는/빈 `data-copy`, 공식 확인 라벨, 이력 뒤 복사 동작, 보류 안내 누락, 정상 번호/payload 누락·선행0 손실·중복·잘못된 요소, 업체 제목/내용 바꿔치기·영역 누락/추가·다른 업체나 바깥 본문으로 조건 충족, 미확인 근거·다른 숫자 URL·다른 업체 URL·확인일 누락·archived/chart-only의 공식 확인 오표시다.

첫 실패 후 자연문 숫자를 오탐하지 않도록 unknown 설명의 `2단계`와 합성 날짜 `2001-01-01` 허용 사례를 추가했다. 단독 숫자 표시와 자연문/과거 근거를 구분해야 하며, 날짜나 설명의 숫자를 전역 금지하는 완화 없는 정규식으로 대체하지 않는다. swapped-heading fixture의 불필요한 문자열 치환도 직접 fixture로 정리했다. 최초 실패 hash와 결과는 위 표에 그대로 남긴다.

## 수정 후 검증과 인계

root가 공유 검증기를 보완한 뒤 이 담당이 실제 변경 코드를 다시 읽었다. `data-copy`의 속성 존재 자체를 검사하고, 정상 복사 요소는 실제 `type="button"`과 정확한 숫자 본문을 요구한다. unknown의 현재 표시 영역에서는 단독 숫자 요소를 거부하되 자연문·역사 근거의 숫자는 허용한다.

이 담당이 일반 network permission으로 **`node --test tests/conflict-artifact.test.mjs`를 실제 재실행하여 exit0, tests45/pass45/fail0**을 확인했다. cancelled/skipped/todo는 모두0이다. 2026-10-19T00:53:54.665287+00:00에 읽은 최종 테스트와 해당 실행의 공유 검증기 hash는 다음과 같다.

| 파일 | SHA256 |
| --- | --- |
| 동결 `tests/conflict-artifact.test.mjs` | `e71a450a5457f71e3d567fe63e590fc7b663273f58d37459b57463f534ef4e8f` |
| 당시 root 공유 `scripts/verify-dist.mjs` | `5b2c3e20ae1426f457a69e8b4b3df44febe63a26fe9e758005e29f99b44054ce` |

두 소유 파일을 동결하여 다른 두 담당의 읽기 평가를 root에게 인계한다. 공유 검증기는 현재 Astro 상세의 두 section과 역사 `verification-note` 구조를 검증하는 한정된 정적 검사이며 임의 HTML의 완전한 DOM/보안 검증기라고 주장하지 않는다. 전체 unit/type/artifact/브라우저, 같은 SHA CI/preview/실제 복구·운영/Release는 root 통합 게이트다. 이 감사 담당은 운영 배포나 AdSense 신청·승인을 실행·확정하지 않았다.
