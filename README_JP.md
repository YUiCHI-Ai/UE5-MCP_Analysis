# UE5-MCP (Model Control Protocol)

## 概要

UE5-MCP（Model Control Protocol）は、BlenderとUnreal Engine 5（UE5）のワークフローにAI駆動の自動化を統合するために設計されたプロジェクトです。このプロジェクトは[BlenderMCP](https://github.com/ahujasid/blender-mcp)をベースに拡張し、AIを活用したゲームレベル作成のためのエンドツーエンドパイプラインを提供します。レベルデザイン、アセット管理、ゲームプレイプログラミングを強化します。

## 主な機能

- **AI駆動のシーン生成**（BlenderMCP経由）:
  - Blenderでのテキストからシーンへの変換
  - 画像を参照としたシーン生成
  - マテリアルとテクスチャの管理
  - 自動シーン構成
  - PolyHavenアセットの統合
- **Unreal Engine統合**:
  - 自動シーンインポート
  - ゲームレベル変換
  - マテリアルとライティングの転送
  - レベル最適化ユーティリティ
  - Blueprintベースのシーン操作
- **アセット管理と作成**: 
  - AIを使用した3Dモデル、テクスチャ、マテリアルの生成と修正
  - BlenderとUE5間のアセット転送のワークフロー自動化
- **ゲームプレイプログラミングとデバッグ**: 
  - Bluepriptスクリプティングの支援
  - パフォーマンスプロファイリング
  - UE5の自動デバッグ

## 前提条件

- Blender（最新の安定版）
- Unreal Engine 5（UE5）
- Python 3.x
- 必要なPythonパッケージ（`dependencies.md`に記載）

## インストール

1. リポジトリをクローン:
   ```bash
   git clone https://github.com/your-repo/UE5-MCP.git
   ```
2. 依存関係をインストール:
   ```bash
   pip install -r requirements.txt
   ```
3. 設定を構成（`configurations.md`を参照）
4. ワークフロー手順（`workflow.md`）に従ってBlenderまたはUE5内でMCPを実行

## 使用方法

- **Blender**: Blender-MCPを使用してシーン作成とアセット生成を自動化
- **Unreal Engine 5**: UE5-MCPを活用して自動レベルデザイン、アセット統合、デバッグを行う
- **AI統合**: AI駆動の自動化機能については`ai_integration.md`を確認

詳細なコマンドとパラメータについては、`commands.md`を参照してください。

## アーキテクチャ

MCPは以下の主要コンポーネントで構成されています：

1. **コア処理レイヤー**
   - 手続き型生成と自動化のためのAI対話を処理
   - 外部AIモデル（Claude、GPT、Stable Diffusionなど）と通信
   - 自然言語指示を実行可能なタスクに解釈するロジックを管理

2. **Blender-MCPモジュール**
   - BlenderのAPIと統合してシーン作成、アセット修正、マテリアル生成を自動化
   - 手続き型モデリングとテクスチャ合成の自動化のためのPythonスクリプティングをサポート
   - 標準化されたフォーマットを介してUE5への直接アセット転送を促進

3. **UE5-MCPモジュール**
   - Unreal Engine 5内で動作し、レベルデザイン、Blueprint自動化、デバッグを効率化
   - UE5のPython APIとBlueprintsを使用してエンジン内アセットとゲームプレイロジックを修正
   - 地形生成、植生配置、環境構造化のためのAI駆動ツールを実装

4. **ミドルウェア通信レイヤー**
   - BlenderとUE5を橋渡しし、シームレスなアセット移行を確保
   - 複数のプラットフォーム間でのAPI認証とコンテキスト追跡を管理
   - AI対話とスクリプト自動化のための抽象化レイヤーを提供

5. **データストレージと構成管理**
   - メタデータ、ユーザー設定、手続き型生成テンプレートを保存
   - 異なるタイプのワークフロー用の構成プリセットをサポート

## ワークフロー

MCPのエンドツーエンドワークフローは以下のステップで構成されています：

### ステップ1: Blenderでのアセット作成
1. Blenderを開き、MCPプラグインをロード
2. `mcp.generate_scene "description"`を使用して手続き型シーンを作成
3. `mcp.add_object "object_type" x y z`を使用してシーンにオブジェクトを追加
4. `mcp.generate_texture "object_name" "texture_type"`を使用して手続き型テクスチャを適用
5. `mcp.export_asset "object_name" "format" "filepath"`を使用してUE5用にアセットをエクスポート

### ステップ2: UE5へのアセット転送
1. Blenderからエクスポートした`.fbx`、`.obj`、`.gltf`ファイルをUE5にインポート
2. UE5内のMCPツールを使用してアセットを処理および最適化
3. MCPテクスチャマッピングを使用して自動的にマテリアルとテクスチャを割り当て

### ステップ3: UE5でのレベルデザイン自動化
1. `mcp.generate_terrain width height detail_level`を使用して手続き型地形を生成
2. `mcp.populate_level "asset_type" density`を使用してレベルにアセットを配置
3. AIが地形とゲームプレイ要件に基づいて配置を洗練し提案

### ステップ4: Blueprint自動化とゲームプレイロジック
1. `mcp.generate_blueprint "logic_description"`を使用してゲームプレイスクリプトを作成
2. UE5のエディタ内でテストと反復
3. `mcp.profile_performance "level_name"`を使用してパフォーマンスの問題をデバッグ

### ステップ5: プロジェクトの最終化とエクスポート
1. LOD生成と圧縮を使用してアセットを最適化
2. AI支援のデバッグを実行してゲームプレイメカニクスを洗練
3. デプロイメント用にプロジェクトをパッケージ化

## 設定

MCPの設定ファイル（`mcp_config.json`）は以下の場所にあります：
- **Blender:** `~/.mcp/blender_mcp_config.json`
- **Unreal Engine 5:** `~/.mcp/ue5_mcp_config.json`

主な設定カテゴリ：
1. **一般設定**: AI有効化、デフォルトエクスポート形式、ログレベルなど
2. **Blender-MCP設定**: シーン生成スタイル、テクスチャ解像度、LODレベルなど
3. **Unreal Engine 5-MCP設定**: レベルデザイン自動化、パフォーマンス最適化など
4. **AI統合**: AIプロバイダー、APIキー、AI提案など

設定の詳細については、`configurations.md`を参照してください。

## トラブルシューティング

一般的な問題と解決策：

1. **MCPが起動しない**
   - Pythonがインストールされていることを確認
   - 必要な依存関係を再インストール
   - 設定ファイルにエラーがないか確認

2. **Blender固有の問題**
   - BlenderがMCPプラグインでロードされていることを確認
   - エクスポート前にテクスチャがパックされていることを確認

3. **UE5固有の問題**
   - UE5のPython Editor Script Pluginが有効になっていることを確認
   - パフォーマンスボトルネックにはMCPのLOD最適化を使用

4. **AI統合の問題**
   - AIプロバイダーが正しく設定されていることを確認
   - APIキーが正しく有効であることを確認
   - ネットワーク接続を確認

詳細なトラブルシューティングについては、`troubleshooting.md`を参照するか、GitHubで問題を提出してください。

## 貢献

貢献ガイドラインとベストプラクティスについては、`CONTRIBUTING.md`を参照してください。

## ライセンス

ライセンスの詳細については、`LICENSE.md`を参照してください。

## 謝辞

- BlenderMCPプロジェクトの貢献者
- Unreal Engineコミュニティ
- [その他の謝辞]