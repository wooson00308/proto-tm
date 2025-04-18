# Proto-TM 아키텍처 문서

## 1. 시스템 아키텍처

### 1.1 전체 구조

```mermaid
graph TB
    subgraph Core[Core Systems]
        IM[InputManager]
        UM[UIManager]
        PM[PlayerManager]
        DM[DataManager]
    end

    subgraph Data[Data Layer]
        PD[PlayerData]
        PS[Position & Stats]
        TR[Traits]
        SD[SaveData]
    end

    subgraph UI[UI Layer]
        PLV[PlayerListView]
        PDV[PlayerDetailView]
        TRV[TrainingView]
    end

    IM --> UM
    UM --> PLV
    UM --> PDV
    UM --> TRV
    PM --> PD
    PD --> PS
    PD --> TR
    PLV --> PM
    PDV --> PM
    DM --> SD
```

### 1.2 핵심 컴포넌트

#### 1.2.1 UIManager
- UI Document 관리
- 화면 전환 처리
- UI 이벤트 라우팅
- 레이아웃 관리

#### 1.2.2 InputManager
- 터치 입력 처리
- UI 네비게이션
- 입력 이벤트 발행
- 입력 상태 관리

#### 1.2.3 PlayerManager
- 선수 데이터 CRUD
- 선수 목록 관리
- 스탯 계산
- 성장 시스템

#### 1.2.4 DataManager
- 데이터 저장/로드
- 데이터 캐싱
- 데이터 마이그레이션

### 1.3 데이터 흐름

```mermaid
sequenceDiagram
    participant User
    participant Input as InputManager
    participant UI as UIManager
    participant View as UIViews
    participant Logic as PlayerManager
    participant Data as DataManager
    
    User->>Input: 사용자 입력
    Input->>UI: 입력 이벤트
    UI->>View: UI 업데이트 요청
    View->>Logic: 데이터 요청
    Logic->>Data: 데이터 로드/저장
    Data-->>Logic: 데이터 반환
    Logic-->>View: 데이터 전달
    View-->>UI: UI 갱신
    UI-->>User: 화면 표시
```

## 2. 모듈 구조

### 2.1 Scripts 디렉토리 구조
```
Assets/
└── Scripts/
    ├── Core/
    │   ├── Managers/
    │   │   ├── UIManager.cs
    │   │   ├── InputManager.cs
    │   │   ├── PlayerManager.cs
    │   │   └── DataManager.cs
    │   └── Systems/
    │       ├── SaveSystem.cs
    │       └── EventSystem.cs
    ├── Data/
    │   ├── Models/
    │   │   ├── PlayerData.cs
    │   │   ├── Stats.cs
    │   │   └── Traits.cs
    │   └── ScriptableObjects/
    │       ├── PlayerConfig.cs
    │       └── GameConfig.cs
    ├── UI/
    │   ├── Views/
    │   │   ├── PlayerListView.cs
    │   │   └── PlayerDetailView.cs
    │   └── Components/
    │       ├── StatDisplay.cs
    │       └── TraitIcon.cs
    └── Utils/
        ├── Extensions/
        └── Helpers/
```

### 2.2 리소스 구조
```
Assets/
└── Resources/
    ├── UI/
    │   ├── USS/
    │   └── UXML/
    ├── Data/
    │   └── Configs/
    └── Prefabs/
        └── UI/
```

## 3. 확장성 고려사항

### 3.1 모듈 확장
- 새로운 매니저 추가 시 IManager 인터페이스 구현
- UI 컴포넌트는 BaseView 상속
- 데이터 모델은 IData 인터페이스 구현

### 3.2 성능 최적화
- Object Pooling 사용
- 데이터 캐싱 전략
- UI 업데이트 최적화

### 3.3 테스트 전략
- Unit Tests
- Integration Tests
- UI Tests
