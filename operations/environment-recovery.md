# 개발 환경과 복구

클라우드 작업은 이미 격리되어 있으므로 /workspace/docs, /workspace/.github, /workspace/jpoptoday의 기존 체크아웃을 사용한다. 명시 요청 없이 worktree를 만들지 않는다. 최초 상태와 작업자 변경을 확인하고 reset/force push로 복구하지 않는다.

## 현재 조직 하네스

Node.js >=22, 이번 실행 Node 24.19.0. 외부 패키지·DB·추가 비밀 값 없이 아래 명령을 실행한다.

```sh
cd /workspace/jpoptoday
node --test
node scripts/validate-catalog.mjs tests/fixtures/catalog.json
```

안전 승격 사용법과 실제 입력 검증은 [하네스](../engineering/harness.md)를 따른다. 합성 fixture를 운영 데이터로 사용하지 않는다. docs와 .github에는 앱 서버·빌드가 없다.

## 원본 조사 재현

원본 기본 브랜치는 dev이고 main과 다른 이력이다. 운영 branch를 기본 branch와 동일하다고 추정하지 않는다. Git HTTPS 플랫폼 인증으로 읽기 접근을 시험하고 필요한 경우 /tmp의 별도 조사 checkout을 만든다. 빌드는 원본 조사 checkout을 변경하지 않는 별도 복사본에서 한다.

```sh
npm ci
npm run build
npm run check:js
npm test
```

위 명령은 개인 원본 package/lockfile이 있는 복사본에서 검증했다. 조직 앱 명령으로 설명하지 않는다. 브라우저 baseline은 기존 Playwright 설정을 사용하며 이번 클라우드에서는 별도 외부 config로 /usr/bin/chromium을 사용했다. WebKit은 미실행이다. 운영 Chromium 대표 흐름은 기존 환경 CA가 등록된 표준 NSS 저장소 접근을 허용한 실행에서2개 통과했다. 인증서 오류를 ignoreHTTPSErrors나 TLS 해제로 우회하지 않는다. Node24·시스템 Chromium·원본 SHA·실행 config를 함께 기록한다.

## 접근

GitHub Git 읽기 성공과 API/쓰기 권한을 구분한다. 토큰·환경 값은 출력하지 않는다. 네트워크 추가 도메인은 환경 설정의 기존 allowlist를 보존하며 추가한다. draft 저장은 runtime 적용·publish가 아니다. Cloudflare/Google 계정 연결과 본인정보는 실제 readiness를 확인한다. 서버 프로세스는 새 작업에 유지된다고 가정하지 않는다.
