# STEP 06 — 호랑이 고용/자동 생산(C++)

목표: 호랑이 고용 후 탭 없이 떡이 자동 생성된다(연출 포함).

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
- `ATRCSTigerWorkerActor`
  - Timer로 `DoWork()` 반복
  - `DoWork()` → `ApplyMashing(AutoTapPower)`
- 고용:
  - `bool TryHireTiger(int32 Cost)`
  - UI `BtnHireTiger` 클릭 연결
- 연출:
  - 자동 작업 시에도 절구 반응 호출

## AC
- 호랑이 고용 후 방치해도 재고 증가
- 자동 작업 연출이 눈에 보임
