# Proto-TM API 문서

## 1. Core API

### 1.1 PlayerManager

#### Methods

```csharp
/// <summary>
/// 새로운 선수를 생성합니다.
/// </summary>
/// <param name="name">선수 이름</param>
/// <returns>생성된 선수 데이터</returns>
public PlayerData CreatePlayer(string name)

/// <summary>
/// 선수를 삭제합니다.
/// </summary>
/// <param name="playerId">선수 ID</param>
/// <returns>삭제 성공 여부</returns>
public bool RemovePlayer(string playerId)

/// <summary>
/// 선수 정보를 업데이트합니다.
/// </summary>
/// <param name="playerData">업데이트할 선수 데이터</param>
public void UpdatePlayer(PlayerData playerData)

/// <summary>
/// 모든 선수 목록을 반환합니다.
/// </summary>
/// <returns>선수 목록</returns>
public List<PlayerData> GetAllPlayers()
```

#### Events

```csharp
// 선수가 추가될 때 발생하는 이벤트
public event Action<PlayerData> OnPlayerAdded;

// 선수가 제거될 때 발생하는 이벤트
public event Action<string> OnPlayerRemoved;

// 선수 정보가 업데이트될 때 발생하는 이벤트
public event Action<PlayerData> OnPlayerUpdated;
```

### 1.2 DataManager

#### Methods

```csharp
/// <summary>
/// 게임 데이터를 저장합니다.
/// </summary>
/// <returns>저장 성공 여부</returns>
public bool SaveGameData()

/// <summary>
/// 게임 데이터를 로드합니다.
/// </summary>
/// <returns>로드된 게임 데이터</returns>
public GameData LoadGameData()

/// <summary>
/// 특정 선수의 데이터를 저장합니다.
/// </summary>
/// <param name="playerData">저장할 선수 데이터</param>
/// <returns>저장 성공 여부</returns>
public bool SavePlayerData(PlayerData playerData)
```

## 2. UI API

### 2.1 UIManager

#### Methods

```csharp
/// <summary>
/// UI 화면을 전환합니다.
/// </summary>
/// <param name="screenType">전환할 화면 타입</param>
public void SwitchScreen(ScreenType screenType)

/// <summary>
/// 팝업을 표시합니다.
/// </summary>
/// <param name="popupType">팝업 타입</param>
/// <param name="data">팝업에 표시할 데이터</param>
public void ShowPopup(PopupType popupType, object data = null)

/// <summary>
/// 현재 표시된 팝업을 닫습니다.
/// </summary>
public void ClosePopup()
```

### 2.2 PlayerListView

#### Methods

```csharp
/// <summary>
/// 선수 목록을 업데이트합니다.
/// </summary>
/// <param name="players">업데이트할 선수 목록</param>
public void UpdatePlayerList(List<PlayerData> players)

/// <summary>
/// 선수 항목을 선택합니다.
/// </summary>
/// <param name="playerId">선택할 선수 ID</param>
public void SelectPlayer(string playerId)
```

## 3. Data Structures

### 3.1 PlayerData

```csharp
public class PlayerData
{
    // 기본 정보
    public string Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
    public Position Position { get; set; }

    // 스탯 정보
    public Dictionary<StatType, float> Stats { get; set; }
    
    // 특성 정보
    public List<Trait> Traits { get; set; }
}
```

### 3.2 GameData

```csharp
public class GameData
{
    // 게임 기본 정보
    public string SaveName { get; set; }
    public DateTime LastSaved { get; set; }
    
    // 선수 데이터
    public List<PlayerData> Players { get; set; }
    
    // 게임 설정
    public GameSettings Settings { get; set; }
}
```

## 4. Enums

### 4.1 ScreenType

```csharp
public enum ScreenType
{
    MainMenu,
    PlayerList,
    PlayerDetail,
    Training,
    Match,
    Settings
}
```

### 4.2 StatType

```csharp
public enum StatType
{
    Speed,
    Strength,
    Stamina,
    Technique,
    Intelligence
}
```

### 4.3 Position

```csharp
public enum Position
{
    Forward,
    Midfielder,
    Defender,
    Goalkeeper
}
```

## 5. 사용 예시

### 5.1 선수 생성 및 관리

```csharp
// 선수 매니저 참조
PlayerManager playerManager = GetComponent<PlayerManager>();

// 새로운 선수 생성
PlayerData newPlayer = playerManager.CreatePlayer("John Doe");

// 선수 스탯 업데이트
newPlayer.Stats[StatType.Speed] = 80;
newPlayer.Stats[StatType.Strength] = 75;
playerManager.UpdatePlayer(newPlayer);

// 선수 제거
playerManager.RemovePlayer(newPlayer.Id);
```

### 5.2 UI 조작

```csharp
// UI 매니저 참조
UIManager uiManager = GetComponent<UIManager>();

// 선수 목록 화면으로 전환
uiManager.SwitchScreen(ScreenType.PlayerList);

// 선수 상세 정보 팝업 표시
uiManager.ShowPopup(PopupType.PlayerDetail, selectedPlayer);
```

### 5.3 데이터 저장/로드

```csharp
// 데이터 매니저 참조
DataManager dataManager = GetComponent<DataManager>();

// 게임 데이터 저장
bool saveSuccess = dataManager.SaveGameData();

// 게임 데이터 로드
GameData loadedData = dataManager.LoadGameData();
```
