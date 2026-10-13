# ID·URL·즐겨찾기 호환 어댑터

Issue [jpoptoday#3](https://github.com/jpop-today/jpoptoday/issues/3) · SONG-001, FAVORITES-001 · 코드 contracts/legacy-compatibility.mjs. Node built-in/DOM/global storage 의존성이 없는 브라우저 ESM이며 앱 연결 전 독립 실행 계약이다.

## ID와 URL

```js
import { createIdentityResolver, resolveLegacyUrl } from './contracts/legacy-compatibility.mjs';
const resolver = createIdentityResolver(baseline); // 초기에는 explicit alias 없음
const result = resolveLegacyUrl('/song?id=' + encodeURIComponent(legacyId), resolver);
```

baseline3,317 ID를 그대로 canonicalId로 사용한다. 제목·번호를 고쳐도 ID를 다시 계산하지 않고 번호 공유11그룹을 자동 병합하지 않는다. 명시 alias는 `{alias,canonicalId,verified:true,evidence:'검토한 변경기록 참조'}`만 허용하며 known target·중복·체인·순환을 검사한다. verified 플래그는 호출자의 주장이고 근거의 진실성을 검증한 결과가 아니다. 현재 실제 alias는0개다.

`resolveLegacyUrl`은 동일 origin의 상대 href/상태만 반환하며 location/history를 직접 변경하지 않는다. known ID의 /song?id=, /song.html?id=를 /songs/<encoded-stable-id>/로 연결한다. /filter의 q·반복 number/tag·anime/artist/page·추가 query/hash는 /search/로 보존한다. /#favorites-view와 .html 공개 주소를 변환한다. unknown ID는 unresolved와 원래 값/href를 반환하고 다른 곡을 추측해 선택하지 않는다. 외부 origin·URL credential·중복 id·깨진 percent/UTF8·control 문자는 거부한다.

새 앱은 반환 상태를 처리하고 canonical route 실제 렌더를 제공해야 한다. 어댑터 성공은 현재 운영 사이트에 새 /songs/ 페이지가 배포됐다는 의미가 아니다.

## 즐겨찾기 실행 계약

```js
import { readFavorites, migrateFavorites, updateFavorites } from './contracts/legacy-compatibility.mjs';
const previous = readFavorites(localStorage, resolver);
const migrated = migrateFavorites(localStorage, resolver);
const next = updateFavorites(localStorage, resolver, { action: 'save', id });
const following = updateFavorites(localStorage, resolver, { action: 'save', id: anotherId }, { state: next });
```

키는 jpop_today_favorites를 유지한다. 문자열·이전{id} 목록을 읽고 known/missing/invalid entries·raw·diagnostics를 반환한다. 읽기에는 setItem/removeItem이 없다. 알 수 없는 ID와 이전 객체의 추가 필드를 보존한다. 사용자 명시 remove는 선택한 ID/검토된 canonical 관계만 지운다.

명시 migration은 원본 exact-byte backup을 고유 backup key에 저장·재확인→후보 파싱/검증→동시 변경 확인→기존 key 쓰기→읽어 확인 순서다. 이미 사용된 backup key에 다른 원본이 있으면 충돌로 중단한다. 계정 로그인·서버 저장을 도입한 것은 아니다.

손상 JSON·shape/entry 오류·SecurityError·quota에서 원본을 자동 복구/삭제하지 않고 session 상태를 반환한다. session 변경을 이어 사용하려면 직전 결과를 options.state로 전달한다. quota 상태를 영속 저장 성공으로 설명하지 않는다. 앱은 저장 제한과 재시도/복구 안내를 제공해야 한다.

쓰기 후 오류를 던지는 storage 구현은 원래 바이트로 rollback을 시도한다. 다른 writer의 변경은 덮지 않는다. rollback 자체가 실패하면 restored:false와 rollback-failed 진단·원본 backupKey를 반환한다. 이를 원본 보존 성공으로 표시하지 않는다. native localStorage의 setItem 원자성·오류와 임의 storage 구현의 동작을 구분한다.

## 최근 필터

jpop_today_recent_filters의 이전 snapshot과 버튼 entry를 읽는다. 화면은 최대5개를 보여주고 dedupe/savedAt 정렬하되 읽을 때 저장값을 자르거나 날짜를 현재로 만들지 않는다. unknown 형식·overflow는 원본에 남긴다. 명시 migrateRecentFilters는 인식한 완전 후보만 같은 backup/rollback 순서로 쓰고 overflow-preserved이면 쓰지 않는다.

## 실제 검증

Node 전용 테스트37개 통과: 실제3,317 ID old/new URL roundtrip, 모든 제목/번호 정정 이후 ID 유지, unknown 보존, alias 거부, 외부 URL·query escaping, storage 오류·동시 변경·backup 충돌·rollback 실패, 최근 필터 호환.

Browser plugin not available로 기존 Playwright와 Chromium154.0.8037.57을 사용했다. /tmp/jpop-compat-browser-check.mjs에서 로컬 합성 fixture의 ESM import·native localStorage exact read/backup·alias/URL·unknown 보존·save/remove·DOMException quota/session을17개 assertion으로 검사하고 pageerror0을 확인했다. 원본 UI·운영 사용자 저장값은 변경하지 않았다. runner/screenshot은 repo밖에 둔다.

이 단계는 라이브러리 동작이다. 새 앱 화면 연결, 실제 HTTP redirect/404, 모바일/보조기술 UI 검증과 preview/운영 전환은 후속 앱 검증에서 실행한다.
