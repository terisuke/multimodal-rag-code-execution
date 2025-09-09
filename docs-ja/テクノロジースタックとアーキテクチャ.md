# テクノロジースタックとアーキテクチャ

## 主要技術
- **Python 3.10+** - メイン開発言語
- **FastAPI** - APIサーバーフレームワーク
- **Chainlit** - チャットインターフェースフレームワーク
- **Streamlit** - 文書取り込みとプロンプト管理UI
- **OpenAI GPT-4/GPT-4V** - 処理と分析用言語モデル
- **Azure OpenAI** - クラウドホスト型OpenAIモデル
- **Azure Cognitive Search** - ベクトルおよびハイブリッド検索
- **Azure Document Intelligence** - 文書処理サービス

## 主要依存関係
- **文書処理**: pypdf、pdf2image、python-docx、openpyxl、pdfplumber
- **AI/ML**: openai、tiktoken、llama-index、azure-ai-*
- **Webフレームワーク**: uvicorn、gunicorn、fastapi、chainlit、streamlit
- **データ**: pandas、numpy、matplotlib、plotly
- **Azureサービス**: azure-search-documents、azure-cosmos、azureml-core

## アーキテクチャコンポーネント

### 処理パイプラインシステム
- **プロセッサークラス**: 形式固有のサブクラス（`PdfProcessor`、`DocxProcessor`、`XlsxProcessor`、`AudioProcessor`）を持つベース`Processor`
- **処理プラン**: `processing_plan.json`の文書タイプごとのステップシーケンスを定義するJSON設定
- **モジュラーステップ**: 各処理ステップは`ingestion_pipeline_dict`を操作する個別の関数

### 文書処理モード
- **PDF**: gpt-4-vision、document-intelligence、hybrid
- **DOCX**: py-docx、document-intelligence
- **XLSX**: openpyxl

### Webアプリケーション構造
- **APIサーバー** (`code/api.py`): RESTエンドポイントを提供するFastAPIバックエンド
- **チャットUI** (`ui/chat.py`): Chainlitベースの会話インターフェース
- **管理UI** (`ui/main.py`): Streamlitベースの文書取り込みとプロンプト管理

### 主要な処理ステップ
1. 文書のチャンク化と抽出（テキスト、画像、表）
2. GPT-4Vを使用したマルチモーダルコンテンツ処理
3. ハイブリッド検索最適化のためのタグ生成
4. Azure Cognitive Searchでのベクトル埋め込みとインデックス作成
5. 要約と分析の生成

### ストレージとインデックス作成
- **Azure Cognitive Search**: ハイブリッド検索を持つプライマリベクトルストア
- **Azure Cosmos DB**: プロンプトストレージとログ記録
- **Azure File Share**: 文書と処理成果物のストレージ
- **ローカル処理**: `ingestion/`ディレクトリでの一時ステージング

## デプロイメント
- **Azure Container Instances/Apps**: 本番環境ホスティング
- **Docker**: コンテナ化されたアプリケーション
- **Azure Machine Learning**: 大規模文書セットのバッチ処理ジョブ
- **PowerShellスクリプト**: デプロイメント自動化