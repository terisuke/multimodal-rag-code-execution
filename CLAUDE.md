# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリでコードを扱う際のガイダンスを提供します。

## プロジェクト概要

Research CoPilotは、コード実行機能を持つマルチモーダルRAG（検索拡張生成）システムです。PDF、DOCX、XLSXファイルからマルチモーダル文書（テキスト、画像、表）を処理し、検索可能なセマンティックコンテンツを作成し、コードインタープリターを備えたチャットインターフェースを通じて分析クエリを可能にします。

## 開発コマンド

### 環境セットアップ
```bash
# conda環境の作成とアクティベート
conda create -n mmdoc python=3.10
conda activate mmdoc

# 依存関係のインストール
pip install -r requirements.txt  # コアコンポーネント
pip install -r ui/requirements.txt  # UIコンポーネント
```

### アプリケーションの実行
アプリケーションは順番に実行する必要がある3つのコンポーネントで構成されています：

1. **APIサーバー（最初に実行必須）**:
```bash
cd code
python -m uvicorn api:app --reload --port 9000
```

2. **Chainlit Webアプリ（メインチャットインターフェース）**:
```bash
cd ui
chainlit run chat.py
```

3. **Streamlit Webアプリ（文書取り込みとプロンプト管理）**:
```bash
cd ui
streamlit run main.py
```

### テストと品質管理
```bash
# テストの実行
pytest

# 依存関係のチェック
pip check

# コードフォーマット（pre-commitが設定されている場合）
pre-commit run --all-files
```

## アーキテクチャ

### 主要コンポーネント
- **処理パイプラインシステム**: 設定可能なステップを持つ`Processor`クラスを使用したモジュラー文書処理
- **APIサーバー** (`code/api.py`): RESTエンドポイントを提供するFastAPIバックエンド
- **チャットインターフェース** (`ui/chat.py`): Chainlitベースの会話インターフェース
- **管理インターフェース** (`ui/main.py`): Streamlitベースの文書取り込みとプロンプト管理

### 文書処理パイプライン
文書は`code/processing_plan.json`で定義された設定可能なパイプラインを通じて処理されます：

- **PDF処理**: 3つのモード - `gpt-4-vision`、`document-intelligence`、`hybrid`
- **DOCX処理**: 2つのモード - `py-docx`、`document-intelligence`
- **XLSX処理**: 1つのモード - `openpyxl`

各パイプラインは、ステップ間で渡される`ingestion_pipeline_dict`を操作する個別の処理ステップで構成されています。

### 主要な処理ステップ
1. 文書抽出（テキスト、画像、表）
2. GPT-4Vを使用したマルチモーダルコンテンツ処理
3. コンテキスト保持を伴うスマートチャンク化
4. ハイブリッド検索最適化のためのタグ生成
5. Azure Cognitive Searchでのベクトル埋め込みとインデックス作成
6. 文書要約と分析の生成

## テクノロジースタック

### 主要技術
- **Python 3.10+**: 互換性のために必須
- **FastAPI**: APIサーバーフレームワーク
- **Chainlit/Streamlit**: Webインターフェースフレームワーク
- **Azure OpenAI**: 処理用のGPT-4/GPT-4Vモデル
- **Azure Cognitive Search**: ベクトルおよびハイブリッド検索バックエンド
- **Azure Document Intelligence**: 高度な文書処理

### 主要依存関係
- 文書処理: `pypdf`、`python-docx`、`openpyxl`、`pdfplumber`
- AI/ML: `openai`、`llama-index`、`tiktoken`
- Azureサービス: `azure-search-documents`、`azure-cosmos`、`azureml-core`

## 設定

### 環境セットアップ
1. `.env.sample`を`.env`にコピー
2. 必要なすべてのAzureサービスエンドポイントとキーを設定
3. ローカル開発の場合: `API_BASE_URL=http://localhost:9000`を設定
4. Azure OpenAIモデルが正しい名前でデプロイされていることを確認

### 必要なAzureサービス
- Azure OpenAI（GPT-4、GPT-4V、埋め込みモデル）
- Azure Cognitive Search
- Azure Cosmos DB
- Azure Document Intelligence
- Azure File Share ストレージ

## コード規約

### スタイルガイドライン
- **クラス**: PascalCase（例：`Processor`、`PdfProcessor`）
- **関数/変数**: snake_case
- **定数**: UPPER_SNAKE_CASE
- **インポート**: 相対インポートのtry/exceptブロックと絶対インポートのフォールバック

### ファイル構成
- `/code`: コア処理ロジックとAPI
- `/code/utils`: ヘルパー関数とユーティリティ
- `/ui`: Webインターフェースコンポーネント
- 設定ファイルはJSONまたは.env形式を使用

### 処理パイプラインの拡張
新しい処理ステップを追加するには：
1. 適切なプロセッサークラスに関数を追加
2. 新しいステップで`processing_plan.json`を更新
3. ステップが`ingestion_pipeline_dict`を操作し、返すことを確認

## 開発のベストプラクティス

### 変更を提出する前に
1. `pytest`を実行してテストが通ることを確認
2. APIサーバーの起動とエンドポイントをテスト
3. 両方のUIアプリケーションが正しく機能することを確認
4. サンプルファイルで文書処理パイプラインをテスト
5. コードにハードコードされた認証情報がないことを確認

### 一般的な開発タスク
- **新しい文書プロセッサーの追加**: ベース`Processor`クラスを拡張
- **処理ステップの変更**: 関数と`processing_plan.json`を更新
- **UI変更**: `/ui`のChainlitまたはStreamlitコンポーネントを変更
- **APIエンドポイント**: 適切なリクエストモデルと共に`code/api.py`に追加

### トラブルシューティング
- Azureサービスが失敗する場合は`.env`設定を確認
- Azure OpenAIモデルのデプロイが環境変数と一致することを確認
- 適切なconda環境のアクティベーションを確認
- 処理失敗時はAzureサービスのクォータと権限を確認

## デプロイメント

システムは以下を使用したAzureデプロイ向けに設計されています：
- WebアプリケーションのためのAzure Container Instances/Apps
- 大量文書処理のためのAzure Machine Learning
- 含まれるDockerfileを持つDockerコンテナ
- `/deployment`のPowerShellデプロイメントスクリプト