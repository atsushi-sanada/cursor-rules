# JavaScript コーディング規約

CursorエディタでのJavaScript開発におけるコーディングルール。可読性・保守性・一貫性を重視し、高品質なコードを実現します。

---

## 🎯 コーディング方針
* **可読性**：誰が見ても理解しやすいコード
* **保守性**：変更・拡張が容易な構造
* **一貫性**：チーム全体で統一されたスタイル

---

## 📋 ルールセット

### 1. フォーマット（Formatting）
* インデント：スペース2つ
* 最大行長：80～100文字以内
* クォート：シングルクォート`'`を推奨（必要に応じてテンプレートリテラル）
* セミコロン：**必須**
* 改行コード：LF
* ファイル末尾に改行を挿入
* 自動整形：`prettier --write`
* Linter：`eslint --fix`（`eslint:recommended` または Airbnb スタイルガイド）

### 2. 命名規則（Naming）
* 変数・関数：`camelCase`（例: `getUserData`）
* クラス・コンストラクタ関数：`PascalCase`（例: `UserManager`）
* 定数（値が変わらないもの）：`UPPER_SNAKE_CASE`（例: `API_TIMEOUT`）
* ファイル名：`kebab-case.js`（例: `user-service.js`）
* ディレクトリ名：小文字ハイフン区切り（例: `components`）

### 3. モジュール構造（Modules）
* ES6+ モジュールを使用
  ```js
  // ✅ 良い例
  import { fetchData } from './api-service.js';
  export function init() { /* ... */ }
  ```
* 1ファイル1モジュール／クラス
* 循環依存を避ける

### 4. 変数宣言 & 非同期（Declaration & Async）
* `var`は禁止、必ず`const`／`let`
* 再代入しない変数は`const`
* 非同期処理は`async`/`await`または`Promise`チェーンを使用
  ```js
  async function loadData() {
    try {
      const data = await fetchData();
      return data;
    } catch (err) {
      console.error(err);
      throw err;
    }
  }
  ```

### 5. ドキュメンテーション（Documentation）
* JSDoc形式で関数・クラスにコメント
  ```js
  /**
   * ユーザーデータを取得する
   * @param {string} userId - ユーザーID
   * @returns {Promise<Object>} ユーザーデータ
   */
  async function getUser(userId) { /* ... */ }
  ```
* 複雑ロジックや意図不明箇所は`//`で補足

### 6. エラーハンドリング & ログ（Error & Logging）
* `try`/`catch`で明示的に例外をキャッチ
* ログ出力は`console.log`/`console.warn`/`console.error`を使い分け
* 重要情報以外のデバッグログは`if (process.env.NODE_ENV === 'development')`で制限

### 7. パフォーマンス & ベストプラクティス
* 不要なグローバル変数を避ける
* 配列操作は高階関数（`map`/`filter`/`reduce`）を活用
* 重い処理はWeb Workerまたは非同期に分離
* メモ化やキャッシュを適宜利用

### 8. セキュリティ（Security）
* `eval`禁止
* ユーザー入力のサニタイズ
* フロントエンドではCSP(Content Security Policy)を設定
* HTTPリクエストはHTTPSを使用

### 9. テスト（Testing）
* フレームワーク：`jest` または `mocha` + `chai`
* テストファイル：`__tests__/` または `*.test.js`
* カバレッジ：最低80%
* モック・スタブの利用

### 10. CI / ツール（CI & Tools）
* Lint: `eslint`
* Format: `prettier`
* テスト: `jest`
* CI: GitHub Actions / GitLab CI などで自動チェック

---

## 🔍 品質チェックリスト
- [ ] ESLintエラーがないか
- [ ] Prettierでフォーマット済みか
- [ ] JSDocコメントが適切か
- [ ] 変数宣言が`const`/`let`で正しく行われているか
- [ ] モジュール間の依存が適切か
- [ ] 非同期処理でawaitが正しく使われているか
- [ ] セキュリティリスクがないか（eval等）
- [ ] テストが十分にカバレッジを確保しているか

---

## 📚 参考資料
* Airbnb JavaScript Style Guide: https://github.com/airbnb/javascript
* ESLint: https://eslint.org/
* Prettier: https://prettier.io/
* MDN Web Docs: https://developer.mozilla.org/ja/ 