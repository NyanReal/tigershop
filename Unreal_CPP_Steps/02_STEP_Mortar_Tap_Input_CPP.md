# STEP 02 — 절구 탭 입력 + 반응 연출(C++)

목표: 절구 클릭/터치 시 눌림 연출 + SFX/VFX 재생.

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
- `ATRCMortarActor : AActor`
  - `UStaticMeshComponent* MortarMesh`
- 입력:
  - Enhanced Input `IA_Tap`
  - `GetHitResultUnderCursorByChannel`/`GetHitResultUnderFingerByChannel`
- 연출:
  - 스케일 인터폴레이션(간단) 또는 머티리얼 파라미터
  - Niagara/SFX 플레이스홀더

## AC
- 절구 탭 시 항상 연출이 보임
- 절구 외 탭 시 무반응
