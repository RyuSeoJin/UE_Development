# RecordTales 가챠 확률 테이블

> 길드 레벨별 희귀도 가중치. RequestGacha() 호출 시 이 테이블로 확률 계산.
> 3장 뽑기 각각 독립 확률 적용, 중복 허용.

## 희귀도별 가중치 (길드 레벨 기준)

| 길드 레벨 | Normal | Rare | Epic | Legendary | 비고 |
|-----------|--------|------|------|-----------|------|
| 1~3 | 70 | 25 | 4 | 1 | 초기 단계 |
| 4~6 | 65 | 27 | 6 | 2 | |
| 7~9 | 60 | 28 | 9 | 3 | |
| 10~12 | 55 | 29 | 12 | 4 | |
| 13~15 | 50 | 30 | 14 | 6 | |
| 16~18 | 45 | 30 | 17 | 8 | |
| 19~20 | 40 | 30 | 19 | 11 | 최대 레벨 구간 |

## 확률 계산 공식

```
확률(희귀도) = 해당 희귀도 가중치 / (Normal + Rare + Epic + Legendary)
```

예: 길드 레벨 10 → Normal=55, Rare=29, Epic=12, Legendary=4, 합계=100
- Normal 확률 = 55%
- Rare 확률 = 29%
- Epic 확률 = 12%
- Legendary 확률 = 4%

## 캐릭터 풀 매핑

| 희귀도 | 캐릭터 |
|--------|--------|
| Normal | char_archer, char_warrior, char_thief, char_assassin, char_bard (5명) |
| Rare | char_knight, char_mage, char_healer (3명) |
| Epic | char_sage, char_paladin (2명) |
| Legendary | char_dragon, char_fairy (2명) |

## 중복 정책
- 이미 보유한 캐릭터가 다시 뽑힐 수 있음 (중복 허용)
- MaxOwnedMembers(8) 도달 시 가챠 자체가 차단됨
