# STEP 09 — 꾸미기(6슬롯) 즉시 적용(C++ + WBP)

목표: 꾸미기 선택 즉시 배경/간판/등불 등이 바뀐다(하우징 없음).

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
- 슬롯:
```cpp
UENUM() enum class ETRCSDecorSlot: uint8 { Signboard, LanternLeft, LanternRight, Background, Floor, ShowcaseSkin };
```
- 데이터: `UTRCSDecorData : UPrimaryDataAsset`
- `UTRCSDecorSubsystem`
  - `TryBuyAndEquip(DecorId)`
  - `ApplyEquippedToWorld()`
- `ATRCSDecorAnchors`가 대상 컴포넌트 참조 제공
- UI: `UTRCSDecorWidget` + WBP 그리드

## AC
- 최소 2종(배경/간판/등불)에서 즉시 변화 확인
