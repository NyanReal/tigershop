# STEP 02 — “탭하면 반응” (절구 탭 인터랙션)

목표: 중앙 절구(탭 대상)를 클릭/터치하면 **눌림 애니메이션 + 파티클/사운드**가 재생된다. (아직 자원 증감은 없음)

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

### 1) 절구 오브젝트 추가(월드 Actor 권장)
- `BP_TRCS_Mortar` (Actor)
  - StaticMesh(플레이스홀더 가능)
  - Collision: Visibility Trace에 히트되도록 설정
  - Dispatcher: `OnMortarTapped`

### 2) 입력 처리(PC/모바일 공통)
- `BP_TRCS_PlayerController`:
  - 클릭/터치 시 `GetHitResultUnderCursor` 또는 LineTraceByChannel(Visibility)
  - 히트 Actor가 `BP_TRCS_Mortar`면 탭 처리 호출

### 3) 시각/청각 피드백
- 눌림 애니:
  - Mortar Mesh Scale을 0.95로 줄였다가 0.2초 내 원복(Timeline)
- 이펙트:
  - Niagara 파티클(가루 튐) 플레이스홀더
- 사운드:
  - “탕!” SFX 플레이스홀더

### 4) 디버그
- 탭 시 `PrintString("Mortar Tap!")` 0.5초 표시(개발 빌드)

---

## 산출물
- `BP_TRCS_Mortar`
- (선택) `NS_TRCS_TapDust`
- (선택) `SFX_TRCS_Hit`
- PlayerController 입력 처리

---

## 완료 기준(AC)
- 절구를 탭하면 **항상** 눌림 연출이 보인다.
- 절구 외를 탭하면 반응이 없다.
- 터치 입력에서도 동일 동작한다.
