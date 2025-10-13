# フォルダ構成ルール

プロジェクトごとに最適化されたフォルダ構成を推奨します。以下の共通方針に沿って、主要言語別のサンプル構成をご参照ください。

---

## 🧠 共通方針
- 責務分離：機能ベースまたはレイヤーベースでフォルダを分割
- 環境別設定：`dev/` / `prod/` を分離するか、`config/` で制御
- ルート直下：最低限 `README.md`, `.gitignore` を配置
- 明示的分離：`tests/`, `docs/`, `scripts/` を必ず独立フォルダにする
- ネストは最大2階層までに留める

---

## 1. JavaScript / TypeScript (Node.js・React)
```
project/
├── public/              # 静的アセット
├── src/                 # ソースコード
│   ├── components/      # UIコンポーネント
│   ├── pages/           # ページ（Next.js等）
│   ├── hooks/           # カスタムフック
│   ├── services/        # API呼び出し
│   ├── utils/           # 汎用関数
│   └── types/           # 型定義（.ts）
├── tests/               # テストコード
├── scripts/             # ビルド／CI補助
├── .env                 # 環境変数
└── README.md
```

## 2. Python (Web/API/データ処理)
```
project/
├── app/                 # アプリ本体
│   ├── api/             # ルーティング
│   ├── models/          # ORMモデル
│   ├── services/        # ビジネスロジック
│   ├── core/            # 設定・依存定義
│   └── utils/           # 共通ユーティリティ
├── tests/               # pytest用テスト
├── scripts/             # バッチ／スクリプト
├── requirements.txt     # 依存
└── README.md
```

## 3. C# (Unity / .NET)
**Unity:**
```
project/
├── Assets/             # Unity標準フォルダ
│   ├── Scripts/        # C#スクリプト
│   ├── Art/            # 画像・音声
│   └── Prefabs/        # プレハブ
├── ProjectSettings/    # Unity設定
└── README.md
```
**.NET（非Unity）:**
```
project/
├── src/                # ソース（.csproj）
│   └── AppName/
│       ├── Controllers/
│       ├── Models/
│       ├── Services/
│       └── Program.cs
├── tests/              # テスト（xUnit等）
├── scripts/            # デプロイ／初期化
└── README.md
```

## 4. Java (Spring Boot / Android)
```
project/
├── src/
│   ├── main/
│   │   ├── java/       # パッケージ構造
│   │   └── resources/  # 設定ファイル
│   └── test/          # テスト
├── build.gradle       # or pom.xml
└── README.md
```

## 5. Go (Go Modules)
```
project/
├── cmd/                # エントリポイント
├── internal/           # 内部パッケージ
├── pkg/                # 再利用ライブラリ
├── api/                # gRPC/OpenAPI定義
├── scripts/            # 開発用スクリプト
└── go.mod
```

## 6. Rust
```
project/
├── src/
│   ├── main.rs
│   ├── lib.rs
│   └── modules/        # 機能別モジュール
├── tests/              # 統合テスト
├── Cargo.toml
└── README.md
```

## 7. Swift (iOS)
```
project/
├── Project.xcodeproj
├── Sources/            # ソースコード
├── Resources/          # 画像・Asset
├── Tests/              # テスト
└── README.md
```

---

### ⚙️ 運用のヒント
- 言語をまたぐ共通部分は `shared/` にまとめる
- CI/CD設定や環境ファイルは `config/` に集約
- 新規導入時はこのテンプレートをコピーして調整してください 