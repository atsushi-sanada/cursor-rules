# cursor-rules

## 概要

Cursor で使う開発ルールと AI 向け運用ルールを管理するリポジトリです。
日本語の原本を `00_common/`、`01_ai-governance/`、`02_ai-role/`、`03_code/` に置き、AI が実行しやすい英語版を `.cursor/rules/*.mdc` として運用します。
このリポジトリ自体は実行ツールではなく、Cursor Rules と Skill を管理するためのルール集です。

## クイックスタート

1. Cursor でこのリポジトリを開き、`.cursor/rules/*.mdc` を現在の有効ルールとして使います。
2. ルールを直すときは日本語原本の `.md` を先に編集します。
3. `/translate-ai-guidance-to-english @対象ファイル` で英語版 `.mdc` に反映します。

## セットアップ

1. Git を用意します。
2. リポジトリを clone します。

```bash
git clone https://github.com/atsushi-sanada/cursor-rules.git
cd cursor-rules
```

3. Cursor で `cursor-rules` フォルダを開きます。
4. 他プロジェクトで使う場合は、必要な `.cursor/rules/*.mdc` と `.cursor/skills/` を対象プロジェクトへコピーします。

## 使い方

1. 日本語原本を読む場合は、共通ルールから順に確認します: `00_common/`、`01_ai-governance/`、`02_ai-role/`、`03_code/`。
2. Cursor に読ませる実運用ファイルは `.cursor/rules/*.mdc` を使います。
3. 日本語原本を更新したら、`/translate-ai-guidance-to-english @対象ファイル` を実行して `.mdc` を更新します。
4. 新しいルールを追加するときは、まず日本語の `.md` を作り、次に英語の `.mdc` を作ります。
5. Skill の内容を直すときは `.cursor/skills/translate-ai-guidance-to-english/` を更新します。

## GitHub.com への自動ミラー（GHE 正本）

正本は `https://github.enish.jp/doge/cursor-rules.git` です。`main` へ push すると、GitHub Actions が `https://github.com/atsushi-sanada/cursor-rules.git` へブランチとタグを同期します。

### 初回セットアップ（GHE 側）

1. GitHub.com で Personal Access Token（classic）または fine-grained token を作成します。
   - classic: `repo` スコープ
   - fine-grained: 対象リポジトリ `atsushi-sanada/cursor-rules` に **Contents: Read and write**
2. GHE の `doge/cursor-rules` → **Settings** → **Secrets and variables** → **Actions** に、名前 `MIRROR_TO_GITHUB_COM_TOKEN` でトークンを登録します（`GITHUB_` で始まる名前は GHE では登録できません）。
3. このリポジトリの workflow を GHE の `main` に push します。
4. **Actions** タブで `Mirror to GitHub.com` が成功することを確認します。手動実行は **Run workflow**（`workflow_dispatch`）でも可能です。

### 反映タイミング

- GHE の `main` への push 直後に workflow が起動します。
- 通常は Runner の待ち時間を含め **1〜5 分程度** で GitHub.com に反映されます（Runner の混雑状況により変動します）。

### Runner ラベル

workflow は既定で `ubuntu-latest` です。社内 Runner のラベルが異なる場合は、`.github/workflows/mirror-to-github-com.yml` の `runs-on` を変更してください。

## その他

- `.md` は人間が読む原本、`.mdc` は Cursor Agent が実行する英語ルールです。
- README には運用の入口だけを書き、詳細は各ルールファイルと Skill 内ドキュメントを参照します。
- 変更をコミットするときは、ルール追加・Skill 更新・README 更新を責務単位で分けます。
- License: MIT
