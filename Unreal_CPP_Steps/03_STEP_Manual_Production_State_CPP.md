# STEP 03 — 진행도/재고 생성(수동 생산)(C++)

목표: 탭마다 진행도 증가, 100%마다 떡 재고 +1, UI 즉시 갱신.

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
- 상태:
```cpp
USTRUCT()
struct FTRCSResourceState {
  GENERATED_BODY()
  UPROPERTY() int64 Money=0;
  UPROPERTY() int32 Reputation=0;
  UPROPERTY() int32 TteokCount=0;
  UPROPERTY() float MortarProgress=0.f; // 0~100
};
```
- Subsystem/Instance에 `ApplyMashing(float Amount)`
- 이벤트:
  - `OnResourceChanged.Broadcast(State)`
- Widget API:
  - `UTRCSHUDWidget::SetResourceState(const FTRCSResourceState&)`

## AC
- 95 + 10 → 5 이월 등 초과분 처리 OK
- Tick 바인딩 없이 이벤트로 UI 갱신
