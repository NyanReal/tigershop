# STEP 01 — 프로젝트/레벨/기본 UI 프레임

목표: 게임을 실행하면 **기본 화면 레이아웃(상단 자원바/중앙 탭영역/우측 손님 영역/하단 탭바)**이 보이도록 만든다.

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

### 1) 프로젝트 생성/설정
- UE5 새 프로젝트: **Games → Blank**, Blueprint, Starter Content 선택은 자유
- Project Settings:
  - Input: Touch/Mouse 입력 활성
  - UI: DPI Scaling 기본 유지(필요 시 커브 조정)
  - Maps & Modes: 기본 맵을 `Map_TRCS_Main`로 지정(가능하면)

### 2) 맵/레벨 구성
- `/Content/TRCS/Maps/`에 `Map_TRCS_Main` 생성
- 카메라:
  - 고정 카메라(움직이지 않음)로 시작
- 배경:
  - 플레이스홀더 배경(Plane + Material 또는 UMG 이미지)

### 3) GameMode / PlayerController / HUD 연결
- `BP_TRCS_GameMode` 생성 후 `Map_TRCS_Main`에 적용
- `BP_TRCS_PlayerController` 생성 후 GameMode에 지정
- `WBP_TRCS_HUD` 생성 및 BeginPlay에 AddToViewport

### 4) UI 프레임(레이아웃만)
`WBP_TRCS_HUD` 구성(UMG):
- 상단: `WBP_TRCS_TopBar`
  - 돈(냥) 텍스트
  - 명성(소문) 텍스트
  - 손맛 포인트 텍스트(잠금 아이콘/회색 처리 가능)
- 중앙: 절구/탭 영역 자리(이미지/패널)
- 우측: 손님 대기열 패널 자리
- 하단: `WBP_TRCS_BottomTabs`
  - 버튼 5개: 업그레이드/시설/호랑이/레시피(손맛)/꾸미기
  - 클릭 시 패널 전환(실제 패널 내용은 다음 단계에서 채움)

---

## 산출물
- `Map_TRCS_Main`
- `BP_TRCS_GameMode`, `BP_TRCS_PlayerController`
- `WBP_TRCS_HUD`, `WBP_TRCS_TopBar`, `WBP_TRCS_BottomTabs`

---

## 완료 기준(AC)
- 게임 실행 시 `Map_TRCS_Main`이 로드되고 `WBP_TRCS_HUD`가 표시된다.
- 상단/하단 UI 프레임이 다양한 해상도에서 깨지지 않는다.
- 하단 탭 버튼 클릭 시 “선택 상태” 시각 피드백이 바뀐다(내용 패널은 빈 패널이어도 됨).
