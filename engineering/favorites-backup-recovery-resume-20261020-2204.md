# Issue51 · 교체 담당의 즐겨찾기 복구 실패 화면 감사

2026-10-21 KST · `/root/r51_recovery_audit` · run `resume-20261020-2204`.

[Issue51](https://github.com/jpop-today/jpoptoday/issues/51), FAVORITES-001/DATA-001/HARNESS-001/PRIVACY-001와 [ADR-008](../adr/008-favorites-file-backup.md)에 연결한다. 소스 base는 `e52f0566b13bb7879e4c44eed3dd81c326956f70`, 문서 base는 `60e40985d7fdd2d3f0c7ed1f2c4246a275d1260a`다. 작업 시점은 같은 Issue의 미커밋 통합 후보이며 새 확정 SHA의 CI·운영 결과가 아니다.

원래 세 담당의 실제 여섯 평가에서 자료40/앱40/품질39였고, 앱 동료는 `restored=false` 계약 단위 검사와 복구 실패 안내의 실제 브라우저 검사 사이 누락을 기록했다. 관리자가 품질 담당을 active roster에서 제외하고 interrupt(직전 상태 completed)한 뒤 이 교체 담당 한 명을 영입했다. 평가·코드·실패 이력의 물리 삭제를 수행하거나 주장하지 않는다. [관리자 평가](../operations/runs/resume-20261020-2204-reviews.json)와 [교체 기록](../operations/runs/resume-20261020-2204-replacement.json)을 따른다.

쓰기 소유는 신규 `jpoptoday/e2e/favorites-backup-recovery.spec.ts`와 이 문서뿐이다. 기존 앱/계약/검사 파일을 수정하지 않았고 Git/package/CI/빌드/서버/배포 조작은 하지 않았다. root가 이미 제공하는 `http://127.0.0.1:4321`의 최종 후보 artifact를 사용했다. 각 AGENTS, ADR-002/008, Issue51 spec, 기존 여섯 평가, 원문 복구·단일 후보 저장 계약과 앱 안내 분기를 실제 읽었다. Browser 관련 skill/runtime는 부모가 확인한 경로와 별도로 사용 가능 도구를 조회했지만 연결 Browser `js` 도구가 없었다. 이 작업은 기존 Playwright 검증을 명시적으로 허용해 시스템 Chromium 경로를 사용했다. 연결된 로컬 브라우저가 있다고 주장하지 않는다.

## 실제 검증

흐름은 `/favorites/` → 로컬 합성 JSON 선택 → 확인 화면(쓰기0) → 명시 합치기 → 원문 복구 실패 안내 → 임시 목록 실제 파일 다운로드다. 원본/입력/다른 작성자 bytes와 기대 배열은 독립 literal로 만들었으며 formatter/merge 출력으로 기대값을 생성하지 않았다. 데이터는 합성 메모·unknown ID와 원본 staging에서 읽은 실제 stable ID이며 실제 사용자 저장 자료를 읽지 않았다.

| 주입 조건 | 실제 저장소/안내/파일 대조 |
| --- | --- |
| 후보 쓰기 후 예외, 원문 rollback setItem도 거절 | 두 번의 set 시도·남은 후보 bytes를 확인하고 원본 복구 미확인 안내 및 전체 유효 세션 파일을 확인 |
| 후보 직후 다른 작성자가 원문을 교체 | rollback이 후속 작성자의 bytes를 덮지 않음·set 시도1회·세션에는 기존 metadata/unknown과 새 항목이 남고 파일 구출 가능 |
| 원래 저장 키 없음, 후보 쓰기 후 rollback removeItem 거절 | set1/remove1 시도·남은 후보와 세션 파일 내용·복구 미확인 안내 확인 |
| rollback 쓰기 뒤 read-back 전에 다른 작성자가 변경 | set2 시도 뒤 다른 작성자의 정확한 bytes를 보존하며 복구 성공으로 표시하지 않음 |
| rollback 실패와 sessionStorage set 거절이 함께 발생 | 영속/세션 저장 성공을 주장하지 않고 현재 화면의 합친 목록을 파일로 구출 가능 |
| 정상 rollback 대조 | 공백을 포함한 기존 원문 bytes가 정확히 복구되고 보존 안내가 사실과 일치하며 세션 파일에는 새 합친 항목도 포함 |

2026-10-20T18:12Z 실제 실행에서 **6/6 통과(8.7초)**, 관련 console/pageerror0·GET/HEAD 외 파일 업로드 요청0을 확인했다. `#storage-note`, `#favorites-backup-status`의 미복구 분기는 모두 원본 복구를 확인하지 못했다고 알리고 파일 보관을 제안하며 `보존했습니다`·새 ID 저장 성공 문구를 쓰지 않았다. 다운로드 JSON을 실제 디스크에서 읽어 전체 배열·기존 객체 metadata·중복·미해결 ID와 별도로 작성한 기대값을 비교했다. 다운로드는 저장소 bytes나 쓰기 횟수를 바꾸지 않았다. 즐겨찾기 경로의 noindex도 확인했다.

명령은 `JPOP_TEST_BASE_URL=http://127.0.0.1:4321 PLAYWRIGHT_CHROMIUM_EXECUTABLE_PATH=/usr/bin/chromium JPOP_RECOVERY_SCREENSHOT_DIR=/tmp/jpop51-recovery-screenshots npm run test:e2e -- e2e/favorites-backup-recovery.spec.ts --output=/tmp/jpop51-replacement-recovery-e2e`다. Node v24.19.0, Chromium 154.0.8037.57이다. 원본 로그 `/tmp/jpop51-replacement-recovery-first.log`의 SHA256은 `8a05ebf4ee55b302ab1f551f08c3d4f68174a49d526e5d99bb5c5ec51fcd68f3`, 이 시점 검사 파일 SHA256은 `93bdefe5dff1940b781e10b629180c43630d8e36105384fcadea8b9f7e9448ed`다. 두 저장소 `git diff --check`를 읽기 전용으로 실행해 통과했다.

`/tmp/jpop51-recovery-screenshots/favorites-backup-concurrent-recovery-warning.png`를 도구로 직접 열어 두 복구 미확인 안내·구출 버튼·현재 목록을 확인했다(SHA256 `c9ade3e65c8748f50caf7a5002a72e635d4a953a679b3d142081df4e25396f7b`). 이 이미지는 경고로 스크롤한 1440px 화면이며 첫 viewport나 모바일 전체 품질의 근거로 확대하지 않는다.

## 실제 관찰과 남은 범위

첫 캡처에서 기존 `#unresolved-favorites > p`의 고정 설명은 “원래 저장값을 보존했습니다”라고 표시됐다. unknown 항목은 실제 세션에 보존되어 있으나, 원본 저장소 복구 미확인 경고와 함께 읽으면 원문 bytes의 보존 성공으로 혼동할 여지가 있었다. 두 지정 복구 경고의 최초 여섯 검사는 통과했지만 이 별도 설명을 검사하지 않은 좁은 통과였으며, 설명의 모호함은 실제 화면을 직접 보고 root에게 인계했다. 첫 로그·캡처를 실패한 테스트처럼 바꾸거나 덮어쓰지 않았다.

root가 원본 페이지를 [감사 snapshot](audit-evidence/favorites-backup-before-root-recovery.astro)에 보존한 뒤 설명 한 구절만 “현재 곡 자료에서 찾지 못한 항목입니다. 원래 항목을 아래 목록에 유지합니다.”로 보완했다. 이 담당은 소유 범위 밖의 앱 코드를 수정하지 않았다. snapshot SHA256 `49a9cead09c080819d7eabb6f00ce5447c0262c629c39833746371e1964931d5`는 최초 동료 평가 대상과 같고, 보완된 페이지 SHA256은 `60c7f31be3d6ee90080d3ac47d08f91b432f47016291f30e518b209338f53862`다. 원래 항목을 현재 목록에 유지하는 것과 영속 bytes의 복구를 확인한 것을 구분한다.

## 강화 후 실제 확인

root가 새 설명을 포함해 다시 build/verify한 artifact의 브라우저 gate를 연 뒤 검사 파일의 기존 여섯 경우 모두에 새 설명을 정확히 대조하는 assertion을 추가했다. 미복구 다섯 경우는 그 설명에 저장값 보존 성공 문구가 없는 것도 검사하며, 정상 rollback 대조도 같은 목록 설명을 사용한다. 기존 실제 저장 bytes·쓰기 횟수·경고·다운로드 JSON 대조를 유지하고 페이지 제목/h1·오류 overlay 부재와 정상 대조의 console/업로드 계측도 확인했다.

2026-10-20T18:15:17Z 확인 시점에 강화 실행은 **6/6 통과(9.2초)**였다. 새 출력은 `/tmp/jpop51-replacement-recovery-strengthened-e2e`, 원본 로그는 `/tmp/jpop51-replacement-recovery-strengthened.log`(SHA256 `2e30a4484871d38c153bd87364d81dc86edc72de97da26f5ce4cf8e3f028290f`)이며 첫 여섯 통과의 로그/출력을 보존했다. 최종 검사 파일 SHA256은 `e7202eaf294de93735d65c5c9494261011b8af5208e54a6b4bcaffae2ab71f93`다. 명령은 앞선 실행과 같되 `JPOP_RECOVERY_SCREENSHOT_DIR=/tmp/jpop51-recovery-strengthened-screenshots`, `--output=/tmp/jpop51-replacement-recovery-strengthened-e2e`를 사용했다.

새 `/tmp/jpop51-recovery-strengthened-screenshots/favorites-backup-concurrent-recovery-warning.png`도 도구로 직접 열어 두 복구 미확인 안내와 새 목록 설명의 일관성을 확인했다(SHA256 `861bc6ce83ef7e01332cb1dd19765c03b68b74beceb4b9bd07e685ea1b660bb3`). 전후에 `build-info.json`의 sourceSha는 base `e52f0566b13bb7879e4c44eed3dd81c326956f70`이고 `dirty:true`인 로컬 후보였다. 이를 clean commit의 원격 검증 또는 실제 운영 SHA로 바꾸어 쓰지 않는다. 원본 snapshot/최초 평가/최초 여섯 통과/시각 발견/root 보완/강화 여섯 통과를 구분해 보존한다.

이 감사는 로컬 Chromium의 합성 장애 여섯 경우다. 실제 OS 저장 공간 고장·브라우저 종료/전원 손실·모든 스케줄링의 동시 작성자·다른 브라우저 엔진·OS 파일 대화상자 권한·원격 CI/preview/운영 복구·Google 계정 또는 심사 결과를 검증하지 않았다. 공식 원장·raw/chart·3317ID·브랜드·URL/저장 키·출처/조회일을 바꾸거나 새 권리·승인 근거를 만들지 않았다. 최종 통합·배포는 root의 별도 실제 증거를 따른다.
