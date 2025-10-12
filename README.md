# Cursor Rules / カーソルルール

Cursorエディタ向けの共通開発ルール集。チーム開発での品質と効率を向上させます。

---

## 📌 Features / 特徴

- 統一されたルールセットで可読性・保守性を担保
- 多言語・複数プロジェクトに対応
- 継続的改善とバージョン管理がしやすい

---

## 🚀 Getting Started / はじめに

### 必要環境（Requirements）

- Cursorエディタ
- Git
- （任意）Node.js v18+、Python 3.10+、Docker

### セットアップ（Installation）

```bash
git clone https://github.com/atsushi-sanada/cursor-rules.git
cd cursor-rules
```

必要に応じて各ルールファイルを参照し、プロジェクトに適用してください。

---

## 📂 Project Structure / プロジェクト構成

```
cursor-rules/
├── @/00_common/            # 共通ルール
├── 01_ai-governance/       # AIガバナンスルール
├── 02_ai-role/             # AIロール別ルール
├── 03_code/                # コード・スクリプトルール
└── README.md               # プロジェクト概要
```

---

## 🧪 Usage / 使い方

各フォルダの `.md` ファイルをCursorのルールなどにコピペして使用してください

---

## 📝 Configuration / 設定方法

環境ごとの設定は `config/` または各プロジェクトフォルダ内の `config/` を利用して管理してください。

---

## 🤝 Contributing / 貢献

プルリクエスト・Issue を歓迎します。変更を加える際は各ルールファイルに対してレビューを依頼してください。

---

## 📃 License

MIT License

---

## 👤 Author / クレジット

* 真田 淳史（[@atsushi-sanada](https://github.com/atsushi-sanada)）

---

## 📎 References / 関連リンク

* [Cursor Editor](https://cursor.sh/)
* [Google Style Guides](https://github.com/google/styleguide) 
