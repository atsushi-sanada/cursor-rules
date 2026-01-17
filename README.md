# cursor-rules（開発ルール集）

エディタ/IDE での開発における共通ルール集です。**「何をどの順で読めば迷わないか」**に絞って案内します。

## 概要

チーム開発での品質と効率を上げるための **Markdownルール／READMEルール／コード規約／AIガバナンス** をまとめたリポジトリです。

### このツールについて

<!-- 最初に期待値を揃えて迷いを減らす -->

- **できること（1機能1行）**
  - 必要なルールファイルを選んで、プロジェクトにコピペして導入できる
  - README・Markdown・命名・フォルダ構成・言語別コード規約を、最低限の型として揃えられる
  - AI利用時のガバナンスやロール別ルールを、チーム標準として共有できる
- **未対応 / 制約 / 注意**：このリポジトリ自体は実行ツールではありません（ルール文書の集約）。
- **設定 / カスタマイズ**：各 `.md` をプロジェクト向けに編集して使用します（詳細は「使い方」参照）。

---

## クイックスタート

### 初回のみ

1. このリポジトリを clone して手元で参照できるようにする
2. `00_common/` を先に読み、文書ルール（README/Markdown）をプロジェクトに導入する
3. `03_code/` から、対象言語のコード規約とフォルダ構成ルールを導入する

### 毎回

1. ルールを更新する（pull）／必要な差分だけプロジェクトへ反映する
2. README や規約が増えすぎた場合は「入口に戻す」（要点だけ残し、詳細は各ファイルへ逃がす）

---

## セットアップ

### 必要なもの

- Git
- （任意）Node.js / Python / Docker（プロジェクト側の事情に合わせる）

### インストール（参照用にclone）

```bash
git clone https://github.com/atsushi-sanada/cursor-rules.git
cd cursor-rules
```

---

## 使い方

このリポジトリは **「必要なルールだけ選んでコピペ」**する運用を想定しています。

### まず読む（おすすめ順）

1. `00_common/readme_rule.md`（READMEの型）
2. `00_common/markdown_rule.md`（Markdown記法の統一）
3. `03_code/folder_structure_rules.md`（フォルダ構成の考え方）
4. `03_code/source_code_rules.md`（ソースコード全般の共通ルール）
5. `03_code/lang/`（言語別ルール）

### 導入のやり方

- プロジェクトに合わせて、必要な `.md` をコピーして配置します
- チームで使う場合は、参照先を固定するために「プロジェクト側の docs/ や rules/ に置く」運用がおすすめです

---

## プロジェクト構成

```
cursor-rules/
├── 00_common/               # 共通ルール（README/Markdown/命名など）
├── 01_ai-governance/        # AIガバナンスルール
├── 02_ai-role/              # AIロール別ルール
├── 03_code/                 # コード・スクリプトルール
└── README.md                # この説明書
```

---

## 貢献（Contributing）

Issue / Pull Request を歓迎します。変更する場合は「どのプロジェクトで困ったか」「どう改善するか」を一緒に書いてください。

---

## License

MIT

---

## Author

- 真田 淳史（[@atsushi-sanada](https://github.com/atsushi-sanada)）

---

## References

- [Google Style Guides](https://github.com/google/styleguide)
