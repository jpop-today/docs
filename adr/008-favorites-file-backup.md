# ADR-008 · 즐겨찾기 수동 파일 보관과 합치기

상태: accepted · 2026-10-20 · [Issue51](https://github.com/jpop-today/jpoptoday/issues/51). FAVORITES-001/DATA-001/PRIVACY-001, ADR-002/003을 확장한다.

현재 번호 목록 복사는 곡 선택의 복원 파일이 아니다. 브라우저 데이터 삭제·기기 이동 전에 사용자가 직접 보관할 수 있도록 별도 JSON 다운로드/가져오기를 제공한다. 서버 계정·자동 동기화나 provider 확인 자료를 파일에서 신뢰하는 기능은 추가하지 않는다.

파일 계약은 `{ "format": "jpop.today/favorites", "version": 1, "values": [...] }`다. UTF-8 전체 256KiB 이하만 허용한다. values는 기존 string ID 또는 유효한 string id를 가진 JSON 객체다. 번호·제목·가수로 병합하거나 ID를 trim/숫자화하지 않는다. 알려지지 않은 ID와 기존 객체 metadata도 보관한다. 파일에 별도 확인일·권리·추천을 생성하지 않는다. 내보내기는 현재 필터와 무관하게 전체 저장/유효 세션 목록이며 손상된 항목을 조용히 누락하지 않고 실패를 알린다.

파일 선택·검증과 개수 표시까지는 읽기 전용이다. 별도 명시적 합치기에서만 기존 전체 목록의 순서·객체·중복을 그대로 두고 새 ID를 추가한다. 새 항목 간 중복은 exact ID 또는 근거 있는 기존 resolver alias만 비교하며 첫 항목을 보존한다. 재해석/덮어쓰기/비우기 가져오기는 제공하지 않는다. 잘못된 JSON/형식/version/ID/크기는 후보 저장 전에 거부한다. 오류 메시지에 파일 내용·항목값을 복사하지 않는다.

root 계약 API: `MAX_FAVORITES_BACKUP_BYTES=262144`, `formatFavoritesBackup(values,resolver): string`, `parseFavoritesBackup(text,resolver): {values,total,known,unresolved}` (오류는 고정 code를 가진 Error). `mergeFavorites(storage,resolver,values,{state,expectedRaw})`는 기존 저장 모듈의 단일 후보 저장·사전 compare·사후 verify·실패 rollback을 재사용한다. 확인 화면에서 읽은 원문과 현재 원문이 다르면 status conflict로 쓰지 않고 재선택을 요구한다. 같은 탭의 save/remove와 storage event도 대기 중인 확인을 무효화한다. 느린 파일 읽기 중 새 파일 선택/취소는 오래된 결과가 최신 화면을 덮지 못하게 한다.

저장소 손상/접근·쓰기 거절 시 원본을 보존하며 임시 세션 결과를 영속 성공과 구별한다. 마지막 정상 목록/동시 작성자의 bytes를 덮지 않는다. 병합을 여러 save 호출로 나누는 대안은 부분 저장 위험 때문에 택하지 않는다. JSON 파일은 사용자가 보관/전달하며 기존 metadata를 포함할 수 있음을 안내한다. 앱이 읽은 파일을 서버에 업로드하지 않는다. 이미 다운로드한 파일의 보관·공유는 사용자 선택이다.

실제 단위 반례·다운로드/다시선택/확인/합치기·동시변경·거절 저장·unknown/metadata·JSoff/360px/키보드·개인정보 안내를 검증한다. 공식 ledger/raw/chart/ID/URL/저장 키/원래 색상, preview noindex/무광고와 동일 SHA 배포/실제 복구 게이트는 그대로 적용한다.
