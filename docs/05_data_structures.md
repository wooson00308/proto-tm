# Proto-TM 데이터 구조

## 1. 데이터 모델

### 1.1 선수 데이터 (PlayerData)

```mermaid
classDiagram
    class PlayerData {
        +string Id
        +string Name
        +int Age
        +Position Position
        +Dictionary~StatType, float~ Stats
        +List~Trait~ Traits
        +DateTime LastTrainingDate
        +int ContractYears
        +float MarketValue
    }
    
    class Position {
        <<enumeration>>
        Forward
        Midfielder
        Defender
        Goalkeeper
    }
    
    class StatType {
        <<enumeration>>
        Speed
        Strength
        Stamina
        Technique
        Intelligence
    }
    
    class Trait {
        +string Id
        +string Name
        +TraitType Type
        +float Impact
    }
    
    PlayerData --> Position
    PlayerData --> StatType
    PlayerData --> Trait
```

### 1.2 게임 데이터 (GameData)

```mermaid
classDiagram
    class GameData {
        +string SaveName
        +DateTime LastSaved
        +List~PlayerData~ Players
        +GameSettings Settings
        +TeamData Team
        +List~MatchResult~ History
    }
    
    class GameSettings {
        +float DifficultyLevel
        +bool AutoSave
        +Language Language
        +Dictionary~string, bool~ Toggles
    }
    
    class TeamData {
        +string Name
        +float Budget
        +int Reputation
        +List~string~ Achievements
        +Dictionary~FacilityType, int~ Facilities
    }
    
    class MatchResult {
        +DateTime Date
        +string OpponentName
        +int HomeScore
        +int AwayScore
        +List~PlayerPerformance~ Performances
    }
    
    GameData --> GameSettings
    GameData --> TeamData
    GameData --> MatchResult
```

## 2. 데이터 저장 구조

### 2.1 파일 구조

```
SaveData/
├── profiles/
│   ├── profile1/
│   │   ├── gamedata.json
│   │   ├── players.json
│   │   ├── team.json
│   │   └── history.json
│   └── profile2/
│       └── ...
├── settings.json
└── metadata.json
```

### 2.2 JSON 구조

#### gamedata.json
```json
{
    "saveName": "My Career",
    "lastSaved": "2024-03-20T15:30:00Z",
    "settings": {
        "difficultyLevel": 0.75,
        "autoSave": true,
        "language": "ko_KR",
        "toggles": {
            "tutorialEnabled": false,
            "autoRotation": true
        }
    }
}
```

#### players.json
```json
{
    "players": [
        {
            "id": "p001",
            "name": "John Doe",
            "age": 23,
            "position": "Forward",
            "stats": {
                "speed": 85,
                "strength": 70,
                "stamina": 75,
                "technique": 80,
                "intelligence": 65
            },
            "traits": [
                {
                    "id": "t001",
                    "name": "Quick Starter",
                    "type": "Passive",
                    "impact": 1.2
                }
            ],
            "lastTrainingDate": "2024-03-19T10:00:00Z",
            "contractYears": 3,
            "marketValue": 1500000
        }
    ]
}
```

#### team.json
```json
{
    "name": "Proto United",
    "budget": 10000000,
    "reputation": 75,
    "achievements": [
        "League Winner 2023",
        "Cup Runner-up 2024"
    ],
    "facilities": {
        "training": 3,
        "youth": 2,
        "medical": 4
    }
}
```

## 3. 데이터 관계

```mermaid
erDiagram
    GAME_DATA ||--o{ PLAYER_DATA : contains
    GAME_DATA ||--|| TEAM_DATA : has
    GAME_DATA ||--o{ MATCH_RESULT : records
    PLAYER_DATA ||--o{ TRAIT : has
    PLAYER_DATA ||--o{ STAT : has
    MATCH_RESULT ||--o{ PLAYER_PERFORMANCE : includes
    TEAM_DATA ||--o{ FACILITY : has
```

## 4. 데이터 타입 정의

### 4.1 기본 타입

```csharp
// 선수 포지션
public enum Position
{
    Forward,
    Midfielder,
    Defender,
    Goalkeeper
}

// 스탯 타입
public enum StatType
{
    Speed,
    Strength,
    Stamina,
    Technique,
    Intelligence
}

// 특성 타입
public enum TraitType
{
    Passive,
    Active,
    Conditional
}

// 시설 타입
public enum FacilityType
{
    Training,
    Youth,
    Medical,
    Commercial
}
```

### 4.2 복합 타입

```csharp
// 선수 퍼포먼스 데이터
public class PlayerPerformance
{
    public string PlayerId { get; set; }
    public float Rating { get; set; }
    public int MinutesPlayed { get; set; }
    public Dictionary<string, int> Stats { get; set; }
}

// 시설 데이터
public class Facility
{
    public FacilityType Type { get; set; }
    public int Level { get; set; }
    public float Efficiency { get; set; }
    public DateTime LastUpgrade { get; set; }
}
```

## 5. 데이터 변환 및 직렬화

### 5.1 JSON 직렬화 설정

```csharp
public class JsonConverterSettings
{
    public static JsonSerializerSettings Default => new JsonSerializerSettings
    {
        Formatting = Formatting.Indented,
        ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
        DateTimeZoneHandling = DateTimeZoneHandling.Utc,
        Converters = new List<JsonConverter>
        {
            new StringEnumConverter(),
            new IsoDateTimeConverter()
        }
    };
}
```

### 5.2 데이터 변환 예시

```csharp
// 데이터 저장
public void SavePlayerData(PlayerData player)
{
    string json = JsonConvert.SerializeObject(player, JsonConverterSettings.Default);
    string path = $"SaveData/profiles/{CurrentProfile}/players/{player.Id}.json";
    File.WriteAllText(path, json);
}

// 데이터 로드
public PlayerData LoadPlayerData(string playerId)
{
    string path = $"SaveData/profiles/{CurrentProfile}/players/{playerId}.json";
    string json = File.ReadAllText(path);
    return JsonConvert.DeserializeObject<PlayerData>(json, JsonConverterSettings.Default);
}
```
