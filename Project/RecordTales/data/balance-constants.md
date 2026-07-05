# RecordTales 밸런스 상수 정의서

> URecordTalesSettings (UDeveloperSettings, Config=Game)에 노출되는 모든 밸런스 상수.
> Project Settings ▸ Game ▸ RecordTales Settings에서 수정 가능.
> 단일 소스: 이 문서. 각 설계 문서(01~08)에서 분산 기술된 값을 통합.

## 시간 관리 (Focus)

| 상수명 | 타입 | 기본값 | 단위 | 설명 | 출처 |
|--------|------|--------|------|------|------|
| DefaultFocusDuration | int32 | 1500 | 초 (25분) | 포모도로 기본 집중 시간 | 01 |
| DefaultRestDuration | int32 | 300 | 초 (5분) | 포모도로 기본 휴식 시간 | 01 |
| DefaultLastRestDuration | int32 | 900 | 초 (15분) | 마지막 루틴 휴식 시간 | 01 |
| MaxRepeatCount | int32 | 10 | 회 | 루틴 최대 반복 횟수 | 01 |
| StopwatchXPThreshold | float | 1500.0 | 초 | 스톱워치 XP 적립 최소 시간 | 01 |

## 경제 (Economy)

| 상수명 | 타입 | 기본값 | 단위 | 설명 | 출처 |
|--------|------|--------|------|------|------|
| GoldTickInterval | float | 1.0 | 초 | Gold 적립 주기 (집중 상태에서) | 01, 05 |
| GoldPerTick | int32 | 1 | Gold | 틱당 Gold 적립량 | 05 |
| XPPerPomodoroComplete | int32 | 15 | XP | 포모도로 1사이클 완료 XP | 05 |
| XPPerStopwatchMinute | int32 | 10 | XP/분 | 스톱워치 분당 XP (threshold 이후) | 05 |
| XPComboBonus | int32 | 20 | XP | 3연속 포모도로 보너스 | 05 |
| MaxGuildLevel | int32 | 20 | Lv | 길드 최대 레벨 | 05 |
| LevelXPBase | int32 | 100 | XP | 레벨업 XP 기본값 (Lv1→2) | 05 |
| LevelXPMultiplier | float | 1.4 | 배수 | 레벨 XP 곡선 승수 | 05 |
| LevelGoldCostBase | int32 | 500 | Gold | 레벨업 Gold 기본값 | 05 |
| LevelGoldCostPerLevel | int32 | 500 | Gold×Lv | 레벨업 Gold = Base × Lv | 05 |

## 가챠 & 길드 (Gacha & Guild)

| 상수명 | 타입 | 기본값 | 단위 | 설명 | 출처 |
|--------|------|--------|------|------|------|
| GachaCostGold | int32 | 1000 | Gold | 가챠 1회 비용 | 04 |
| GachaCardCount | int32 | 3 | 장 | 뽑기당 표시 카드 수 | 04 |
| GachaPickCount | int32 | 1 | 장 | 카드 중 선택 가능 수 | 04 |
| MaxSquadSlots | int32 | 4 | 슬롯 | 스쿼드 최대 멤버 수 | 04 |
| DefaultSquadSlots | int32 | 2 | 슬롯 | 초기 스쿼드 슬롯 수 | 04 |
| MaxPresets | int32 | 3 | 개 | 스쿼드 프리셋 최대 수 | 04 |
| MaxOwnedMembers | int32 | 8 | 명 | 캐릭터 보유 상한 | 04, 08 |
| CharacterPoolSize | int32 | 12 | 명 | 전체 캐릭터 풀 수 | 04 |

## 퀘스트 (Quest)

| 상수명 | 타입 | 기본값 | 단위 | 설명 | 출처 |
|--------|------|--------|------|------|------|
| QuestRecurrenceMaxDays | int32 | 365 | 일 | 반복 퀘스트 자동 종료 기간 | 02 |
| RolloverCheckInterval | float | 60.0 | 초 | 자정 롤오버 체크 주기 | 02 |
| QuestCompletionGold | int32 | 50 | Gold | 퀘스트 완료 기본 보상 | 02, 05 |

## 오디오 & 비주얼 (Audio & Visual)

| 상수명 | 타입 | 기본값 | 단위 | 설명 | 출처 |
|--------|------|--------|------|------|------|
| MusicPreviewDuration | float | 30.0 | 초 | 미해금 트랙 미리듣기 시간 | 06 |
| DefaultMusicVolume | float | 0.7 | 0~1 | 뮤직 기본 볼륨 | 06 |
| DefaultAmbientVolume | float | 0.5 | 0~1 | 앰비언트 기본 볼륨 | 06 |
| AmbientTypeCount | int32 | 5 | 종 | 앰비언트 사운드 유형 수 | 06 |
| MusicTrackCount | int32 | 6 | 곡 | BGM 트랙 수 | 06 |
| BackgroundCount | int32 | 9 | 개 | 배경 테마 수 | 06 |

---

> 이 문서는 개발 시 URecordTalesSettings.h에 UPROPERTY로 선언할 필드의 사전 정의입니다.
> 값 변경 시 이 문서를 먼저 갱신하고, 코드에 반영합니다.
