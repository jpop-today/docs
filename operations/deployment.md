# Cloudflare 배포·복구

RELEASE-001. 운영 todayjpop의 npm run build/dist/source dev 설정을 Pages API로 확인했다([기준선](cloudflare-baseline.md)). 개인 원본 README의 Node24 요구와 실제 runtime 설정 확인은 구분한다.

## 연결 조사

프로젝트명·계정·현재 연결 repo/production branch·build command/output·custom domain·현재 운영 deployment ID/SHA·직전 정상 deployment·예약 수집 branch를 실제 Cloudflare/GitHub에서 확인해 기록한다. 비밀 값은 기록하지 않는다. 신규 org Git 연결이 자동 운영 배포를 유발하는지 확인하고 먼저 preview branch를 사용한다.

수정·미리보기·배포는 사용자가 위임했다. 정상 게이트를 통과한 릴리스에 반복 확인을 요구하지 않는다. 계정 본인 정보·약관 동의·사용자 데이터 삭제 같은 입력은 사용자에게 맡긴다.

## 게이트

1. 관련 스펙·ADR·변경 데이터·권리/호환성 검토, 소스 SHA 고정.
2. lockfile 설치, 테스트·typecheck·고정 스냅샷 빌드. 현재 org에는 앱 빌드가 없으므로 이를 완료로 표시하지 않는다.
3. 실제 dist에서 원본 TSV/JSON/가사·scripts·tests·환경 파일 비공개 검사. 설정만 믿지 않는다.
4. CI와 같은 SHA/artifact로 preview 배포. 검색·번호 복사·즐겨찾기·기존 URL·404·본문·sitemap·광고/동의를 브라우저·HTTP로 확인.
5. 이전 정상 deployment로 복구하는 방법과 데이터 snapshot을 확보.
6. 위임 범위에서 production 배포, 실제 jpop.today에서 핵심 스모크·artifact SHA 연결 확인.
7. 릴리스 기록에 source SHA, catalog hash, content version, CI run, preview/production deployment ID, 검사 결과, rollback target 기록.

/songs 정적 상세뿐 아니라 기존 /song?id=, /filter?q=유이카, /chart, /coverage, /favorites, /guides/*, .html 리다이렉트와 /#favorites-view를 확인한다. /data/jpop-playlist.raw.json·/scripts/*·존재하지 않는 경로는 실제 404여야 한다. 운영 HTTPS origin을 유지해야 기존 localStorage를 읽을 수 있다.

## 실패와 복구

수집 오류는 신규 데이터 승격과 배포를 중단하고 마지막 정상 snapshot을 유지한다. UI/URL 회귀는 이전 정상 artifact로 복구한다. 데이터와 코드 버전을 함께 고정하고 smoke를 다시 실행한다. 권리 문제 자료를 내릴 때는 새 artifact에서 제외 후 운영 반영·캐시 상태를 확인한다. 이전 이력을 지우는 강제 push나 운영 프로젝트 삭제는 하지 않는다.

## 현재 제한

초기 운영 GET·GitHub API는 CONNECT403이었으나 후속 HTTP·PR·CI 확인은 성공했다. GitHub 배포 목록 API는 integration scope403이다. 사용자가 CF binding을 입력한 뒤 특정 Pages 프로젝트/domain/deployment 읽기는 성공했다. 아직 새 앱 artifact/preview/rollback/운영 전환은 실행하지 않았다. 인증·프로젝트 연결을 확인할 때까지 배포 완료를 주장하지 않는다. 네트워크 설정 반영만으로 인증·push·배포 권한이 생겼다고 가정하지 않는다.
