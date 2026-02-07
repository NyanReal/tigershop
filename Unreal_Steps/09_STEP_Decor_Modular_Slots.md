# STEP 09 — 꾸미기(6슬롯) MVP: 배경/간판/등불 교체

목표: 꾸미기에서 선택하면 **즉시 외형이 바뀌는 것**이 보인다. (하우징/드래그 배치 없음)

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

### 1) 슬롯 Enum
- `E_TRCS_DecorSlot`: Signboard, LanternLeft, LanternRight, Background, Floor, ShowcaseSkin

### 2) 꾸미기 데이터테이블
- Struct: `F_TRCS_DecorRow`
  - `DecorItemId`, `Slot`, `DisplayName`
  - `CostMoney` 또는 `bFree`
  - `AssetRef`(Texture/Material/Mesh)
  - `ThemeTag`
- DataTable: `DT_TRCS_Decor`

### 3) UI
- `WBP_TRCS_Decor`
  - 슬롯 탭(6)
  - 아이템 그리드(선택 즉시 적용)
  - 구매가 필요하면 “구매 후 장착”
  - 현재 장착 표시(체크/테두리)

### 4) 월드 적용
- 배경: UMG 이미지 교체 또는 Plane Material 교체
- 간판/등불/진열대 스킨: Mesh 또는 Material/Texture 스왑
- 적용 담당:
  - `BP_TRCS_DecorAnchors`(월드에 1개)로 각 슬롯의 대상 Mesh 참조를 한 곳에서 관리

### 5) 성능 원칙
- 성능 영향 없음(권장). 대신 연출/대사 테마화는 옵션.

---

## 산출물
- `DT_TRCS_Decor`
- `WBP_TRCS_Decor`
- `BP_TRCS_DecorAnchors`

---

## 완료 기준(AC)
- 배경/간판/등불 중 최소 2종은 선택 즉시 확실히 바뀐다.
- 구매/장착 상태가 저장(다음 단계 Save) 가능한 구조로 되어 있다.
