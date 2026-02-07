# STEP 05 — 손님 스폰/주문/자동 구매(C++)

목표: 손님 큐가 생기고 자동 구매로 돈/명성이 오르는 것이 보인다.

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
- 타입/주문:
```cpp
UENUM() enum class ETRCSCustomerType: uint8 { AhnNae, SeonBi, NaGeunNe, Merchant };
USTRUCT() struct FTRCSCustomerOrder {
  GENERATED_BODY()
  UPROPERTY() ETRCSCustomerType Type;
  UPROPERTY() int32 Amount=1;
  UPROPERTY() FName RequestedItemId;
};
```
- `UTRCSCustomerSubsystem : UWorldSubsystem`
  - 스폰 타이머, 최대 큐(예 3)
  - 1초마다 첫 주문 처리(재고 충분 시 구매/제거)
- UI:
  - `UTRCSCustomerQueueWidget` + WBP 카드 리스트

## AC
- 큐가 주기적으로 증가/감소하며 UI에 보임
- 구매 시 돈/명성이 즉시 증가
