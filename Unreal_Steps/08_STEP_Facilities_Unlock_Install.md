# STEP 08 — 시설 해금/설치(시루/포장대) 1~2개

목표: 시설을 설치하면 **월드에 오브젝트가 추가**되고, 효과가 실제로 반영된다.

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

### 1) 시설 데이터테이블
- Struct: `F_TRCS_FacilityRow`
  - `FacilityId`, `DisplayName`
  - `CostMoney`, `UnlockReputation`
  - `FacilityType`(Enum)
  - `EffectType`, `EffectValue`
  - `FacilityClass`(BP Class Reference)
- DataTable: `DT_TRCS_Facilities`

### 2) 시설 BP
- `BP_TRCS_FacilityBase` (Actor)
- 자식 2개:
  - `BP_TRCS_Facility_Steamer` (시루): PriceMultiplier +10% 등
  - `BP_TRCS_Facility_Packaging` (포장대): 대기열 처리/만족도 보정 등(간단히 QueueSize +1로 대체 가능)

### 3) 설치 UI
- `WBP_TRCS_Facilities`
  - 해금 조건(명성) 만족 시 활성화
  - 설치 버튼 클릭 → 돈 차감 → 월드 지정 슬롯에 스폰
  - 설치 후 “설치됨” 표시

### 4) 배치(드래그 없음)
- 월드에 “시설 슬롯” Transform 2~4개 미리 배치
- 설치 시 비어 있는 슬롯에 자동 배치

---

## 산출물
- `DT_TRCS_Facilities`
- `WBP_TRCS_Facilities`
- 시설 BP 2종 + 슬롯 배치

---

## 완료 기준(AC)
- 조건 충족 시 시설 설치 가능.
- 설치 시 월드에 시설이 보이고, 수익이 눈에 띄게 증가한다.
