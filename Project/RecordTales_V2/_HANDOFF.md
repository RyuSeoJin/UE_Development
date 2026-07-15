# RecordTales V2 — 설계 & 진행 인수인계 (HANDOFF)

> **이 문서의 목적:** 다른 컴퓨터/세션에서 V2 문서 생성을 이어가기 위한 자립적 인수인계.
> **⚠️ 주의:** 상세 설계 원장(플랜 파일 `~/.claude/plans/replicated-weaving-stearns.md`)과 메모리는 `~/.claude/`에 있어 **기기 간 이동이 안 됩니다.** 따라서 **이 문서(git 저장)가 다른 컴퓨터에서의 단일 기준**입니다.

---

## A. 현재 상태 (2026-07-09)
- 새 폴더 `Project/RecordTales_V2/` + 프로젝트 허브 `recordtales_v2-index.html` 생성.
- **완료 문서 9건** (허브 문서 목록에 링크 등록됨):
  - `00-recordtales-v2-overview.html` — 프로젝트 개요 (02-A)
  - `01-multi-guild-architecture.html` — 멀티 길드 아키텍처 (02-A)
  - `02-estate-management-economy.html` — 영지·경영 건물 경제 (02-A)
  - `03-data-design.html` — 데이터 설계 지도 (04 템플릿)
  - `04-gacha-ability-awakening.html` — 가챠·능력치 개화 (02-A)
  - `05-data-schema-spec.html` — 데이터 스키마 명세 (02-B, 모든 컬럼 코드+표 · .codeblock 탭+JS)
  - `06-honest-economy.html` — 정직한 경제·레벨 (02-A)
  - `07-archive-record-softdelete.html` — 아카이브·기록 ID·소프트 삭제 (02-A)
  - `08-phase1-core-systems.html` — Phase 1 코어(포커스·퀘스트·UI) (02-A)
- **✅ 문서 세트 완료.** 남은 것 = 실수치 튜닝(DataTable/Settings)뿐. (아래 D는 각 문서의 아웃라인 기록 — 이미 전부 생성됨.)

---

## B. 확정 설계 요약 (전체 — 이게 원장)

**멀티 길드 하이브리드 C:** 활동 도메인마다 길드. **계정 공유** = 캐릭터 로스터·Gold·조각·가챠 확률·**영지(경영 건물)**. **길드별** = 아카이브·레벨·카테고리(Guild→Category 2단). 가챠 확률 = **계정 XP**(전 길드 활동 시간 합).

**정직한 경제/레벨:** 레벨은 **실제 집중 시간으로만** — **60분(3600초)=1레벨**. 캐릭터·Gold로 못 올림(레벨=시간/3600 파생, 저장 안 함). 퀘스트는 XP 없이 Gold만. 레벨 **무제한**(천장 없음). 가챠 확률은 계정 레벨 고점에서 **plateau**.

**내비게이션/영지:** 영지 = **계정 단위 하나**, 앱 홈. 홈 기본 화면 = "오늘 할 일". 건물 선택 → **UI 패널**(공간 진입 아님). **골드 건물**(캐릭터 배치→Gold) + **기억 거래소**(캐릭터 분해→조각). 길드 건물엔 캐릭터 배치 없음(작업 순수). 전체 기록 = 기념비 오브젝트. 빠른시작 고속도로 없음. 제작: **Phase 1=기록·집중 본체(게임 없음), Phase 2=게임 레이어(가챠·영지)**.

**경영 건물 경제:** 산출 = `(건물 기본 + Σ캐릭터 flat) × 상태 배율`. 상태 배율: 일반 = 1 + Σ(일반 배율 보너스), 집중 = 기본 집중 배율 N(영지 규칙) + Σ(집중 배율 보너스). 배율 보너스 = (각 배율−1) 합산. **유휴 = 오프라인 누적 캡**(집중 중 N배). 건물 업그레이드로 기본↑·슬롯(≤3) 해금. 자원 2종(Gold·조각).

**캐릭터 특성 = 능력치:** 캐릭터 = **능력치 슬롯 최대 4** 개화. 능력치 3종: **flat 골드 / 유휴 배율 / 집중 배율**(전부 골드 건물 배치용). 능력치 **테마 매칭**(캐릭터 테마==건물 테마면 보너스, 비매칭도 배치 가능). 캐릭터 조합/건물 시너지 = 나중.

**가챠:** 보유 무제한·중복 허용·**3장 후보 1택**(스킵/리롤 없음). **두 등급**: ①캐릭터 등급(뽑을 때 롤=인스턴스; 개화 슬롯 수 + 능력치 등급 분포 결정) ②능력치 등급(일반~레전더리; 효과 크기=계수 tier). 능력치는 단계가 있고(고등급일수록 많음), 롤=시작 단계, **조각으로 상한까지 강화**(개화 능력치 중 가중 랜덤=남은 단계, +1). 출현 pity 없음(3장 미리보기가 완충). 캐릭터 타입=풀 균등 랜덤.

**아카이브:** 계정 단위 **통합 원장**, 기록마다 (Guild→Category→Source) 태그. 기록 ID `YYYYMMDDhhmmssSSS-{GUILDID}`(ms 정밀 + 생성순 길드번호; 최소 6자리 zero-pad이되 초과 시 자릿수 확장, 재사용 금지). **정렬은 문자열 아닌 정수/날짜 필드로**. v2→v3 마이그레이션은 구 기록을 "기본 길드"로 백필.

**소프트 삭제 + 휴지통(로컬):** 길드·아카이브 기록에 `bDeleted`+`DeletedTime`. 일반 화면=미삭제만, 휴지통=삭제분 복원, 완전삭제=실제 제거. Cascade(길드 삭제→그 기록 함께). 무기한 보존(용량 무의미). 성능: 삭제 항목은 휴지통 열 때만 로드. 서버 없음(로컬).

### 데이터 모델 (03 문서에 상세)
**정적 DataTable (밸런스):** ①DT_Character_Rarity_Ability_Open_Chance(등급→개화 슬롯 수) ②DT_Character_Rarity_Ability_Select_Chance(등급→능력치 등급 분포) ③DT_Ability_Type(RowName=효과×등급; DisplayName·AbilityRarity·EffectType[FlatGold/IdleMult/FocusMult]·SelectWeight) ③-2 DT_Ability_Step(RowName=AbilityId+단계 → Value·RollWeight·ShardCostToNext; **단계별 값·확률·비용 직접 지정, 등급별 단계 수 다름**) ④DT_Character(이름·종족(enum)·소속(enum)·직업·스토리·Icon·MatchedTheme; Rarity·MultiTarget 없음) ⑤DT_Gacha_Rate_ByLevel(계정 레벨→캐릭터 등급, plateau) ⑥DT_Building(정체성: 이름·타입·테마·해금조건·MaxSlots) ⑦DT_Building_Upgrade(RowName=건물id+레벨 → BaseOutput·SlotCount·UpgradeCost; 건물별 개별 곡선). **DT_ShardUpgrade는 별도 없음 — Ability_Step에 흡수.**
**런타임 SaveGame (계정 단위):** `FOwnedCharacter`{InstanceId·CharacterId·Rarity·Abilities[`FOwnedAbility`{AbilityId·CurrentStep}], ≤4} · `FBuildingState`{BuildingId·Level·DeployedInstanceIds·LastCollectTime} · `FEconomyState`{Gold·Shards} · `FGuildData`{GuildId·GuildName·Categories·FocusSeconds·bDeleted·DeletedTime, GetLevel()=FocusSeconds/3600} · 통합 Archive(FArchiveRecord) · NextInstanceId/NextGuildId/AccountFocusSeconds.
**전역 상수 (DeveloperSettings):** EstateFocusBaseNx, IdleOfflineCapHours, SecondsPerLevel=3600.
**DataTable 읽기 패턴:** RowName 외에 필드(AbilityId·StepIndex 등)도 컬럼으로 넣고, 로드 시 `TMap<FName, TArray<Row>>`로 캐시(그룹·정렬). 능력치는 CurrentStep만 저장, 값은 표 조회.

---

## C. 문서 생성 방법 (패턴 — 반드시 따를 것)
플레이북 `doc-process/DOC-GENERATION-PLAYBOOK.md` + `DOC-TRACKS.md`(Track B) + `design-template/TEMPLATE-CATALOG.md` 준수.

1. **STEP 2 아웃라인 → 사용자 확인 ①** (섹션 목록 + 컴포넌트 + 설명 전략, 템플릿 가판별). **확인 전 본문 금지.**
2. **STEP 3 Case A/B/C → 사용자 확인 ②.** 지금까지 전부 Case A(기존 템플릿). 템플릿: 02-A(아키텍처)·04(데이터 지도)가 주로 쓰임.
3. **본문 작성 — 조립 방식**(완료 문서들이 이 방식):
   - 스크래치에 `head` 조각(doctype~title~CHANGELOG 주석, `<style>` 앞까지)과 `body` 조각(`<body>`~`</html>`) 작성.
   - 조립: `{ cat head; echo; echo '<style>'; cat design-guide/design-guide-ue-master.css; echo '</style>'; echo '</head>'; cat body; } > OUT.html`
   - **마스터 CSS 전체 inline**(모든 컴포넌트+hljs 포함). `<style>`은 자기 줄에 오도록 head 뒤 `echo`로 개행.
   - 컴포넌트(마스터에 있음): `.rel-map/.rel-row/.rel-node`, `.tbl`+`.owner(o-subsystem/o-savegame/o-gamestate)`, `.pipe-step`, `.callout(info/tip/warn)`, `.var-card`+`.vbadge(row/save/run/cfg)`, `.stat-row/.stat-card`, `.tree`, `.legend`, `.badge`.
   - **코드는 hljs span 직접 구워넣기**: `<span class="hljs-meta">USTRUCT()</span>`, `hljs-keyword`(struct/class/public/const/void/for/if/return), `hljs-type`(FString/int32/TArray/커스텀), `hljs-comment`, `hljs-number`, `hljs-title`(함수명), `hljs-literal`(true/false). 의미 있는 줄에 **한국어 '왜' 주석**(v3.8). `<`·`>`·`&`는 `&lt;`·`&gt;`·`&amp;`. **코드 블록은 `.codeblock` 래핑**(2026-07-12 사용자 피드백으로 변경): `<div class="codeblock">` > `.codeblock-bar`(파일명 `.codeblock-tab tab-h|tab-cpp active` + `.codeblock-copy` 복사 버튼) > `.codeblock-panel active` > `<pre class="hljs">`. 블록이 한 파일 내용이면 단일 탭(파일명 표기), .h/.cpp 쌍이면 탭 2개+패널 2개. 문서 끝 `</body>` 직전에 탭 전환·복사 JS(v1 문서 01-focus-system.html과 동일 스니펫) 필수.
   - **문서 간 상대링크 금지**(연결은 허브 담당). 실수치는 "DataTable 튜닝" 참조로 넘김.
   - CHANGELOG를 각 파일 head 주석에.
4. **STEP 8 — 허브 등록:** `recordtales_v2-index.html`의 `#docs` 표에서 해당 행의 상태를 `<a href="./파일.html">열기 →</a>`로 링크.
5. **검증:** `<pre class="hljs">` 블록별 `<span>`/`</span>` 개수 일치, 사용 클래스가 마스터에 존재, v1 잔재 없음.

---

## D. 문서 아웃라인 (참고 — ✅ 06·07·08 모두 생성 완료)

### 06 정직한 경제 · 레벨 (`06-honest-economy.html`, 02-A, Case A)
1. **개념** — 정직한 레벨(60분=1레벨, 시간 거울), 왜(캐릭터·Gold로 못 올림). callout.
2. **자원 흐름** — Gold(활동+골드건물 → 가챠·업그레이드·해금) / 조각(분해 → 강화) / XP(활동 → 길드 레벨+계정 XP). 표/관계도.
3. **레벨 규칙** — 60분=1레벨 선형·무제한, 퀘스트 XP 없음, 계정 XP=전 길드 합→가챠 plateau. pipe-step/callout.
4. **해금 트리** — Gold로 새 건물/테마·배경/BGM 꾸미기(방향, 수치 튜닝). 표.
5. **스켈레톤** — `UEconomySubsystem`(AddFocusTime→길드+계정, Gold 지갑, 해금). hljs.
6. **확장** — Do/Don't(레벨 저장 금지·plateau·해금 추가).

### 07 아카이브 · 기록 ID · 소프트 삭제 (`07-archive-record-softdelete.html`, 02-A, Case A)
1. **개념** — 통합 원장 + (Guild→Category→Source) 태그, 왜(분리 뷰 + 전체 조망).
2. **기록 ID** — `YYYYMMDDhhmmssSSS-{GUILDID}` 규칙, 정렬은 정수/날짜 필드로, 자릿수 확장. 표/예시.
3. **뷰** — 홈 "오늘 할 일" / 길드별 / 기념비 전체 / 캘린더. pipe-step.
4. **소프트 삭제·휴지통** — bDeleted+DeletedTime, 복원/완전삭제, cascade, 무기한. pipe-step + callout.
5. **스켈레톤** — `FArchiveRecord`(+GuildId·bDeleted), `URecordSubsystem`(AddRecord·SoftDelete·Restore·Purge). hljs.
6. **확장** — 마이그레이션(v2→v3 기본 길드 백필), 삭제 항목 지연 로드.

### 08 포커스 · 퀘스트 · UI (이관/개정, `08-*.html`)
- v1 문서(01 포커스, 02 퀘스트, 07 UI, 10 UMG)에서 **개념만 참고**하되 V2 맥락 반영: 포커스 보상 출력 재배선(XP→계정 풀+길드, Gold→공유), 퀘스트 XP 제거(Gold만), UI에 길드 선택기·홈 "오늘 할 일". 필요 시 문서 분할.

---

## E. 참고
- 완료 문서 01·02·04(코드 포함)는 마스터 CSS **전체 inline** + hljs 구워넣기 예시. 03(데이터)은 `.var-card`/`.vbadge` 예시.
- 마스터 CSS 버전 = **v1.5**. 결과물은 생성 시점 스냅샷.
- 실수치(활동당 Gold·건물 산출·확률표·조각 비용·plateau 지점·estate Nx·유휴 캡 시간)는 전부 **DataTable/Settings 튜닝** — 문서엔 공식·구조만.
