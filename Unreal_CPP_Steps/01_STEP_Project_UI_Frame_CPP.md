# STEP 01 — 프로젝트/레벨/기본 UI 프레임(C++)

목표: 실행하면 메인 맵에서 HUD 프레임(UI 레이아웃)이 표시된다.

## 공통 규칙(모든 단계 적용)

### 1) 소스/모듈 구조(권장)
- 모듈: `TigerRiceCakeShop` (프로젝트 기본 모듈)
- 소스 폴더:
  - `/Source/TigerRiceCakeShop/Public/`
  - `/Source/TigerRiceCakeShop/Private/`
- 콘텐츠:
  - `/Content/TRCS/Maps/`
  - `/Content/TRCS/UI/` (WBP 전용)
  - `/Content/TRCS/Art/`, `/Audio/`, `/Data/`

### 2) UI 바인딩 규칙(WBP ↔ C++)
- C++ 위젯 클래스:
  - `UPROPERTY(meta=(BindWidget))`
  - WBP 디자이너의 위젯 이름 = C++ 변수명과 **완전 일치**
- 데이터 갱신:
  - Tick 바인딩 지양
  - C++ Delegate/이벤트로 위젯 갱신 API 호출


## 구현 작업
- 맵: `/Content/TRCS/Maps/Map_TRCS_Main`
- C++ 클래스:
  - `ATRCSGameMode : AGameModeBase`
  - `ATRCSPlayerController : APlayerController`
  - `UTRCSGameInstance : UGameInstance`
- HUD:
  - `UTRCSHUDWidget : UUserWidget`
  - PlayerController BeginPlay에서 `CreateWidget` → `AddToViewport`
- WBP(MCP):
  - `WBP_TRCS_HUD` (부모: `UTRCSHUDWidget`)
  - 구성: TopBar / CenterPanel / CustomerQueuePanel / BottomTabs

## AC
- 실행 시 `WBP_TRCS_HUD` 표시
- 하단 탭 클릭 시 선택 하이라이트 변경
