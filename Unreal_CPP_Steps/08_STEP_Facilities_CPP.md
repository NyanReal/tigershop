# STEP 08 — 시설 해금/설치 1~2개(C++)

목표: 시설 설치 시 월드에 오브젝트가 생기고 수익 변화가 보인다.

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
- 데이터: `UTRCSFacilityData : UPrimaryDataAsset`
- `UTRCSFacilitySubsystem`
  - 설치 상태 저장
  - 앵커 슬롯에 Actor 스폰
- `ATRCSFacilityAnchors` (레벨 배치)
- UI: `UTRCSFacilitiesWidget` + WBP

## AC
- 설치 시 월드에 시설이 보임
- 설치 효과로 돈 벌이 변화 체감
