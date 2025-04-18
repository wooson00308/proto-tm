# Proto-TM (Proto Team Manager)

Unity로 개발되는 팀 매니저 스타일의 프로토타입 게임입니다.

## 아키텍처 다이어그램

```mermaid
classDiagram
    class UIManager {
        - Dictionary<string, UIDocument> uiDocuments
        + ShowUI(string uiName)
        + HideUI(string uiName)
        + UpdateUI(string uiName)
    }

    class InputManager {
        - PlayerInputActions inputActions
        + OnUINavigate
        + OnUISubmit
        + OnUICancel
        + OnTouchPrimary
    }

    class PlayerManager {
        - List<PlayerData> players
        + CreatePlayer()
        + UpdatePlayer()
        + GetPlayerList()
        + GetPlayerDetails()
    }

    class PlayerData {
        + string Id
        + string Name
        + Position Position
        + Stats Stats
        + List<Trait> Traits
    }

    class UIViews {
        <<interface>>
        + Initialize()
        + Show()
        + Hide()
        + Update()
    }

    class PlayerListView {
        - ScrollView playerList
        - Button addButton
        + OnPlayerSelected()
        + UpdatePlayerList()
    }

    class PlayerDetailView {
        - VisualElement statsContainer
        - Button trainButton
        + UpdatePlayerDetails()
        + OnTrainButtonClicked()
    }

    UIManager --> PlayerListView
    UIManager --> PlayerDetailView
    UIManager --> InputManager
    PlayerListView --|> UIViews
    PlayerDetailView --|> UIViews
    PlayerManager --> PlayerData
    PlayerListView --> PlayerManager
    PlayerDetailView --> PlayerManager
```

## 시스템 구조도

```mermaid
graph TB
    subgraph Core[Core Systems]
        IM[InputManager]
        UM[UIManager]
        PM[PlayerManager]
    end

    subgraph Data[Data Layer]
        PD[PlayerData]
        PS[Position & Stats]
        TR[Traits]
    end

    subgraph UI[UI Layer]
        PLV[PlayerListView]
        PDV[PlayerDetailView]
    end

    IM --> UM
    UM --> PLV
    UM --> PDV
    PM --> PD
    PD --> PS
    PD --> TR
    PLV --> PM
    PDV --> PM
```

## 데이터 흐름도

```mermaid
sequenceDiagram
    participant User
    participant Input as InputManager
    participant UI as UIManager
    participant View as PlayerListView/DetailView
    participant Data as PlayerManager
    
    User->>Input: 터치/키보드 입력
    Input->>UI: 입력 이벤트 전달
    UI->>View: UI 업데이트 요청
    View->>Data: 데이터 요청
    Data-->>View: 데이터 전달
    View-->>UI: UI 갱신
    UI-->>User: 화면 표시
```

## 프로젝트 구조

### 핵심 매니저 클래스

#### UIManager
- UI 문서들을 관리하는 중앙 시스템
- UI 표시/숨김 및 업데이트 기능 제공
- UI Document 기반의 런타임 UI 관리

#### InputManager
- 새로운 Input System을 활용한 입력 처리
- UI 네비게이션 및 터치 입력 처리
- 이벤트 기반 입력 시스템 구현

#### PlayerManager
- 선수 데이터 관리 및 CRUD 작업 처리
- 선수 목록 및 상세 정보 제공
- 메모리 최적화를 위한 데이터 캐싱

### 데이터 구조

#### PlayerData
- 선수의 기본 정보 (ID, 이름)
- 포지션 정보
- 스탯 정보
- 특성(Trait) 목록

### UI 뷰

#### UIViews (Interface)
모든 UI 뷰의 기본 인터페이스:
- Initialize(): 뷰 초기화
- Show(): 뷰 표시
- Hide(): 뷰 숨김
- Update(): 뷰 업데이트

#### PlayerListView
- 선수 목록을 스크롤 뷰로 표시
- 새로운 선수 추가 기능
- 선수 선택 이벤트 처리

#### PlayerDetailView
- 선수 상세 정보 표시
- 스탯 컨테이너를 통한 정보 시각화
- 훈련 기능 구현

## 기술 스택
- Unity 2022.3 LTS
- UI Toolkit (UI Builder)
- New Input System
- C# (.NET Standard 2.1)

## 개발 가이드라인
1. 모든 UI는 UI Toolkit을 사용하여 구현
2. 이벤트 기반 아키텍처 사용
3. 데이터 변경은 반드시 해당 Manager를 통해서만 수행
4. SOLID 원칙을 준수한 설계
5. 확장 가능한 구조 유지

## 설치 및 실행
1. Unity Hub에서 프로젝트 열기
2. Unity 2022.3 LTS 버전 사용
3. 필요한 패키지 자동 설치
4. 실행 및 테스트

## 라이선스
MIT License
