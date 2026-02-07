# Tiger Rice Cake Shop (호랑이 떡집) — Unreal Engine C++ 단계별 구현 지시서

- 엔진: Unreal Engine 5.x
- 구현 방식: **C++ 클래스 구현 위주**
- UI: **UMG 위젯은 WBP(Widget Blueprint)로 MCP 파이프라인을 통해 제작**
  - C++는 **Widget 클래스(UUserWidget 파생)** + **바인딩 변수(BindWidget/BindWidgetOptional)** + **이벤트/데이터 갱신 API** 제공
  - WBP는 디자이너가 만든 위젯 트리에서 **변수 이름을 C++와 동일하게 맞춰 바인딩**
- 목표: 매 단계마다 “눈으로 확인 가능한 결과”가 나오도록 점진 구현

> 포함 파일: README + 단계별 md 10개
