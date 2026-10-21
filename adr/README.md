# 아키텍처 결정

| ADR | 상태 | 결정 |
| --- | --- | --- |
| [001](001-static-architecture.md) | accepted, 앱 구현 예정 | 정적 Cloudflare 배포와 Astro/TypeScript 경계 |
| [002](002-stable-identity.md) | accepted, 이관 구현 예정 | 편집값과 독립된 ID 및 legacy alias |
| [003](003-evidence-and-publication.md) | accepted, 증거 보강 필요 | 출처·권리 기반 발행과 수집 실패 보존 |
| [004](004-observed-preview-read-model.md) | accepted, 앱 미리보기 구현 | 관측 read model·검증 카탈로그 분리와 noindex 미리보기 |
| [005](005-daily-agent-management.md) | accepted, 실행 증거 별도 | 매일09시 새3명·두 동료 평가/교체·Discord 보고·의미 있는 릴리스·원본 색상 |
| [006](006-reviewed-corrections-and-production.md) | accepted, 실제 배포 증거 별도 | 검토한 번호 정정과 운영 발행 경계 |
| [007](007-deployment-response-verification.md) | accepted, 실제 HTTP 증거 별도 | 고정 artifact와 운영 응답의 비교·복구 검증 |
| [008](008-favorites-file-backup.md) | accepted, Issue51 구현·검증 중 | stable ID·미해결 항목·기존 metadata의 수동 JSON 보관과 확인 후 합치기 |

새 결정은 원인, 대안, 선택, 결과, 검증 방법을 기록한다. accepted는 구현·배포 완료를 의미하지 않는다.
