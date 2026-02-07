# STEP 07 — 업그레이드 UI(데이터 기반) + 수치 반영

목표: 업그레이드 구매가 **데이터 기반**으로 동작하고, 구매 즉시 생산/판매가 변화한다.

## 공통 규칙(모든 단계 적용)

### 프로젝트 폴더 구조(권장)
- `/Content/TRCS/Blueprints/`
- `/Content/TRCS/UI/`
- `/Content/TRCS/Data/`
- `/Content/TRCS/Art/` (임시 플레이스홀더 포함)
- `/Content/TRCS/Audio/` (임시 사운드 포함)
- `/Content/TRCS/Maps/`

### 블루프린트/자산 네이밍(권장)
- 게임 인스턴스: `BP_TRCS_GameInstance`
- 게임 상태(세션 런타임): `BP_TRCS_GameState` (또는 Subsystem)
- 플레이어 컨트롤러: `BP_TRCS_PlayerController`
- 매니저(월드): `BP_TRCS_WorldManager`
- 손님: `BP_TRCS_Customer`
- 호랑이: `BP_TRCS_TigerWorker`
- 절구(탭 대상): `BP_TRCS_Mortar`
- UI 루트: `WBP_TRCS_HUD`
- 상단 바: `WBP_TRCS_TopBar`
- 하단 탭바: `WBP_TRCS_BottomTabs`
- 손님 패널: `WBP_TRCS_CustomerQueue`
- 업그레이드 패널: `WBP_TRCS_Upgrades`
- 시설 패널: `WBP_TRCS_Facilities`
- 꾸미기 패널: `WBP_TRCS_Decor`

### 데이터 주도 설계(최소)
- 통화/자원은 `F_TRCS_ResourceState` 구조체로 관리
- 업그레이드/시설/꾸미기 항목은 DataTable 또는 Primary Data Asset로 관리
- 수치는 “하드코딩 최소화” — 적어도 DataTable에서 조정 가능하게 만들기

### 완료 기준(AC) 표기
- **AC** 항목이 모두 만족되면 해당 단계 완료로 간주


---

## 작업 범위

### 1) 업그레이드 데이터테이블
- Struct: `F_TRCS_UpgradeRow`
  - `UpgradeId`(Name)
  - `DisplayName`, `Description`
  - `BaseCostMoney`, `CostMultiplier`
  - `MaxLevel` (0=무한)
  - `EffectType`(Enum), `EffectValue`(float)
- DataTable: `DT_TRCS_Upgrades`

### 2) Modifiers 구조
- `F_TRCS_Modifiers`
  - `TapPowerBonus`
  - `AutoIntervalMultiplier` (작을수록 빨라짐)
  - `PriceMultiplier`
  - `CustomerSpawnIntervalMultiplier`
  - `QueueSizeBonus`
- “현재 스탯”은 `Base + Modifiers`로 계산

### 3) 업그레이드 UI
- `WBP_TRCS_Upgrades`
  - ScrollList에 업그레이드 항목 생성
  - 항목: 이름/설명/레벨/가격/구매 버튼
- 구매 시:
  - 돈 차감
  - 레벨 저장(Map<Name,int>)
  - Modifiers 갱신
  - HUD/관련 시스템 업데이트 이벤트

### 4) MVP 업그레이드 6개(눈에 보이게)
1. TapPower +5
2. AutoInterval ×0.9
3. PriceMultiplier +10%
4. CustomerSpawnInterval ×0.9
5. QueueSize +1
6. (옵션) CriticalChance +5% (연출만)

---

## 산출물
- `DT_TRCS_Upgrades`
- `WBP_TRCS_Upgrades` + 항목 위젯
- Modifiers 적용 로직

---

## 완료 기준(AC)
- 업그레이드 리스트/수정이 DataTable 변경으로 가능하다.
- TapPower/Auto/Price 중 최소 2개는 구매 즉시 체감(바 속도/돈 증가)이 된다.
