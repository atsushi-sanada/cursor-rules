# cursor-rules

## 概要

Cursor 向けのチーム共通ルールを一か所で管理し、各プロジェクトまたは Cursor 設定へ配布するリポジトリです。日本語の原稿（`.md`）を編集し、英語の実行用ファイル（`.mdc`）に反映して使います。

### このリポジトリについて

- 原稿（日本語 `.md`）: `00_common` / `01_ai-governance` / `02_ai-role` / `03_code`
- 実行用（英語 `.mdc`）: `.cursor/rules/`
- `main` へ push すると GitHub.com にミラーされ、**Remote Rules** から参照できる

### 適用方法

| 方法 | 用途 | 操作 |
| --- | --- | --- |
| **Project Rules** | プロジェクト単位でルールを適用 | `.mdc` をそのプロジェクトの `.cursor/rules/` に配置 |
| **Remote Rules** | GitHub 上の同一版を参照 | Cursor で `atsushi-sanada/cursor-rules` を指定 |
| **User Rules** | 全プロジェクトで共通適用 | Cursor 設定 → Rules → User Rules へ **手動でコピー** |

User Rules はリポジトリと自動同期されません。内容を更新した場合は、設定画面へ再度貼り付けてください。

---

## セットアップ

- Cursor（Project Rules 対応）
- ルールを編集・配布する場合は Git

1. リポジトリを clone する
2. 「適用方法」の表から利用方法を選ぶ
3. Project Rules の場合: 使う `.mdc` を作業プロジェクトの `.cursor/rules/` に配置する
4. Cursor でプロジェクトを開き、ルールが効いているか確認する
5. `alwaysApply: true`（常時適用）と `globs`（対象ファイルの限定）を確認する

---

## 使い方

1. 日本語原稿（`00_common/` などの `.md`）を編集する
2. 対応する `.cursor/rules/*.mdc` を更新する
3. 配布する
   - Project Rules: 各リポジトリの `.cursor/rules/` へコピー
   - Remote Rules: `main` に push（`MIRROR_TO_GITHUB_COM_TOKEN` 設定時は GitHub.com に同期）
   - User Rules: 設定画面へ手動で貼り付け

---

## その他

- 詳細は各 `.md` / `.mdc` を参照する
- `.cursor/cursor-documents/` は Git 管理外
- **Project Rules**: プロジェクト単位 / **User Rules**: ユーザー全体 / **globs**: 指定したファイルのみルール適用
