# STEP 04 — 판매 버튼으로 돈(냥) 획득(C++)

목표: 판매 버튼 클릭 시 떡 재고 감소 + 돈 증가가 보인다.

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
- 가격: `BaseTteokPrice=5`
- Subsystem 함수: `bool TrySellTteok(int32 Amount)`
- 위젯 바인딩:
  - `UButton* BtnSell` (BindWidget)
  - `OnClicked`에서 `TrySellTteok(1)` 호출

## AC
- 재고>=1: 떡 -1, 돈 +5 즉시 표시
- 재고=0: 불가 피드백
