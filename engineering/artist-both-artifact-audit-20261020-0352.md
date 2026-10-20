# 두 업체 탐색 artifact 독립 감사 · Issue44

실행 `manual-20261020-0352`, 담당 `/root/run44_replacement`. 한국 시간 2026-10-20의 Issue44 후속이며 [제품 스펙](../product/spec.md#두-업체-공식-확인곡-탐색--issue44-2026-10-20), [실행](../operations/runs/manual-20261020-0352.json), [실제 여섯 평가](../operations/runs/manual-20261020-0352-reviews.json)에 연결한다.

소스 base `49d6806bb00e813bb3dcda15a22f2cf8d2d21da8`, 문서 base `4ddfa9d20b2d8aee5c538d25388c499a83c8675f`, 브랜치 `codex/issue44-provider-both`의 커밋 전 공유 작업을 읽었다. 최초 세 담당의 실제 평가는 40/40/40 동률이다. 사전 순서 evidence→app→quality로 evidence 담당이 active roster에서 논리적으로 제외됐고 실제 신규 교체 담당을 배정받았다. 품질 열세·물리 삭제·변경량에 따른 순위를 주장하지 않는다.

## 범위와 독립성

소유 파일은 소스 `tests/both-provider-artifact.test.mjs`와 이 문서 두 개다. root 소유 `scripts/verify-dist.mjs`의 `verifyBothProviderDiscovery(html, songs)`만 import하여 검사한다. 앱의 카운트·그룹·판정·URL helper를 기대값 계산에 사용하지 않는다. 합성 HTML과 provider fixture의 명시적 기대 그룹/교집합을 별도로 쓴다. SHA-256 원문 가수 anchor 규약만 Node crypto로 직접 계산한다.

합성 `2001-01-01`·제목·가수·번호는 테스트 전용이며 실제 공식 조회·권리·심사 근거가 아니다. 같은 가수의 `001`/`1` ID와 `00123`/`123` 문자열을 구분한다. `共演 A & B`와 `共演 A&B`는 별개 원문 가수 표기로 유지한다. 명시 기대는 두 업체 교집합 3 ID, 업체 합집합 8 ID, 업체별 건수 합 11, 가수 그룹 7개다. 단일 업체만 확인된 가수의 교집합 0을 표시하고, 충돌·관측·URL 번호 불일치·archived·chart-only·unknown을 서로 구분한다. 같은 가수의 TJ 전용 ID와 금영 전용 ID를 합쳐 두 업체 확인곡 1개로 만드는 오류도 거부한다.

양성은 혼합 자료·0곡·순서가 다른 그룹/AND query·선행 0·원자료 불변을 확인한다. 음성은 합집합/업체 수 합산·data만 맞고 가시 숫자가 틀린 경우·중복/누락 카운트·누락/다른 가수 그룹·깨진 anchor·실제 검색 외 URL·단일/중복 업체/추가 조건·빈/중복/비-anchor 검색 동작을 검사한다. 정확한 가수명과 HTML `hidden`에 의해 접근할 수 없는 요구 요소도 별도로 검사한다.

## 최초 실제 실행과 발견

2026-10-19T19:09:52Z의 사후 읽기에서 아래 초기 파일 hash를 확인했다. 명령은 소스 저장소에서 `node --test tests/both-provider-artifact.test.mjs`다. Git·공용 파일·package·CI·배포·빌드·브라우저 서버·전체 suite는 이 담당이 조작하지 않았다.

| 증거 | SHA-256 / 결과 |
| --- | --- |
| 초기 테스트 파일 | `6794eb13a45658e1b2df93f7db330a3597e529314e376bf87a32702a3384fc81` |
| 초기 root verifier | `2361fb24225b858a2df8af31a67ada28ff400e93482f65416194f641b43fea9e` |
| 최초 제한 sandbox 로그 `/tmp/jpop44-replacement-initial-test.log` | `1b52022672ebd3c090b14e478cc4c0b293cd99088a98d8322a448e472c32e650` · 파일 wrapper 1 실패, leaf 실행/통과를 확인할 수 없음 |
| 일반 네트워크 실행 권한의 실제 로그 `/tmp/jpop44-replacement-initial-permitted-test.log` | `ae7ffb66c17e41ca73c0eacf4f5570d87ff604b95dc33e4860fe3f9f2b758316` · Node nodes 47, 37 통과/10 실패 |

제한 sandbox의 실패 원인을 이번 담당은 확정하지 않는다. 이어서 허용된 일반 네트워크 실행 권한으로 동일 명령·동일 테스트 파일을 실행했다. 이 실행에서 8개 leaf가 `Missing expected exception`으로 실제 실패했다. 실패한 leaf를 포함한 부모 2개도 실패하므로 총 실패 node 10을 독립 결함 10개라고 부르지 않는다.

| 실제 통과해 버린 잘못된 HTML | 재현 방식 | 문제 |
| --- | --- | --- |
| 다른 index 가수명 | 올바른 원문 hash/수는 두고 표시 span만 `別の歌手`로 교체 | count와 anchor가 맞아도 방문자가 다른 가수명으로 안내됨 |
| 다른 group 가수명 | 올바른 section hash/수는 두고 h2만 `別の歌手`로 교체 | 해당 교집합을 다른 가수의 결과로 표시 |
| 다른 group 원문 속성 | `data-artist-name`만 `別の歌手`로 교체 | 가수 identity metadata와 원자료 불일치 |
| 숨겨진 global/index/group 수 | 각 실제 count element에 boolean `hidden` 추가 | 올바른 data/문자열만으로 사용자에게 없는 수를 허용 |
| 숨겨진 검색 anchor/label | anchor에 `hidden` 또는 유일 label을 hidden span에 넣음 | 동작 또는 그 이름에 접근할 수 없어 usable link 요구에 미달 |

root에 실제 실패와 로그/hash를 즉시 인계했다. 테스트를 약화하거나 가짜 결함을 요구하지 않았으며 공유 verifier는 root가 수정한다. 초기 테스트·로그를 덮어쓰지 않는다.

## 최종 결과

root는 verifier에서 HTML의 정적 비노출 하위 트리를 제외하고, 원문 가수 map을 보존하여 index 가시 이름·group h2·`data-artist-name`을 대조하도록 수정했다. 수정 파일을 직접 읽었으며 이 담당의 테스트 파일은 초기 hash 그대로다. 2026-10-19T19:11:55Z 사후 확인 기준, 일반 네트워크 실행 권한으로 동일 명령을 독립 재실행한 결과 Node nodes **47 통과/0 실패**다. 최초 실패 사례 여덟 개를 삭제·완화하지 않았다.

| 최종 증거 | SHA-256 / 결과 |
| --- | --- |
| 최종 owned 테스트 파일 | `6794eb13a45658e1b2df93f7db330a3597e529314e376bf87a32702a3384fc81` · 초기와 동일 |
| 수정 후 root verifier | `b31b13f31ebdb40784bbb1171eec62062b2c6575ba1b794fc1afa7eb1c426d74` |
| 이 담당의 독립 재실행 로그 `/tmp/jpop44-replacement-final-test.log` | `9a4e3bc94093e92d062dc9fa53338a8f5517f6f3ef28b94b97d3f313a55a7929` · 47/47 |
| root 별도 테스트 로그 `/tmp/jpop44-replacement-root-fixed-test.log` | `55b76001ff5a84afbcb276bb0bb28e7d0602963bec39b252f682201c1b9f4fa5` · 이 담당의 독립 실행과 별개 |
| 직접 읽은 root artifact 로그 `/tmp/jpop44-artifact-hardened.log` | `63952b7db049e802408a6be03e9cc4758dc1ab959c566b2b24c1bb2fb441fce4` · dirty local preview 3,347 파일/3,336 HTML/0 indexable |

artifact 로그의 source는 base `49d6806…`이며 dirty build다. 이를 clean 최종 SHA·CI·실제 미리보기/운영 배포 통과로 바꾸어 부르지 않는다. CSS 계산 결과·일반 HTML parser와의 완전한 등가성·브라우저 focus·모바일·실제 provider 조회·CI·미리보기/운영 SHA·복구는 이 합성 artifact 감사만으로 증명하지 않는다. 실제 브라우저/같은 SHA 게이트와 최종 통합은 root의 별도 검증 범위다. 이 담당은 테스트와 이 문서를 읽기 전용 동료 평가에 인계하고 동결한다.
