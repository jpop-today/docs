# Issue47 r2 · 원본 ads.txt 바이트 복원 근거

run `manual-20261020-1325`의 운영 후속이다. source 기준 `7ced2f6b2d072db75c82d48d2196577bb04b3342`, root branch `codex/issue47-ads-txt`를 이어간다. 자료 담당은 이 신규 문서만 작성했으며 소스·원장·가이드·public 파일·하네스·Git·CI·배포는 수정하지 않았다. [Issue47 후속 스펙](../product/spec.md#실제-운영-후속--원본-adstxt-복원)을 따른다.

## 전후 관측과 보류

- root의 [재적용 후 실제 사이트 영수증](../operations/runs/manual-20261020-1325-postrestore-site.json)은 2026-10-20T07:26:45.207Z~07:27:01.828Z의 일반 10URL 상태200을 기록한다. 전후 build-info는 production·위 source SHA·dirty:false 및 같은 카탈로그/ledger/chart hash다. 이 일반 URL 성공과 별도로 같은 영수증의 `/ads.txt`는404·2439bytes였다.
- [별도4요청 재확인](../operations/runs/manual-20261020-1325-ads-txt-recheck.json)의 실제 시각은 `2026-10-20T07:28:38.982Z`다. 운영도메인, cache query를 붙인 운영URL, 기본 pages.dev, 이전배포 URL 모두404·HTML·2439bytes이며 응답 SHA256은 `63ec26390803a46d63878007da45ac26e516c934d35ba244397107433d256f2a`다. 이 본문이나 게시자 값은 보존·출력하지 않았다.
- 스펙에 보존된 이전200·59bytes 관측과 이번404는 상태 차이다. 현재404의 발생 시점·변경 원인·캐시/라우팅 원인·Google봇 접근이나 계정 상태는 확정하지 않는다. 이전 운영 전환/CI/복구 성공을 이 파일의 현재 제공 성공으로 확대하지 않는다. root는 미발행 v0.4.9를 보류하고 r2 검증을 진행한다.

## 공개 원본과 현재 소스의 연결

[원본 조회 영수증](../operations/runs/manual-20261020-1325-original-ads-provenance.json)은 root가 공개 원본 `kim75503/todayjpop`의 기존 파일을 실제 읽은 기록이다. 이 담당도 연결 GitHub의 GET으로 immutable commit과 tree의 관계를 따로 확인했다. 응답에서 commit/tree/blob·경로·크기만 사용했고 파일 본문은 새로 가져오지 않았다.

| 연결 | 실제 근거 |
| --- | --- |
| 원본 immutable commit | `744b90ba1c99b60134dcc57e347b408f61557cd0` |
| commit의 tree | `9d7cbe9c0b65203fb73431604a3f45d784925ecb` |
| tree의 원본 경로 / blob | `ads.txt` / `aeee54215b01df4256bd69a3eaec9c95226d0850` |
| root의 복원 경로 | 소스 `public/ads.txt` |
| 현재 로컬 파일 | 59bytes·1행·판매자 형식1행 |
| 현재 SHA256 | `485311a4f1407193caa0a8a7314c316e2f37aaae73986b7c7f935033fb44db80` |

- 독립 commit 읽기: [GitHub GET](https://api.github.com/repos/kim75503/todayjpop/git/commits/744b90ba1c99b60134dcc57e347b408f61557cd0), 실제 확인 `2026-10-20T07:36:18.707Z`.
- 독립 tree 읽기: [GitHub GET](https://api.github.com/repos/kim75503/todayjpop/git/trees/9d7cbe9c0b65203fb73431604a3f45d784925ecb), 실제 확인 `2026-10-20T07:36:27.733Z`; 원본 경로의 blob·59bytes 일치.
- 현재 소스 파일을 읽기 전용으로 검사해 길이·행수·SHA256과 Git blob SHA1(`blob 59` 헤더 포함)을 계산했다. Git blob SHA1도 위 원본 blob과 같으므로 원본 공개 바이트의 정확 복원이다. 게시자 행 값·파일 전문은 이 문서·로그·채팅에 쓰지 않았다.

공개 원본의 존재와 동일 바이트 복원은 운영자 계정과의 일치·소유권·광고 활성화·약관/법적동의·Google 심사결과의 증거가 아니다. 이 항목들은 operator unknown이며 새 게시자 값이나 동의를 만들지 않았다. 새 외부 정책 조사/확인일도 생성하지 않았다.

## 제공 계약과 완료 경계

production은 확정된 artifact에서 원본과 같은59bytes를 `/ads.txt`의200·text/plain으로 제공해야 한다. preview에서는 해당 파일을 제거하고404·전체noindex·광고없음을 유지한다. 파일의 존재가 광고 삽입·광고 클릭 테스트 허가를 뜻하지 않는다.

스펙은 production 누락/변조와 preview 유출을 artifact에서 거부하고 실제HTTP의 상태·형식·전체바이트를 비교하도록 요구한다. 해당 파일이 원래 없던 동결 release는 동일한 release artifact의 조건으로 복구하며 새 파일 요구로 과거 복구를 막지 않는다. 기존27검사·이메일/metadata/바이트 보호도 유지한다. 이 문서는 계약과 현재 소스 복원 근거이며 r2의 새 동일SHA CI·preview·production·실제복구→재적용·최종사이트200을 수행했다는 완료 주장이 아니다. 최신 root 영수증으로 각각 확인한 뒤 릴리스한다.

원래7건 번호 근거, ledger99/가이드, 원6+후속2 평가와 최초 artifact 실패는 덮어쓰지 않았다. 이 담당은 관련6파일(원장·가이드·조회영수증·자신의원peer·후속peer·최초실패로그)의 기존 SHA256이 같은 것도 읽기 검사했다. 운영자에게 계정 일치/법적동의/실제 심사결과가 필요한 단계는 계속 별도다.

## 읽은 실행 근거의 SHA256

| 영속 JSON | SHA256 |
| --- | --- |
| `manual-20261020-1325-postrestore-site.json` | `9e20e150513fdaadc4ea2c35a69a5e55b5ccc716fc7f4fda3f7534348c170b5b` |
| `manual-20261020-1325-ads-txt-recheck.json` | `446ac0b297a07cd6094a4afc8046daeae6e6b0ecc5c90e35c4a73993911b3931` |
| `manual-20261020-1325-original-ads-provenance.json` | `ebdbc371b62752a8a72f43cf2e9069ac44fa548565c538f1ccdc074c1e00ea4d` |

위 시각은 각각 실제 HTTP/원본 API/로컬 읽기 근거의 시각이며 Google 심사일·새 정책 확인일이 아니다. 문서 작성 시각: `2026-10-20T07:38:12.014194Z`.
