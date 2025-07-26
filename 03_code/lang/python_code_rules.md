# Pythonコーディング規約

CursorエディタでのPython開発におけるコーディングルール。可読性・保守性・一貫性を重視し、チーム全体で統一した品質を実現します。

---

## 🎯 コーディング方針

* **可読性**：他者が容易に理解できるコード
* **保守性**：変更・拡張がしやすい構造
* **一貫性**：チーム全体で統一されたスタイル

---

## 📋 ルールセット

### 1. フォーマット（Formatting）

* インデント: スペース4つ
* 最大行長: 79文字以内（80～100文字も許容）
* 改行コード: LF
* ファイル末尾に改行を挿入
* 空行: 関数／クラス定義前に2行、メソッド内は1行
* 自動整形: `black` を導入しコードをフォーマット
* Import順序: 標準 → サードパーティ → ローカル、各グループ間に空行（`isort`推奨）

### 2. 命名規則（Naming）

* 変数・関数: `snake_case`（例: `get_player_health`）
* クラス: `PascalCase`（例: `PlayerController`）
* 定数: `UPPER_SNAKE_CASE`（例: `MAX_PLAYER_HEALTH`）
* モジュール（ファイル名）: `snake_case.py`（例: `game_utils.py`）
* パッケージ（ディレクトリ）: `lowercase`（例: `services`）

### 3. ドキュメンテーション（Documentation）

* モジュール／クラス／関数には必ずDocstringを記述
* 形式: GoogleスタイルまたはNumPyスタイル（プロジェクトで統一）

```python
def heal_player(amount: int) -> int:
    """
    プレイヤーの体力を回復する

    Args:
        amount (int): 回復量

    Returns:
        int: 実際に回復した量
    """
    ...
```

* コメント: 複雑ロジックや意図不明箇所に限定して`#`で記述

### 4. 型ヒント（Type Hints）

* 関数／メソッドには可能な限り型ヒントを付与（PEP484）
* 静的型チェック: `mypy` を導入

### 5. エラーハンドリング & ログ（Error Handling & Logging）

* 例外捕捉時は具体的な例外クラスを指定
* ワイルドカード`except:`は禁止
* ログ出力: 標準`logging`モジュールを使用
  ```python
  import logging
  logger = logging.getLogger(__name__)

  logger.info("処理開始")
  logger.warning("低メモリ")
  logger.error("読み込み失敗")
  ```

### 6. テスト（Testing）

* テストフレームワーク: `pytest` を推奨
* テスト配置: `tests/`ディレクトリ
* テストファイル名: `test_*.py`
* テスト関数名: `test_*` 形式
* カバレッジ: 最低80%以上を目標

### 7. 依存管理（Dependency Management）

* 仮想環境: `venv`、`pipenv`、`poetry` などを使用
* 依存定義: `requirements.txt` または `poetry.lock`
* バージョン固定: `package==version` 形式

### 8. CI / ツール（CI & Tools）

* Linter: `flake8`
* Formatter: `black`
* Import Sorter: `isort`
* 型チェック: `mypy`

---

## 🔍 品質チェックリスト

- [ ] PEP8に準拠しているか（`flake8`エラーなし）
- [ ] `black`でフォーマット済みか
- [ ] `isort`でImport順序が正しいか
- [ ] 型ヒントが適切に記述されているか
- [ ] Docstringが記載されているか
- [ ] 例外処理が適切に実装されているか
- [ ] ログレベルが適切に使い分けられているか
- [ ] テストが十分にカバレッジを確保しているか
- [ ] 依存定義が正しく管理されているか

---

## 📚 参考資料

* PEP8 ドキュメント: https://pep8-ja.readthedocs.io/
* Black: https://black.readthedocs.io/
* isort: https://pycqa.github.io/isort/
* mypy: http://mypy-lang.org/
* pytest: https://docs.pytest.org/ 