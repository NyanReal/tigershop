# STEP 06 — 자동 생산: 호랑이 1마리 고용/작업

목표: 호랑이를 고용하면 탭 없이도 떡이 자동으로 증가한다(작업 연출 포함).

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

### 1) 호랑이 액터
- `BP_TRCS_TigerWorker`
  - 변수: `AutoTapInterval` (예: 1.0s), `AutoTapPower`(TapPower와 동일 또는 별도)
  - 작업 시작 시 Timer로 절구 작업 반복 호출

### 2) 절구 작업 함수 재사용
- 수동 탭과 자동 탭이 같은 함수(예: `ApplyMashing(Amount)`)를 타게 만들어 중복 제거
- 자동 작업 시에도 절구 눌림/이펙트를 트리거(“일한다”가 보이도록)

### 3) 고용 UI
- 버튼: “호랑이 고용(50냥)”
  - 돈 차감 후 호랑이 스폰 또는 활성화
  - 이미 고용했으면 버튼 비활성

### 4) 좌측 영역 시각화
- 좌측에 호랑이(플레이스홀더) 표시
- 작업 시 간단 애니(Timeline로 좌우 흔들림 등)

---

## 산출물
- `BP_TRCS_TigerWorker`
- 고용 버튼/비용 처리

---

## 완료 기준(AC)
- 호랑이 고용 후 탭 없이 떡이 일정 간격으로 증가한다.
- 자동 작업 시 절구/이펙트 반응이 보인다.
