# UE5-MCP 使用ガイド

## 概要

このガイドでは、UE5-MCP（Model Control Protocol）を使用してBlenderとUnreal Engine 5（UE5）でのAI駆動の自動化ワークフローを実装する方法について詳細に説明します。初心者から上級者まで、段階的な手順とベストプラクティスを提供します。

---

## 1. 初期セットアップ

### 1.1 環境の準備

1. **前提条件のインストール**:
   - Blender（最新の安定版）をインストール
   - Unreal Engine 5をインストール
   - Python 3.xをインストール
   ```bash
   # Pythonのバージョンを確認
   python --version
   ```

2. **UE5-MCPのインストール**:
   ```bash
   # リポジトリをクローン
   git clone https://github.com/your-repo/UE5-MCP.git
   
   # 依存関係をインストール
   cd UE5-MCP
   pip install -r requirements.txt
   ```

3. **Blenderの設定**:
   - Blenderを起動
   - `編集` → `設定` → `アドオン`に移動
   - `Node Wrangler`を検索して有効化
   - MCPプラグインをインストール（`ファイル` → `インストール`からアドオンZIPを選択）

4. **UE5の設定**:
   - UE5を起動
   - `編集` → `プラグイン`に移動
   - 以下のプラグインを検索して有効化:
     - `Python Editor Script Plugin`
     - `UnrealCV Plugin`
     - `Procedural Content Generation (PCG) Framework`
   - UE5を再起動して変更を適用

### 1.2 設定ファイルの構成

1. **MCP設定ファイルの場所**:
   - Blender: `~/.mcp/blender_mcp_config.json`
   - UE5: `~/.mcp/ue5_mcp_config.json`

2. **基本設定の例**:
   ```json
   {
     "ai_enabled": true,
     "default_export_format": "fbx",
     "logging_level": "INFO",
     "auto_update": true,
     "ai_integration": {
       "provider": "openai",
       "api_key": "your-api-key-here"
     }
   }
   ```

3. **APIキーの設定**:
   - OpenAI、Stable Diffusion、またはClaudeのAPIキーを取得
   - 設定ファイルの`api_key`フィールドに追加

---

## 2. Blenderでの基本的な使用方法

### 2.1 シーン生成

1. **テキストからシーンを生成**:
   - Blenderを起動し、MCPプラグインが有効になっていることを確認
   - Blenderのコンソールを開く（`ウィンドウ` → `トグルシステムコンソール`）
   - 以下のコマンドを実行:
   ```python
   import mcp
   mcp.generate_scene("森の中の小さな湖。周囲に木々と岩がある")
   ```

2. **シーンのカスタマイズ**:
   - 生成されたシーンに特定のオブジェクトを追加:
   ```python
   mcp.add_object("tree", 5.0, 2.5, 0.0)  # x, y, z座標に木を追加
   ```
   
   - オブジェクトにテクスチャを適用:
   ```python
   mcp.generate_texture("tree", "樹皮のテクスチャ、茶色で粗い表面")
   ```

### 2.2 アセットのエクスポート

1. **単一アセットのエクスポート**:
   ```python
   mcp.export_asset("tree_model", "fbx", "./exports/tree.fbx")
   ```

2. **複数アセットのバッチエクスポート**:
   ```python
   mcp.batch_export(["tree_model", "rock_model"], "fbx", "./exports/")
   ```

3. **エクスポート前の最適化**:
   ```python
   # ポリゴン数を減らす
   mcp.optimize_asset("tree_model", polycount=5000)
   
   # LODを生成
   mcp.generate_lods("tree_model", levels=3)
   ```

---

## 3. UE5での基本的な使用方法

### 3.1 アセットのインポートと管理

1. **Blenderからのアセットインポート**:
   - UE5を起動し、新規または既存のプロジェクトを開く
   - Pythonコンソールを開く（`ウィンドウ` → `開発者ツール` → `出力ログ`）
   - 以下のコマンドを実行:
   ```python
   import mcp
   mcp.import_asset("./exports/tree.fbx")
   ```

2. **マテリアルの自動割り当て**:
   ```python
   mcp.apply_material("tree_model", "bark_material")
   ```

### 3.2 レベルデザイン自動化

1. **手続き型地形の生成**:
   ```python
   # 2000x2000の高詳細地形を生成
   terrain = mcp.generate_terrain(2000, 2000, "high")
   ```

2. **アセットの自動配置**:
   ```python
   # 500本の木をレベルに配置
   mcp.populate_level("trees", 500)
   
   # 特定の密度で岩を配置
   mcp.populate_level("rocks", 200, density=0.5)
   ```

3. **環境の自動生成**:
   ```python
   # 自然な森の環境を生成
   mcp.generate_environment("forest", size=5000)
   ```

### 3.3 Blueprint自動化

1. **基本的なBlueprintの生成**:
   ```python
   # インタラクティブなドアのBlueprintを生成
   door_bp = mcp.generate_blueprint("プレイヤーが近づくと開き、離れると閉じるドア")
   ```

2. **AIキャラクターの動作**:
   ```python
   # 敵AIのBlueprintを生成
   enemy_bp = mcp.generate_blueprint("プレイヤーを見つけると追いかけ、近づくと攻撃するNPC")
   ```

3. **環境インタラクション**:
   ```python
   # 破壊可能なオブジェクトのBlueprintを生成
   mcp.generate_blueprint("プレイヤーが攻撃すると破壊される木箱、アイテムをドロップする")
   ```

---

## 4. 高度な使用方法

### 4.1 AI駆動のワークフロー自動化

1. **完全なレベル生成パイプライン**:
   ```python
   # Blenderでシーンを生成
   mcp.generate_scene("中世の村、中央に広場と城がある")
   
   # アセットを最適化してエクスポート
   mcp.optimize_all_assets()
   mcp.batch_export_all("fbx", "./exports/")
   
   # UE5でレベルを構築
   mcp.import_all_assets("./exports/")
   mcp.build_level("medieval_village")
   
   # ゲームプレイロジックを追加
   mcp.generate_blueprint("村人NPCが日中は仕事をし、夜は家に帰る")
   ```

2. **スタイル転送と変更**:
   ```python
   # 既存のシーンのスタイルを変更
   mcp.transform_style("current_scene", "ファンタジー風から未来的なスタイルへ")
   ```

### 4.2 パフォーマンス最適化

1. **レベル分析**:
   ```python
   # パフォーマンスの問題を特定
   report = mcp.profile_performance("open_world_map")
   print(report.bottlenecks)
   ```

2. **自動最適化**:
   ```python
   # レポートに基づいて最適化を適用
   mcp.apply_optimizations(report)
   ```

3. **LODの一括生成**:
   ```python
   # すべてのアセットにLODを生成
   mcp.generate_lods_for_all(levels=3)
   ```

### 4.3 カスタムAI統合

1. **カスタムAIプロンプトの使用**:
   ```python
   # より詳細なプロンプトでシーンを生成
   mcp.generate_scene_with_prompt("""
   夕暮れ時の海辺の村。
   波が岸に打ち寄せ、漁船が港に戻ってきている。
   村の家々は丘の上に建ち並び、窓から温かい光が漏れている。
   遠くには灯台が見え、その光が海面に反射している。
   """)
   ```

2. **AIモデルの切り替え**:
   ```python
   # 特定のタスクに異なるAIモデルを使用
   mcp.set_ai_model("texture_generation", "stable_diffusion")
   mcp.set_ai_model("blueprint_logic", "claude")
   ```

---

## 5. トラブルシューティング

### 5.1 一般的な問題

1. **MCPが起動しない**:
   - Pythonのバージョンを確認（Python 3.xが必要）
   - 依存関係が正しくインストールされているか確認
   - 設定ファイルにエラーがないか確認

2. **コマンドが見つからない**:
   - MCPがインストールされ、PATHに含まれているか確認
   - 仮想環境を使用している場合は、それが有効になっているか確認

### 5.2 Blender固有の問題

1. **シーンが生成されない**:
   - Blenderが正しいバージョンであることを確認（3.x以上）
   - MCPプラグインが正しくロードされているか確認
   - デバッグモードでコマンドを実行:
   ```python
   mcp.generate_scene("森のシーン", debug=True)
   ```

2. **エクスポートされたアセットにテクスチャがない**:
   - エクスポート前にテクスチャがパックされているか確認
   ```python
   mcp.export_asset("model", "fbx", "./export.fbx", include_textures=True)
   ```

### 5.3 UE5固有の問題

1. **MCPがUE5で検出されない**:
   - Python Editor Script Pluginが有効になっているか確認
   - UE5を再起動して変更を適用

2. **生成されたBlueprintが機能しない**:
   - 生成されたBlueprintノードに接続が欠けていないか確認
   - デバッグコマンドを使用:
   ```python
   mcp.debug_blueprint("door_script")
   ```

3. **パフォーマンスの問題**:
   - アセットのポリゴン数を減らす
   ```python
   mcp.optimize_level(max_polycount=500000)
   ```
   - 動的ライティングを無効にする（設定ファイルで）

### 5.4 AI統合の問題

1. **AI機能が動作しない**:
   - APIキーが正しく設定されているか確認
   - ネットワーク接続を確認
   - AIプロバイダーが正しく設定されているか確認

2. **生成結果の品質が低い**:
   - より詳細なプロンプトを提供
   - 異なるAIモデルを試す
   - 生成パラメータを調整

---

## 6. ベストプラクティス

### 6.1 効率的なワークフロー

1. **モジュール式アプローチ**:
   - 大きなシーンを小さなコンポーネントに分割
   - 再利用可能なアセットライブラリを構築

2. **反復的な開発**:
   - 低解像度/低詳細度で迅速にプロトタイプを作成
   - 満足のいく結果が得られたら詳細度を上げる

3. **バージョン管理**:
   - 重要なマイルストーンでシーンとアセットのバックアップを作成
   ```python
   mcp.save_snapshot("village_scene_v1")
   ```

### 6.2 パフォーマンス最適化

1. **早期最適化**:
   - 開発サイクルの早い段階でパフォーマンスを考慮
   - 適切なLODレベルを設定

2. **アセット再利用**:
   - インスタンス化を活用して同じアセットを複数回使用
   ```python
   mcp.instance_asset("tree", positions_list)
   ```

3. **バッチ処理**:
   - 可能な限り操作をバッチ処理
   ```python
   mcp.batch_process(assets_list, "optimize")
   ```

### 6.3 AI活用のコツ

1. **明確なプロンプト**:
   - 具体的で詳細なプロンプトを使用
   - スタイル、雰囲気、重要な要素を指定

2. **人間とAIの協働**:
   - AIを出発点として使用し、手動で洗練
   - 複雑な部分はAIに、細部の調整は手動で

3. **実験と学習**:
   - 異なるプロンプトとパラメータを試す
   - 成功したアプローチを記録して再利用

---

詳細なコマンドとパラメータについては、`commands.md`を参照してください。
設定オプションについては、`configurations.md`を参照してください。
トラブルシューティングの詳細については、`troubleshooting.md`を参照してください。