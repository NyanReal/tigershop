# STEP 10 — 저장/로드 + 첫 10분 완주(C++)

목표: 재실행 후에도 진행 유지 + 첫 10분 동선 완주 가능.

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
- `UTRCSSaveGame : USaveGame`
  - Resource / UpgradeLevels / InstalledFacilities / EquippedDecor
- `UTRCSSaveSubsystem`
  - LoadOrCreate / RequestSave / SaveNow
- 간단 가이드 위젯(퀘스트 텍스트)

## AC
- 재실행 후 돈/재고/업그레이드/시설/꾸미기 유지
- 10분 내: 호랑이 고용 + 업글 2 + 시설 1 + 꾸미기 1 달성 가능
