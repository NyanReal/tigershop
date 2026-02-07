# STEP 04 — 돈(냥) 시스템 + 즉시 판매 버튼(가짜 손님 단계)

목표: “판매하기” 버튼으로 **떡 재고 → 돈(냥) 전환**이 되고 상단 바에 즉시 반영.

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

### 1) 가격 정의
- `BaseTteokPrice = 5` 냥 (MVP 고정)
- 판매 로직(기본 1개 판매):
  - 재고 0이면 버튼 비활성 또는 토스트 표시

### 2) UI 추가
- 버튼: “판매하기”
- 상단 바 돈 텍스트: `냥: {Money}`

### 3) 판매 처리 함수(권장)
- 상태 객체에 `TrySellTteok(Amount:int32)` 구현
  - 성공 시 떡 감소, 돈 증가, 상태 변경 이벤트 발송

### 4) 시각 피드백
- 판매 성공 시 돈 텍스트가 잠깐 튀는 애니(Scale) 또는 `+5` 플로팅

---

## 산출물
- 판매 버튼 UI
- 판매 로직 함수

---

## 완료 기준(AC)
- 떡 ≥ 1: 판매 버튼 → 떡 -1, 돈 +5가 보인다.
- 떡 = 0: 판매 불가 피드백이 있다.
