# Unity C# コーディング規約

CursorエディタでのUnity開発におけるC#コード作成ルール。可読性・保守性・パフォーマンス・拡張性を重視し、一貫した品質を実現します。

---

## コーディング方針
* **誰が見てもわかるコード**
* **単一責任・疎結合を意識（SRP, DI）**
* **実行時パフォーマンスを常に考慮**

---

## 命名規則
* クラス/構造体/enum: **PascalCase**（例: `PlayerController`）
* メソッド/プロパティ: **PascalCase**（例: `GetHealth()`）
* public フィールド: **PascalCase**（※Inspector公開は `[SerializeField] private` を推奨）
* private フィールド: `_camelCase`（例: `_health`）
* 定数: **PascalCase** or **UPPER_SNAKE_CASE**（例: `MaxHealth` / `MAX_HEALTH`）
* 名前空間: **PascalCase**（例: `MyGame.Systems`）
* ファイル名: クラス名と **完全一致**

---

## フォルダ構成
* Assets/Scripts/
  * 機能別フォルダ：`Player/`, `UI/`, `Managers/` など
* Assets/Scenes/：シーンファイル
* Assets/Prefabs/：プレハブ
* Assets/Resources/ または Assets/Addressables/：動的ロード用アセット

---

## MonoBehaviour 設計
* **ライフサイクル順のメソッド記述**
  1. [Header]/[SerializeField]付きフィールド
  2. public プロパティ
  3. private フィールド
  4. Awake(), OnEnable(), Start(), Update(), FixedUpdate(), LateUpdate(), OnDisable(), OnDestroy()
* **空のコールバックは削除**
* **GetComponentは Awake/Start でキャッシュ**
* **RequireComponent/DisallowMultipleComponent** の活用

```csharp
[RequireComponent(typeof(Rigidbody))]
public class PlayerController : MonoBehaviour
{
    [SerializeField] private float _moveSpeed = 5f;
    private Rigidbody _rigidbody;

    private void Awake()
    {
        _rigidbody = GetComponent<Rigidbody>();
    }

    private void Update()
    {
        Move();
    }

    private void Move()
    {
        Vector3 dir = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));
        _rigidbody.MovePosition(transform.position + dir * _moveSpeed * Time.deltaTime);
    }
}
```

---

## データ管理
* **設定値は ScriptableObject** を活用し、Inspector から変更可能に
* **ハードコード禁止**：`const`/`readonly`/ScriptableObject/Config ファイル 等で管理
* **Resources.Load の乱用禁止**：Addressables を推奨

```csharp
[CreateAssetMenu(menuName = "Config/GameConfig")]
public class GameConfig : ScriptableObject
{
    public int maxPlayerHealth = 100;
    public float jumpForce = 5f;
}
```

---

## パフォーマンス最適化
* **GC Alloc 回避**：毎フレームの配列・List生成を避ける
* **foreach** は注意、`for` か事前キャッシュを活用
* **Update 内は軽量処理のみ**、重い処理は Coroutine へ
* **Physics系処理は FixedUpdate で実行**
* **Profiler／ProfilerMarker** でボトルネックを特定

```csharp
private static readonly ProfilerMarker _moveMarker = new ProfilerMarker("PlayerController.Move");

private void Move()
{
    using (_moveMarker.Auto())
    {
        // 移動処理
    }
}
```

---

## UI 開発ガイド
* **View/Controller 分離**：UIスクリプトは機能単位で分割
* **UI要素は SerializeField で参照**
* **イベントハンドラは UnityEvent またはデリゲート**

---

## エラーハンドリング・ログ
* **Debug.Log 系は開発時のみ**：`#if UNITY_EDITOR` で包む
* **常に `UnityEngine.Debug` フルパス**
* **ログレベルを使い分け**：`Log`/`LogWarning`/`LogError`

```csharp
#if UNITY_EDITOR
UnityEngine.Debug.Log("Editor only log");
#endif
```

---

## 品質チェックリスト
- [ ] 名前空間が適切に設定されているか
- [ ] SerializeField 付き private フィールドが正しく使われているか
- [ ] 未使用のライフサイクルメソッドが残っていないか
- [ ] Data 設定値が ScriptableObject で管理されているか
- [ ] GC Alloc 発生箇所がないか（プロファイラ確認）
- [ ] RequireComponent などの属性が適切に設定されているか
- [ ] アセットロード方式（Addressables/Resources）の使用が適切か
- [ ] ログ出力が本番ビルドで制限されているか

---
