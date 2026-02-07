# STEP 03 — 진행도 게이지 + 떡 1개 생성(수동 생산)

목표: 절구 탭마다 **진행도(0~100%)**가 오르고, 100%마다 **떡 재고 +1**. UI 즉시 반영.

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

### 1) 런타임 상태 구조 설계
- 구조체 `F_TRCS_ResourceState`
  - `int64 Money`
  - `int32 Reputation`
  - `int32 TteokCount` (재고)
  - `float MortarProgress` (0~100)
- 상태 보관:
  - MVP는 `BP_TRCS_GameInstance`에 저장(세션 유지)

### 2) 탭 → 진행도 증가
- 변수:
  - `TapPower` 기본값 10
- 로직:
  - 탭 시 `MortarProgress += TapPower`
  - `MortarProgress >= 100`이면:
    - `MortarProgress -= 100`
    - `TteokCount += 1`
    - 떡 완성 팝업(+1 플로팅) 표시

### 3) UI 표시
- 중앙에 진행도 바(ProgressBar) + 퍼센트 텍스트
- 재고 텍스트: `떡: {TteokCount}`
- 갱신 방식:
  - Tick 바인딩 지양, **이벤트 기반 갱신**
  - 예: GameInstance에 `OnStateChanged` Dispatcher → HUD가 수신

### 4) 시각적 보상(선택)
- 떡 완성 시 “떡 아이콘”이 재고 영역으로 이동(UMG 애니)

---

## 산출물
- `F_TRCS_ResourceState` (Struct)
- `BP_TRCS_GameInstance` (상태 저장 + 이벤트)
- HUD 진행도/재고 표시 및 갱신

---

## 완료 기준(AC)
- 탭 → 진행도 바 상승, 100%마다 떡 재고 +1.
- 초과분 이월(예: 95에서 +10이면 5 남김).
- UI가 즉시 갱신된다.
