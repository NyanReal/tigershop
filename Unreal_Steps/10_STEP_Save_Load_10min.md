# STEP 10 — 저장/로드 + 첫 10분 플레이 완주

목표: 재실행해도 진행이 유지되고, 첫 10분 동선이 자연스럽게 완주된다.

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

### 1) SaveGame
- `BP_TRCS_SaveGame`
  - `F_TRCS_ResourceState`
  - 업그레이드 레벨: `TMap<Name,int>`
  - 시설 설치 상태: `TSet<Name>` 또는 `TMap<Name,bool>`
  - 꾸미기 장착: `TMap<E_TRCS_DecorSlot, Name>`

### 2) 저장/로드 시점
- 로드: GameInstance Init 또는 MainMap BeginPlay
- 저장:
  - 30초마다 자동 저장
  - 업그레이드 구매/시설 설치/꾸미기 구매 직후 저장

### 3) “첫 10분” 가이드(간단 퀘스트 텍스트)
- 화면에 작게 목표 표시(토스트/퀘스트 위젯)
  1) 절구 탭으로 떡 3개 만들기
  2) 떡 팔아 50냥 모으기
  3) 호랑이 고용
  4) 업그레이드 2개 구매
  5) 시설(시루) 설치
  6) 꾸미기 1개 장착

### 4) 밸런스 패스(데이터만 조정 가능하게)
- 비용/가격/스폰 주기 등은 DataTable에서 조정 가능해야 함

---

## 산출물
- `BP_TRCS_SaveGame`
- GameInstance의 Save/Load 함수
- 간단 퀘스트/가이드 UI

---

## 완료 기준(AC)
- 재실행 후에도 돈/재고/업그레이드/시설/꾸미기 상태가 유지된다.
- 개발자 테스트로 10분 내 목표를 달성 가능하다.
- 밸런스 튜닝이 DataTable 수정만으로 가능하다.
