# Cloudflare 운영 배포 기준선

2026-10-13 KST · Issue [jpoptoday#3](https://github.com/jpop-today/jpoptoday/issues/3) · SRC-001, RELEASE-001.

사용자가 환경 설정에 두 항목을 입력한 뒤 같은 머신의 변수가 존재하고 실제 Pages API GET이 성공했다. secret readiness telemetry는 unknown이지만 특정 프로젝트 읽기 요청은 성공한 관찰로 기록한다. 토큰·Account ID 값은 출력/저장하지 않았다. 검증 중 운영 프로젝트 변경·배포·rollback은 실행하지 않았다.

| 항목 | 실제 API 관찰 |
| --- | --- |
| 프로젝트 | todayjpop |
| 프로젝트 ID | 196a371c-8c1c-44da-8065-34478ab07cfb |
| 도메인 | jpop.today, active |
| GitHub 연결 | kim75503/todayjpop |
| 운영 브랜치 | dev |
| build command / output / root | npm run build / dist / 저장소 루트 |
| 현재 운영 deployment ID | 5072e2ec-6858-4c44-b34f-8c27c3a016e8 |
| 현재 운영 소스 SHA | 6dbe5a65d1bd729c09393f746f715f4363089a43 |
| 생성 시각(KST) | 2026-10-12 19:39:17 |
| deploy stage | success |
| 직전 성공 운영 deployment | 394fc369-7263-403a-a72a-dfebc9889760 |
| 직전 소스 SHA | 03b81b7fbe2a70b80e41e71cbfa19de1921bb3a9 |

최근 성공 운영 배포10개가 조회됐다. 직전 배포는 복구 **후보**이며 rollback API를 실행했거나 복구 capability를 실제 시험한 것은 아니다. 전환 직전에 현재 production/후보 상태를 다시 확인한다. 새 조직 repo가 이미 연결돼 있다고 설명하지 않는다.

## 재현

Node 하네스의 외부 npm 의존성은 없으며 이 읽기 전용 운영 helper에는 Python3 기본 라이브러리를 사용한다. 인증 값은 현재 환경의 CLOUDFLARE_API_TOKEN과 CLOUDFLARE_ACCOUNT_ID binding을 재사용한다.

```sh
cd /workspace/jpoptoday
python3 scripts/inspect-cloudflare.py --project todayjpop
```

스크립트는 상속된 proxy/CA 신뢰를 사용해 특정 프로젝트·deployment·domain을 GET한다. source/build/공개 deployment metadata만 선택해 출력하며 전체 프로젝트 response의 환경 변수·secret 값은 출력하지 않는다. 실패는 HTTP 상태·오류 코드로 보고한다. 읽기 성공을 생성·배포·DNS 편집 권한 검증으로 대신하지 않는다.

## 이관 영향

운영 SHA는 기존 baseline의 dev SHA와 동일하므로 현재 두-SHA legacy ID 기준선의3,317개를 유지한다. 실제 운영 ID3,313개와 이전 main 전용4개를 구분하여 staging에서 보존한다. 번호·행별 근거의 진위나 자료 권리는 API 배포 성공으로 검증되지 않는다.

새 앱·공개 artifact·ID/URL/storage 회귀와 preview가 완료된 뒤 배포 연결/도메인 전환을 검토한다. 해당 단계 전에는 현재 프로젝트·Git 연결·기존 예약 수집을 변경하지 않는다.
