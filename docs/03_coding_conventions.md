# Proto-TM 코딩 컨벤션

## 1. 명명 규칙

### 1.1 일반 규칙
- 모든 이름은 영문 사용
- 약어 사용 자제 (UI, IO 등 표준 약어 제외)
- 의미 있는 이름 사용
- 한 글자 이름 금지 (루프의 i, j 제외)

### 1.2 클래스/인터페이스
```csharp
public class PlayerManager { }
public interface IDataProvider { }
public struct PlayerStats { }
```

### 1.3 메서드
```csharp
public void UpdatePlayerStats() { }
private bool ValidateInput() { }
protected virtual void OnEnable() { }
```

### 1.4 변수
```csharp
private int _health;
public float MovementSpeed { get; set; }
private const int MAX_PLAYERS = 11;
private static readonly string DEFAULT_NAME = "Player";
```

### 1.5 이벤트/델리게이트
```csharp
public event Action<int> OnHealthChanged;
public delegate void PlayerEventHandler(PlayerData player);
```

## 2. 코드 구조

### 2.1 네임스페이스
```csharp
namespace ProtoTM.Core
namespace ProtoTM.UI
namespace ProtoTM.Data
```

### 2.2 using 문
```csharp
// 시스템
using System;
using System.Collections;
using System.Collections.Generic;

// 유니티
using UnityEngine;
using UnityEngine.UI;

// 프로젝트
using ProtoTM.Core;
using ProtoTM.Data;
```

### 2.3 클래스 구조
```csharp
public class PlayerManager : MonoBehaviour
{
    #region Fields
    [SerializeField] private GameObject playerPrefab;
    private List<PlayerData> players;
    #endregion

    #region Properties
    public int PlayerCount => players.Count;
    #endregion

    #region Unity Lifecycle
    private void Awake() { }
    private void Start() { }
    #endregion

    #region Public Methods
    public void AddPlayer() { }
    #endregion

    #region Private Methods
    private void Initialize() { }
    #endregion

    #region Event Handlers
    private void OnPlayerAdded() { }
    #endregion
}
```

## 3. 코드 스타일

### 3.1 들여쓰기/공백
- 들여쓰기: 4칸 공백
- 중괄호: 새 줄에 배치
- 연산자 주위 공백
- 콤마 뒤 공백

### 3.2 줄 바꿈
- 한 줄당 최대 120자
- 메서드 사이 빈 줄
- 논리적 그룹 사이 빈 줄

### 3.3 주석
```csharp
/// <summary>
/// 선수의 스탯을 업데이트합니다.
/// </summary>
/// <param name="statType">업데이트할 스탯 타입</param>
/// <param name="value">새로운 값</param>
public void UpdateStat(StatType statType, float value)
{
    // 유효성 검사
    if (!ValidateStat(value))
    {
        return;
    }

    // 스탯 업데이트 로직
    stats[statType] = value;
}
```

## 4. 최적화 가이드

### 4.1 메모리 관리
- 오브젝트 풀링 사용
- 불필요한 가비지 생성 방지
- 캐싱 적극 활용

### 4.2 성능 최적화
```csharp
// 권장
private Transform cachedTransform;

private void Awake()
{
    cachedTransform = transform;
}

// 비권장
private void Update()
{
    transform.position += Vector3.up;
}
```

## 5. 에러 처리

### 5.1 예외 처리
```csharp
public PlayerData GetPlayer(string id)
{
    if (string.IsNullOrEmpty(id))
    {
        throw new ArgumentException("Player ID cannot be null or empty");
    }

    try
    {
        return playerDatabase.Find(id);
    }
    catch (DatabaseException ex)
    {
        Debug.LogError($"Failed to find player: {ex.Message}");
        return null;
    }
}
```

### 5.2 로깅
```csharp
// 개발용 로그
Debug.Log("Player created");

// 경고
Debug.LogWarning("Invalid stat value");

// 에러
Debug.LogError("Failed to save player data");
```

## 6. 테스트 코드

### 6.1 단위 테스트
```csharp
[Test]
public void AddPlayer_WithValidData_AddsPlayerToList()
{
    // Arrange
    var manager = new PlayerManager();
    var playerData = new PlayerData("Test");

    // Act
    manager.AddPlayer(playerData);

    // Assert
    Assert.AreEqual(1, manager.PlayerCount);
}
```

### 6.2 통합 테스트
```csharp
[UnityTest]
public IEnumerator PlayerCreation_WithUI_UpdatesDisplay()
{
    // 테스트 설정
    yield return new WaitForSeconds(0.1f);

    // 테스트 실행
    uiManager.CreatePlayer();

    // 결과 확인
    Assert.IsTrue(playerListView.Contains("New Player"));
}
