# STEP 05 — 손님 등장 + 주문 말풍선(실제 손님 MVP)

목표: 손님이 주기적으로 등장해 우측 패널에 표시되고, **주문**이 뜨며, 재고가 있으면 자동 구매 후 떠난다.

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

### 1) 손님 타입 Enum
- `E_TRCS_CustomerType`: AhnNae, SeonBi, NaGeunNe, Merchant

### 2) 손님 데이터 구조(권장)
- `F_TRCS_CustomerOrder`
  - `CustomerType`
  - `OrderAmount` (1~3)
  - `RequestedItemId` (MVP는 기본 떡 1종으로 고정 가능)

### 3) 월드 매니저(스폰/큐)
- `BP_TRCS_WorldManager` (레벨에 1개 배치)
  - 손님 스폰 타이머: 예) 6초마다
  - 최대 대기열: 3명
  - 내부 큐: Array of `F_TRCS_CustomerOrder`

### 4) UI 표시(권장: 패널 카드 방식)
- `WBP_TRCS_CustomerQueue`
  - 큐의 주문들을 카드로 표시
  - 카드 내용: 손님 타입 아이콘/이름, 주문 수량, 대사 한 줄

### 5) 자동 구매 로직
- 1초마다(또는 이벤트) 큐의 첫 손님이 구매 시도:
  - 재고 ≥ 주문수량:
    - 재고 감소, 돈 증가(가격×수량), 명성 +1(기본)
    - 선비는 명성 +2(추가)
    - 큐에서 제거
  - 부족하면 대기(불만/퇴장은 MVP에서 생략 가능)

---

## 산출물
- `BP_TRCS_WorldManager`
- `WBP_TRCS_CustomerQueue` + 카드 위젯
- 손님/주문 데이터 구조

---

## 완료 기준(AC)
- 손님이 주기적으로 큐에 나타난다.
- 재고가 있으면 자동 구매로 돈/명성이 증가하고 큐에서 제거된다.
- 큐 최대치가 지켜진다.
