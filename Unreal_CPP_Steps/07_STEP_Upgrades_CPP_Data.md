# STEP 07 — 업그레이드(데이터 기반) + 즉시 반영(C++)

목표: 데이터 기반 업그레이드 구매로 생산/판매가 즉시 체감된다.

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
- 데이터: `UTRCSUpgradeData : UPrimaryDataAsset` 또는 DataTable
- `FTRCSModifiers` 누적 적용
- `UTRCSUpgradeSubsystem`
  - `TryBuyUpgrade(UpgradeId)`
  - `RecalculateModifiers()`
- UI: `UTRCSUpgradesWidget` + WBP 리스트

## AC
- 데이터만 수정해도 목록/효과/가격 변경 가능
- 최소 2종 업그레이드 체감(탭/자동/가격)
