# 1 機能のフォルダ構成・ファイル ≒ クラス構成ルール

- 以下は 1 機能におけるフォルダ・ファイル構成のルールです。
- 例として Unity 環境に寄せてますが、全ての環境でこの方針に従ってください。

## コーディングルールの基本方針

誰が見ても理解できるコードを書く

- 設計: 構造が明快で拡張を妨げないこと
- 品質: 動作が安定し、再現性が高いこと
- 運用: 誰が引き継いでも一貫して動作すること

## フォルダ構成・ファイル ≒ クラス構成の基本方針

1 機能単位（Feature）を最小・再利用可能・責務分離の SOLID の原則に基づく。

### SOLID

| 頭文字 | 名称                            | 日本語名                   | 目的                                                       |
| ------ | ------------------------------- | -------------------------- | ---------------------------------------------------------- |
| **S**  | Single Responsibility Principle | 単一責任の原則             | クラスは 1 つの責務だけを持つ                              |
| **O**  | Open/Closed Principle           | 開放・閉鎖の原則           | 変更に閉じ、拡張に開く                                     |
| **L**  | Liskov Substitution Principle   | リスコフの置換原則         | 派生クラスは基底クラスと完全に互換であるべき               |
| **I**  | Interface Segregation Principle | インターフェイス分離の原則 | 不要なメソッドを持つ巨大なインターフェイスを作らない       |
| **D**  | Dependency Inversion Principle  | 依存性逆転の原則           | 具象（具体実装）ではなく抽象（インターフェイス）に依存する |

### 基本原則

| 自作原則                   | 対応する SOLID 原則 | 解説                                     |
| -------------------------- | ------------------- | ---------------------------------------- |
| 1 機能＝ 1 フォルダ        | SRP（単一責任）     | 機能単位で責務を閉じる。変更理由が明確。 |
| フォルダ＝クラス責務の単位 | SRP                 | 階層構造と責務の整合。                   |
| Manager 単一化原則         | OCP / DIP           | 拡張を許容しつつ依存を集中制御。         |
| 責務分離（SRP）            | SRP / ISP           | 表示・演出・ロジックを分離し責務を限定。 |
| 依存方向の一方向化         | DIP                 | 具象に依存しない一方向構造。             |
| 共通層独立化               | OCP / SRP           | 機能横断の共通処理を抽象層に独立化。     |

## ルール

### クラス構成の考え方

1 機能に対するクラス構成は以下。

```text
FeatureName/
 ├─ FeatureManager.cs	※エントリポイント。初期化・依存関係解決・更新ループ管理
 ├─ FeatureController.cs	※入力の解釈とServiceへの出し入れ、Viewの状態制御。
 ├─ FeatureView.cs	※UIなどの表示制御
 ├─ FeatureService.cs ※ゲームロジック
 └─ FeatureData.cs ※設定値や構造体
```

### ゲーム開発における基本構造

ゲームにおいての 1 機能は UI+α の制御を行うので、基本的には以下の構造になる。
α とは基本的には視覚・演出的なシーン要素。

```text
FeatureName/
 ├─ FeatureManager.cs
 ├─ FeatureUIController.cs
	├─ FeatureUIView.cs
	├─ FeatureUIService.cs
	└─ FeatureUIData.cs
 └─ FeatureSceneController.cs
	├─ FeatureSceneView.cs
	├─ FeatureSceneService.cs
	└─ FeatureSceneData.cs
```

### 実際のクラス構成ルール

さらにゲーム開発においては 1 機能＝ 1 場面となる場合が多く、FeatureScene 内の構造は以下と体系化できる。

```text
FeatureName/
 ├─ FeatureManager.cs ※1
	├─ FeatureUIController.cs
	│  ├─ FeatureUIView.cs
	│  ├─ FeatureUIService.cs
	│  └─ FeatureUIData.cs
	├─ FeatureSceneController.cs
	│  ├─ FeatureSceneView.cs
	│  ├─ FeatureSceneService.cs
	│  ├─ FeatureSceneData.cs
	│  ├─ FeatureCameraController.cs　
	│  ├─ FeatureCharacterController.cs　 ※2
	│  ├─ FeatureBackgroundController.cs　 ※2
	│  ├─ FeatureEffectController.cs　 ※3
	│  └─ FeatureSoundController.cs	※3
	└─ FeatureService.cs / FeatureData.cs （共通）
```

※1 1 機能について Manager クラスは基本的には 1 つ。複数も設けたい場合は統括する上位クラスを設けて機能のエントリポイントは 1 つにする。

※2 制御したいもので異なる。カードだったり、LIVE2D だったり。

※3 機能独自のものがある場合。エフェクトやサウンドはゲーム共通のものがあるで基本的には不要。

### フォルダ・ファイル構成

Unity を例にすると実際のプロジェクト上のフォルダ・ファイル構成は以下のように管理する。

```text
FeatureName/
 ├─ FeatureManager.cs
 ├─ FeatureService.cs
 ├─ FeatureData.cs （共通）
 └─ FeatureController/
	├─ FeatureUI/
		├─ FeatureUIController.cs
		│  ├─ FeatureUIView.cs
		│  ├─ FeatureUIService.cs
		│  └─ FeatureUIData.cs
	└─ FeatureScene/
		├─ FeatureSceneController.cs
		│  ├─ FeatureSceneView.cs
		│  ├─ FeatureSceneService.cs
		│  ├─ FeatureSceneData.cs
		│  ├─ FeatureCameraController.cs　
		│  ├─ FeatureCharacterController.cs　 ※2
		│  ├─ FeatureBackgroundController.cs　 ※2
		│  ├─ FeatureEffectController.cs　 ※3
		│  └─ FeatureSoundController.cs	※3
```
