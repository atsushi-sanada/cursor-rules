# GAS（Google Apps Script）コーディング規約

CursorエディタでのGoogle Apps Script開発におけるコーディングルール。可読性・保守性・パフォーマンス・セキュリティを重視し、一貫した品質を実現します。

---

## 🎯 コーディング方針
* **関数単位で役割を明確化**
* **再利用性・変更容易性を最重視**
* **最小限の権限で実行**

---

## 📋 ルールセット

### 1. フォーマット（Formatting）
* インデント：スペース2つ
* 最大行長：100文字以内
* 改行：LF
* ファイル末尾：必ず改行を挿入
* 自動整形：Prettier (`prettier --write`) を導入
* Linter: ESLint (`eslint --fix`) を導入

### 2. 命名規則（Naming）
* 関数・変数：`camelCase`（例: `getSheetData`）
* 定数：`UPPER_SNAKE_CASE`（例: `DEFAULT_TIMEOUT`）
* クラス（ES6+利用時）：`PascalCase`（例: `OrderProcessor`）
* ファイル名：`kebab-case.gs/js`（例: `sheet-utils.gs`）

### 3. ドキュメンテーション（Documentation）
* JSDoc形式でコメントを記述
* ファイル冒頭にモジュール概要
* 関数には必ず `@param` / `@return` を記載

```javascript
/**
 * スプレッドシートから値を取得する
 * @param {string} spreadsheetId - スプレッドシートID
 * @return {GoogleAppsScript.Spreadsheet.Sheet} シートオブジェクト
 */
function getSheet(spreadsheetId) {
  // ...
}
```

* 複雑ロジックには`//`コメントで補足

### 4. 変数宣言（Variable Declaration）
* `var`禁止、必ず`const` / `let`を使用
* 再代入不要なものは`const`、変更する場合は`let`

### 5. エラーハンドリング & ログ
* `try`/`catch`で例外を明示的にキャッチ
* ログ出力は`Logger.log` / `Logger.warn` / `Logger.error`を使い分け

```javascript
try {
  // 処理
} catch (e) {
  Logger.error(`Error in processData: ${e}`);
  throw e;
}
```

### 6. スクリプト構造 & トリガー
* エントリポイント（`doGet` / `doPost`）は必ず明示
* ビジネスロジックは別関数に分離

```javascript
function doGet(e) {
  return HtmlService.createTemplateFromFile('Index').evaluate();
}
function processForm(formData) {
  // ...
}
```

### 7. パフォーマンス & クォータ対策
* バッチ処理を活用（`getValues` / `setValues`）
* API呼び出しをまとめる
* キャッシュサービス(`CacheService`)の利用

### 8. セキュリティ
* XSS対策に`HtmlService.createTemplateFromFile`を利用
* 機密情報はスクリプトプロパティまたはプロジェクトプロパティで管理
* ハードコード禁止

### 9. テスト（Testing）
* clasp + `gas-local` / `jest`等でユニットテストを実装
* テストコードは`test/`ディレクトリに配置
* ファイル名: `*.test.js`

---

## 🔍 品質チェックリスト
- [ ] インデントがスペース2つで統一されているか
- [ ] `let` / `const` を適切に使い分けているか
- [ ] JSDocコメントが記述されているか
- [ ] エントリポイントとビジネスロジックが分離されているか
- [ ] バッチ処理・キャッシュを活用しているか
- [ ] エラーハンドリングが適切に実装されているか
- [ ] 機密情報がプロパティで管理されているか
- [ ] Prettier / ESLint による自動整形が適用されているか

---

## 📚 参考資料
* Google Apps Script ガイド: https://developers.google.com/apps-script
* JSDoc: https://jsdoc.app
* Prettier: https://prettier.io
* ESLint: https://eslint.org
* clasp: https://github.com/google/clasp 