# 원본 staging 실행 계약

Issue [jpoptoday#3](https://github.com/jpop-today/jpoptoday/issues/3) · SRC-001, DATA-001 · 구현 상태: importer와 실제 비공개 staging 생성·검증. 앱 이관/공개 v1 승격/운영 배포는 별개다.

## 재현 가능한 입력

운영 SHA는 Cloudflare API로 확인한6dbe5a65d1bd729c09393f746f715f4363089a43이다([운영 기준선](../operations/cloudflare-baseline.md)). importer는 정확한 Git commit의 raw JSON·TSV·generated playlist/chart와 기존 두-SHA identity 기준선을 읽는다. 소스 코드 실행·eval·실시간 수집을 하지 않는다. 원본 checkout 밖의 출력만 허용하고 baseline 파일/link를 출력으로 덮지 않는다.

```sh
cd /workspace/jpoptoday
node scripts/import-legacy-staging.mjs /path/to/original-checkout 6dbe5a65d1bd729c09393f746f715f4363089a43 data/legacy-identity-baseline.json data/staging/legacy-catalog.json --check
```

`--check` 없이 실행하면 입력 검증 후 출력 디렉터리의 exclusive 임시 파일→sync→close→atomic rename으로 생성한다. `--check`는 같은 입력으로 재생성한 완성 바이트가 저장된 파일과 같은지 검사하며 쓰지 않는다. 출력 parent는 존재해야 한다. 현재 파일은3,804,788bytes이다.

## 데이터와 보존

| 필드 | 의미 |
| --- | --- |
| stagingVersion / purpose | 1 / legacy-catalog-staging. v1 검증된 공개 catalog schemaVersion이 아님 |
| repository / revision | 고정 원본 저장소·운영 소스 SHA |
| sources | SHA·원본 path·SHA256·행 수 |
| songs[].id | 기존 legacy ID 자체를 최초 영속 ID로 고정 |
| availability | current 또는 archived |
| origin | playlist 또는 chart-only |
| titleOriginal / titleKo / artistOriginal / artistKo / tags | 관측 메타데이터와 인용 처리 교정된 표시 제목 |
| legacy | 원래 제목·가수, 변경 전 표기 보존 |
| karaoke.* | observed-unverified/unknown, observedNumber 문자열/null, evidence:null |
| refs | 원본 source index·row index, TSV physical line/endLine |
| titleCorrection | TSV 인용 처리 교정의 original/corrected/kind |
| unresolvedAliases | 공유 번호 후보11그룹, 자동 동일 곡 판정 없음 |

실제 ID3,317개 = playlist3,299 + 현재 chart-only14 + archived chart-only4. 현재 운영 카탈로그3,313 ID와 이전main에만 남은4 ID를 모두 보존한다. 제공사 번호 문자열·선행0을 보존하고 번호가 없으면 unknown으로 기록한다. 출처 없는 번호를 확인된 registered로 바꾸거나 번호 확인일을 새로 만들지 않는다.

가사 본문·가사 파일 참조·이미지·광고 정보·원본 sourceTitle/sourceUrl·checkedAt 필드는 복제하지 않는다. 원본 전체의 hash는 provenance이지만 번호가 공식 확인됐다는 증거는 아니다. 서술형 원본 콘텐츠를 공개하는 importer가 아니다.

TSV3곡의 인용부호를 정확히 읽어 표시 제목을 바로잡았지만 ID는 재계산하지 않는다. quoted tab/newline·doubled quote·BOM·CRLF와 physical 행 범위를 지원한다. 깨진 인용·누락 columns는 실패한다.

## 전체 범위 게이트

baseline을 모든 원본 Git 객체에서 재구성해 깊은 비교하고, raw와 generated playlist 전행의 메타데이터 일치를 확인한다. TSV3299행을 제목·가수·TJ/KY의 정확한 tuple로 raw에 일대일 연결한다. 번호만으로 다른 곡에 연결하지 않는다. 빈/부분 입력, counts 불일치, duplicate/ambiguous tuple, baseline tampering, mutable ref, executable JS를 거부한다. 새 커밋의 부분 snapshot이 valid structure여도 기존 전체 출력을 덮지 않는다.

정확한 baseline이 필요한 source revision이 아직 그 기준선에 없으면 먼저 별도 source audit로 ID/history를 보강한다. 삭제/병합/검증 완료는 별도의 명시적 검토 작업이며 importer 옵션으로 몰래 허용하지 않는다.

## 검증과 남은 범위

새 staging 테스트19개와 실제 `--check`가 통과했다. CI는 외부 개인 repo 없이 committed artifact의 ID·count·관측 상태·3교정·4archive와 synthetic 실패를 검사한다. `--check`로 원본 바이트까지 재대조하려면 해당 Git 객체가 있어야 한다.

공식 번호 진위·이용권리·서로 다른 버전의 번호11그룹·공개 read model·새 앱 UI 연결은 미완료다. 이관 metadata 성공을 v1 공개 catalog 검증이나 AdSense 승인으로 설명하지 않는다.
