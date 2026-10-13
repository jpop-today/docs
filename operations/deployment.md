# Cloudflare 이관·배포·복구

## 현재 확인

운영 도메인: jpop.today. 원본 저장소: kim75503/todayjpop (연결 조회 404). 사용자 Pages 스크린샷으로 프로젝트 todayjpop, todayjpop.pages.dev, 운영 브랜치 dev, 최근 성공 커밋 단축 SHA 6dbe5a6, 자동 배포 활성화를 확인했다. 전체 SHA·프로젝트 ID·빌드 명령·출력 경로·연결 저장소 설정은 아직 확인하지 못했다. 스크린샷은 해당 시점의 상태다.

## 전환 절차

1. 원본 접근 후 커밋·태그·라이선스·데이터 출처·배포 설정을 확보한다. 자격 증명은 이관하지 않는다.
2. 원본의 추적 파일과 이력을 가능한 한 유지하고 조직 소스 저장소로 이관한다. 신규 하네스 파일과 이름 충돌을 조사한다. 강제 푸시로 이미 작성된 조직 이력을 지우지 않는다.
3. 동일 빌드·데이터·URL·저장 키의 기준선을 검증한다. 현재 공개 페이지 수와 카탈로그 수는 원본에서 확인한다.
4. Cloudflare 조직 GitHub 설치에 jpoptoday 접근을 허용하고 새 저장소에 연결된 Pages 프로젝트/미리보기를 준비한다. 기존 프로젝트를 즉시 삭제하지 않는다.
5. 재설계 후보를 미리보기에서 스펙·하네스·기존 URL로 검사한다. 미리보기의 검색 색인은 HTTP X-Robots-Tag 또는 동등한 제어로 방지한다. robots 차단만으로 noindex를 대신하지 않는다.
6. 마지막 정상 배포와 도메인 연결·DNS 상태를 기록한다. 검증된 정확한 SHA를 배포하고 Cloudflare의 custom domain 절차로 연결을 전환한다.
7. 실서비스에서 주요 URL·404·검색·복사·저장·광고/CMP·sitemap을 다시 확인한다. 결과를 릴리스 기록에 쓴다.

GitHub main 병합이 자동 운영 배포를 일으키는지 먼저 확인한다. 지금 초기 소스 저장소는 완성 앱이 없으므로 운영 Pages에 연결하면 안 된다.

## 복구

동일 Pages 프로젝트의 정상 배포가 있으면 Cloudflare rollback을 사용한다. 프로젝트가 바뀌는 전환은 단순 앱 rollback으로 도메인 연결이 되돌아오지 않으므로 이전 프로젝트·custom domain·DNS 연결 복구 절차를 미리 기록한다. 설정값은 실제 관찰 결과를 사용한다. 전환 전 복구에 필요한 변경 범위를 확인하고 새 도메인을 만들거나 이름 서버를 바꿀 필요가 있는지 추정하지 않는다.

## 배포 기록 형식

시각(Asia/Seoul), 소스 SHA, 데이터 스냅샷, 프로젝트/배포 ID, 빌드 명령·출력, 미리보기, 검증 결과, 이전 정상 버전, 도메인 전환, 복구 결과.

[Git integration](https://developers.cloudflare.com/pages/configuration/git-integration/) · [연결 문제](https://developers.cloudflare.com/pages/configuration/git-integration/troubleshooting/) · [Custom domains](https://developers.cloudflare.com/pages/configuration/custom-domains/) · [Rollbacks](https://developers.cloudflare.com/pages/configuration/rollbacks/)
