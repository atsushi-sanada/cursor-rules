# ソースコード作成ルール

Cursorエディタでの開発におけるソースコード作成の統一ルール。可読性・メンテナンス性・拡張性を重視したコーディング規約。

---

## 🎯 コーディング方針

* **誰が見てもわかるコード**
* **メンテナンス性を最重視**

---

## 📋 ルールセット

### 1. 可読性（Readability）

#### 命名規則
* **Microsoft規約に準拠**
  * クラス名：`PascalCase`（例：`PlayerController`）
  * メソッド名：`PascalCase`（例：`GetPlayerHealth`）
  * 変数名：`camelCase`（例：`playerHealth`）
  * 定数名：`UPPER_SNAKE_CASE`（例：`MAX_PLAYER_HEALTH`）
  * プライベート変数：`_camelCase`（例：`_playerHealth`）

#### コメント規約
* **コード冒頭に概要コメント**
  ```csharp
  /// <summary>
  /// プレイヤーの体力管理クラス
  /// 体力の増減、最大値の管理を行う
  /// </summary>
  public class PlayerHealth
  ```

* **関数コメント**
  ```csharp
  /// <summary>
  /// プレイヤーの体力を回復する
  /// </summary>
  /// <param name="amount">回復量</param>
  /// <returns>実際の回復量</returns>
  public int HealPlayer(int amount)
  ```

* **公開変数・定数コメント**
  ```csharp
  /// <summary>
  /// プレイヤーの最大体力
  /// </summary>
  public const int MAX_HEALTH = 100;
  ```

* **複雑ロジック・意図不明箇所の補足コメント**
  ```csharp
  // 体力が0以下の場合、ゲームオーバー状態に移行
  if (currentHealth <= 0)
  {
      GameManager.Instance.SetGameOver();
  }
  ```

* **※変更履歴コメントは不要**（Gitで管理）

---

### 2. 整理・構造（Structure）

#### メソッド・クラス設計
* **1メソッド：20～30行以内**
  ```csharp
  // ✅ 良い例
  public void ProcessPlayerInput()
  {
      if (!IsInputEnabled) return;
      
      var input = GetCurrentInput();
      if (input == null) return;
      
      ApplyInputToPlayer(input);
      UpdateUI();
  }
  ```

* **1クラス：単一責任の原則（SRP）**
  ```csharp
  // ✅ 良い例：体力管理のみに集中
  public class PlayerHealth
  {
      public int CurrentHealth { get; private set; }
      public int MaxHealth { get; }
      
      public void TakeDamage(int damage) { /* ... */ }
      public void Heal(int amount) { /* ... */ }
  }
  
  // ❌ 悪い例：複数の責任を持つ
  public class PlayerController
  {
      public void TakeDamage() { /* ... */ }
      public void UpdateUI() { /* ... */ }
      public void SaveGame() { /* ... */ }
      public void LoadGame() { /* ... */ }
  }
  ```

#### 定数・設定値管理
* **ハードコード禁止**
  ```csharp
  // ✅ 良い例
  public static class GameConstants
  {
      public const int MAX_PLAYER_HEALTH = 100;
      public const float PLAYER_MOVE_SPEED = 5.0f;
      public const string GAME_TITLE = "My Game";
  }
  
  // ScriptableObject使用例
  [CreateAssetMenu(fileName = "GameConfig", menuName = "Game/Config")]
  public class GameConfig : ScriptableObject
  {
      public int maxPlayerHealth = 100;
      public float playerMoveSpeed = 5.0f;
  }
  ```

---

### 3. 拡張性（Extensibility）

#### Magic Number/String禁止
```csharp
// ❌ 悪い例
public void ProcessDamage()
{
    if (health < 0) health = 0;  // Magic Number
    if (damage > 100) damage = 100;  // Magic Number
}

// ✅ 良い例
public void ProcessDamage()
{
    if (health < MIN_HEALTH) health = MIN_HEALTH;
    if (damage > MAX_DAMAGE) damage = MAX_DAMAGE;
}
```

#### SOLID原則の適用
* **Open/Closed原則**
  ```csharp
  // ✅ 良い例：拡張可能な設計
  public interface IWeapon
  {
      void Attack();
  }
  
  public class Sword : IWeapon
  {
      public void Attack() { /* 剣の攻撃 */ }
  }
  
  public class Bow : IWeapon
  {
      public void Attack() { /* 弓の攻撃 */ }
  }
  ```

* **依存性注入（DI）の活用**
  ```csharp
  // ✅ 良い例
  public class PlayerController
  {
      private readonly IWeapon _weapon;
      
      public PlayerController(IWeapon weapon)
      {
          _weapon = weapon;
      }
      
      public void Attack()
      {
          _weapon.Attack();
      }
  }
  ```

#### リファクタリング
* **定期的なリファクタリングを実施**
* **重複コードの排除**
* **長いメソッドの分割**
* **複雑な条件分岐の簡素化**

---

### 4. エラーハンドリング・ログ（Error Handling & Logging）

#### 例外処理
```csharp
// ✅ 良い例
public void LoadPlayerData(string playerId)
{
    try
    {
        if (string.IsNullOrEmpty(playerId))
        {
            throw new ArgumentException("Player ID cannot be null or empty", nameof(playerId));
        }
        
        var data = DataManager.LoadPlayerData(playerId);
        if (data == null)
        {
            UnityEngine.Debug.LogWarning($"Player data not found for ID: {playerId}");
            return;
        }
        
        ApplyPlayerData(data);
    }
    catch (Exception ex)
    {
        UnityEngine.Debug.LogError($"Failed to load player data: {ex.Message}");
        // 適切なエラーハンドリング
    }
}
```

#### ログ出力の使い分け
* **`UnityEngine.Debug.Log`**：通常の情報
  ```csharp
  UnityEngine.Debug.Log($"Player {playerName} joined the game");
  ```

* **`UnityEngine.Debug.LogWarning`**：警告
  ```csharp
  UnityEngine.Debug.LogWarning($"Player health is low: {currentHealth}");
  ```

* **`UnityEngine.Debug.LogError`**：エラー
  ```csharp
  UnityEngine.Debug.LogError($"Failed to save game data: {error.Message}");
  ```

#### ログ出力の原則
* **本質的な情報のみ出力**
* **不要な出力を避ける**
* **パフォーマンスを考慮**

---

## 📝 補足規定

### Debug記述
* **必ず`UnityEngine.Debug`とフルパスで記述**
  ```csharp
  // ✅ 正しい記述
  UnityEngine.Debug.Log("Player moved");
  
  // ❌ 避けるべき記述
  Debug.Log("Player moved");
  ```

### BATファイル作成
* **UTF-8で作成**
* **先頭に文字コード宣言**
  ```batch
  @echo off
  chcp 65001 >nul
  
  echo 処理を開始します
  ```

---

## 🔍 品質チェックリスト

### コードレビュー時の確認項目

- [ ] **命名規則がMicrosoft規約に準拠しているか**
- [ ] **概要コメントが記載されているか**
- [ ] **関数・公開変数・定数にコメントがあるか**
- [ ] **1メソッドが20～30行以内か**
- [ ] **1クラスが単一責任の原則に従っているか**
- [ ] **定数・設定値がハードコードされていないか**
- [ ] **Magic Number/Stringが使用されていないか**
- [ ] **SOLID原則が適用されているか**
- [ ] **適切な例外処理が実装されているか**
- [ ] **ログ出力が適切に使い分けられているか**
- [ ] **`UnityEngine.Debug`がフルパスで記述されているか**

---

## 📚 参考資料

* [Microsoft C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
* [SOLID Principles](https://en.wikipedia.org/wiki/SOLID)
* [Clean Code by Robert C. Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350884) 