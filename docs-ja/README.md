> **注記:**
> まず、tutorialsフォルダのチュートリアルノートブック[こちら](tutorials/)から始めてください。

<br/>

# Research CoPilot: マルチモーダルRAGとコード実行
マルチモーダル文書解析とRAG・コード実行：GPT4-V、TaskWeaver、Assistants APIを使用したテキスト、画像、データ表の処理:

1. **「マルチモーダルデータとのチャット」**: マルチモーダル文書（テキスト、表、画像）を自動的に取り込み、解析し、検索可能なセマンティックコンテンツを生成するGenAIソリューションを実装。
1. **コンテンツ生成**: このソリューションは、メモやPowerPointプレゼンテーションなど、取り込んだデータに関する重要な事実を強調する標準形式に必要なほとんどのコンテンツを出力可能。
1. **OpenAI Assistants APIとTaskweaverによる分析クエリ**: コードインタープリターと統合されたチャットインターフェースを使用してマルチモーダルデータと対話できる高度な機能を開発。このツールは、計算やオンザフライでのグラフ・ファイル生成を含む複雑な分析クエリをサポート。

## デプロイメント手順（Azureポータル）

1. "Deploy to Azure"ボタンをクリック

    [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/?feature.customportal=false#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fmultimodal-rag-code-execution%2Fmain%2Fdeployment%2Finfra-as-code-public%2Fbicep%2Fmain-1click.json)

1. パラメーターを入力

    このデプロイメントには特別なパラメーターはありません。デプロイメントをカスタマイズするためのオプションパラメーターが利用可能です（下記参照）。通常、既存のAzure OpenAIリソースを再利用するために`openAIName`と`openAIRGName`のみが使用されます。

    既存のコンテナレジストリが設定されていない場合、平均デプロイメント時間は10分です。

### カスタマイゼーション
追加のデプロイメントカスタマイゼーションについては、[Deployment README](deployment/README.md)ガイドの詳細な手順に従ってください。

<br/>
<br/>

## Research Copilot YouTube動画
<p align="center">

[<img src="images/Snapshot.PNG" width="30%">](https://youtu.be/Si4m-Zl9xvQ)

</p>
<br/>
<br/>

## 説明
1. この作業は、データ表現と情報抽出を最大化するために、テキスト、画像、データ表を抽出してマルチモーダル分析文書を処理することに焦点を当て、GPT-4モデルとの互換性のためにPythonコード、Markdown、Mermaidスクリプトなどの形式を活用します。
1. テキストはプログラムで文書から抽出され、より良い検索性のための構造とタグ抽出を改善するように処理され、数値データは後で使用するために生成されたPythonコードを通じて取得されます。
1. 画像とデータ表は、情報が検索可能で計算、予測、機械学習モデルの適用にコードインタープリター機能を使用できるように、複数のテキストベース表現（詳細なテキスト説明、Mermaid、画像用Python コード、表用の様々な形式を含む）を生成するように処理されます。

<br/>
<br />

## 現在の課題
1. 従来の技術では、RAGで知識ベースを検索できるようにするために、文書からテキストを抽出し、チャンク化してベクトルデータベースに保存する必要があります
1. このプロセスは現在、純粋にテキストに関するものです：
    * 文書に画像、グラフ、表がある場合、これらの要素は通常無視されるか、煩雑な非構造化テキストとして抽出されます
    * RAGを通じて非構造化表データを取得すると、非常に低い精度の答えになります
1. LLMは通常数字が非常に苦手です。クエリが何らかの計算を必要とする場合、LLMは通常幻覚を見たり、基本的な数学的間違いを犯したりします

<br/>

## なぜこのソリューションが必要か？

1. グラフ、数字、表がたくさんあるマルチモーダル分析文書を取り込み、対話する
1. 以前は不可能だった文書内の一部の要素から構造化情報を抽出：
    * 画像
    * グラフ
    * 表
1. 検索結果に基づいて計算が必要な答えを定式化するためにコードインタープリターを使用

<br/>

## 業界アプリケーションの例

1. プライベートエクイティ案件の投資機会文書の分析
1. 監査目的での税務文書からの表の分析
1. 財務諸表の分析と初期計算の実行
1. マルチモーダル製造文書の分析と対話
1. 学術・研究論文の処理
1. 教科書、マニュアル、ガイドの取り込みと対話
1. 交通・都市計画文書の分析

<br/>
<br/>

# ソリューション機能

このソリューションの一部として実装された技術的機能は以下の通りです：

1. サポートされているファイル形式は、PDF、MS Wordドキュメント、MS Excelシート、csvファイルです。
1. 画像と表を含むマルチモーダル文書の取り込み
1. 信頼性の高い長時間実行と監視のためのAzure Machine Learningでの取り込みジョブの実行
1. Azureでソリューションコンポーネントを作成し、Webアプリ用のDockerイメージをビルドする完全なデプロイメントスクリプト
1. ベクトルおよびキーワード検索、セマンティック再ランカーを使用したAI Searchでのハイブリッド検索
1. キーワード検索を最適化するためのチャンクレベルタグと文書全体レベルチャンクの抽出
1. 追加コンテキストを提供するため最終検索プロンプトの一部として使用される文書全体の要約
1. OpenAI Assistants APIコードインタープリターでのコード実行
1. 長い生成プロンプト用の最適化のためのタグベース検索
1. カスタマイズ可能な処理パイプライン用のプロセッサーのモジュラーで使いやすいインターフェース
1. 反復可能なヘッダーと各チャンクでの表要約を持つMarkdown表のスマートチャンク化
1. `text-embedding-3-small`と`text-embedding-3-large`の新しい埋め込みモデル、および`text-embedding-ada-002`のサポート

### 開発中の今後の機能

1. 近似的な固定サイズチャンクでの動的セマンティックチャンク化（近日公開）
1. データ取得の向上のためのグラフDB サポート。グラフDBはAI Searchリソースを補完し、置き換えるものではありません。

<br/>
<br/>

# 処理パイプラインとプロセッサーのコンセプト

拡張可能なモジュラーアーキテクチャを提供するために、このアクセラレーターで処理パイプラインのコンセプトを実装しました。各文書は事前に指定された数の処理ステップを経て、各ステップが文書にある程度の変更を加えます。プロセッサーはフォーマット固有（PDF、MS Word、Excelなど）で、そのフォーマットに最も効率的な方法でマルチモーダル文書を取り込むように作成されます。したがって、PDFの処理ステップのリストはExcelシートのステップのリストとは異なります。これは`processor.py`Pythonファイルで実装されています。処理ステップのリストは`processing_plan.json`ファイルを変更することでカスタマイズできます。例として、Excelファイルの処理は以下のステップに従い、各ステップは前のステップの結果に基づいて構築されます：

1. `extract_xlsx_using_openpyxl`: OpenPyxlでExcelシートを読み、データフレームに保存。
1. `create_table_doc_chunks_markdown`: データフレームをMarkdownに変換した後、スマートな方法でテキストにチャンク化：ほぼ同じサイズのチャンクですが、途中で文を壊すことはありません。
1. `create_image_doc_chunks`: もしあればExcelから画像を抽出
1. `generate_tags_for_all_chunks`: テキストの各チャンクに対してタグを生成。これはAI Searchでのハイブリッド検索にとって非常に重要です。
1. `generate_document_wide_tags`: 文書全体のタグを生成。これはAI Searchでのハイブリッド検索にとって非常に重要です。
1. `generate_document_wide_summary`: RAGのコンテキストと上位チャンクに挿入される文書要約を提供。
1. `generate_analysis_for_text`: 文書全体に関連する各テキストチャンクの分析を提供、例えば、文書全体のテキストに対してチャンクが情報として何を追加するか。

処理パイプラインの開始時に、すべての入力パラメーターを持つ`ingestion_pipeline_dict`というPython辞書変数がプロセッサーのコンストラクターで作成され、最初のステップに渡されます。ステップは独自の処理を行い、`ingestion_pipeline_dict`内の変数を変更し、新しいものを追加します。その後、`ingestion_pipeline_dict`はこの最初のステップによって返され、次に2番目のステップの入力になります。このように、`ingestion_pipeline_dict`は各ステップから次のパイプライン下流のステップに渡されます。これはすべてのステップが作業する共通のコンテキストです。`ingestion_pipeline_dict`は各ステップの終了時にテキストファイルに保存されるため、`stages`ディレクトリの処理フォルダー名の下でデバッグとトラブルシューティングの方法を提供します。

これはパイプラインの視覚的表現です：

<br />
<p align="center">
<img src="images/pipelines.png" width="800" />
</p>
<br/>

<br/>

この文書の最後に、すべてのステップとそれぞれの短い説明の[リスト](#処理ステップ)があります。以下のJSONブロックは、処理オプションごとの文書形式ごとの処理パイプラインを記述します：
<br/>

```json
{
    ".pdf": {
        "gpt-4-vision": [
            "create_pdf_chunks", "pdf_extract_high_res_chunk_images", "pdf_extract_text", "pdf_extract_images", "delete_pdf_chunks", "post_process_images", "extract_tables_from_images", "post_process_tables", "generate_tags_for_all_chunks", "generate_document_wide_tags", "generate_document_wide_summary", "generate_analysis_for_text"
        ],
        "document-intelligence": [
            "create_pdf_chunks", "pdf_extract_high_res_chunk_images", "pdf_extract_text", "pdf_extract_images", "delete_pdf_chunks", "extract_doc_using_doc_int", "create_doc_chunks_with_doc_int_markdown", "post_process_images", "generate_tags_for_all_chunks", "generate_document_wide_tags", "generate_document_wide_summary", "generate_analysis_for_text"
        ],
        "hybrid": [
            "create_pdf_chunks", "pdf_extract_high_res_chunk_images", "delete_pdf_chunks", "extract_doc_using_doc_int", "create_doc_chunks_with_doc_int_markdown", "post_process_images", "post_process_tables", "generate_tags_for_all_chunks", "generate_document_wide_tags", "generate_document_wide_summary", "generate_analysis_for_text"
        ]
    },
    ".docx": {
        "py-docx": [
            "extract_docx_using_py_docx", "create_doc_chunks_with_doc_int_markdown", "post_process_images", "generate_tags_for_all_chunks", "generate_document_wide_tags", "generate_document_wide_summary", "generate_analysis_for_text"
        ],
        "document-intelligence": [
            "extract_doc_using_doc_int", "create_doc_chunks_with_doc_int_markdown", "post_process_images", "generate_tags_for_all_chunks", "generate_document_wide_tags", "generate_document_wide_summary", "generate_analysis_for_text"
        ]
    },
    ".xlsx": {
        "openpyxl": [
            "extract_xlsx_using_openpyxl", "create_table_doc_chunks_markdown", "create_image_doc_chunks", "generate_tags_for_all_chunks", "generate_document_wide_tags", "generate_document_wide_summary", "generate_analysis_for_text"
        ]
    }
}
```

<br/>

## ソリューションアーキテクチャ

以下はこのソリューションの論理アーキテクチャです。GraphDBはまだソリューションに追加されていませんが、統合は現在開発中です：

<br />
<p align="center">
<img src="images/arch.png" width="800" />
</p>
<br/>

<br/>

## 重要な発見

1. GPT-4-Turboは128kトークンの大きなウィンドウで非常に有用
1. GPT-4-Turbo with Visionは非構造化文書形式からの表の抽出に優れている
1. GPT-4モデルは様々な形式（Python、Markdown、Mermaid、GraphViz DOTなど）を理解でき、これは情報抽出の最大化に不可欠でした
1. 生成プロンプトが通常のユーザークエリと比較して非常に長いため、タグに基づくベクトルインデックス検索の新しいアプローチが必要でした
1. オープンエンドの分析質問を行うためにTaskweaverとAssistants APIのコードインタープリターが導入されました

<br/>
<br/>

# エンタープライズデプロイメント

これを安全にクライアントのテナントにデプロイする方法については、[エンタープライズデプロイメント](ENTERPRISE_DEPLOYMENT.md)ガイドをご確認ください。ソリューションのローカル開発やテスト用には、以下で説明するチュートリアルノートブックまたはChainlitアプリを使用してください。

<br/>
<br/>

# チュートリアルノートブック

[こちら](tutorials/)のチュートリアルノートブックから始めてください。これらのノートブックは、このリポジトリで使用されている一連のコンセプトを説明します。

<br/>
<br/>

# このソリューションの使用方法

このソリューションの一部として実装されている2つのWebアプリがあります。StreamlitWebアプリとChainlit Webアプリです。

1. Streamlit Webアプリには以下が含まれます：
    * WebアプリはAzure Machine Learning（推奨）またはWebアプリ自体でのPythonサブプロセスを使用して（ローカルテスト専用）取り込みジョブを作成する文書を取り込むことができます。
    * Streamlitアプリの2番目の部分は生成です。"プロンプト管理"ビューにより、ユーザーはサブセクションを含む複雑なプロンプトを構築し、Cosmosに保存し、ソリューションを使用してこれらのプロンプトに基づいて出力を生成できます

1. Chainlit Webアプリは取り込んだ文書とチャットするために使用され、検索の監査証跡、マルチモーダルサポート（画像と表を表示可能）付きの答えの参照セクションなどの高度な機能を持ちます。

<br/>

## ローカルConda環境の準備

Conda環境は**プロジェクトルートフォルダー**から以下のコマンドを実行してインストールできます。以下のコマンドに従って**新しい**conda環境を作成してください。Pythonバージョンは>= 3.10が可能です（ただし3.10で十分にテストされています）：

```bash
# conda環境を作成
conda create -n mmdoc python=3.10

# conda環境をアクティベート
conda activate mmdoc

# プロジェクト要件をインストール
pip install -r requirements.txt
```

## .envファイルの準備

`.env`ファイルを適切に設定してください。このソリューションに含まれる`.env.sample`ファイルを参照してください。このソリューションが適切に機能するためには、すべての非オプション値を入力する必要があります。

.envファイルは以下の用途に使用されます：

1. 必要に応じてローカル開発
1. デプロイメントスクリプトは`.env`ファイルから値を読み取り、両方のWebアプリの設定変数を設定します。
1. ソリューションをローカルで実行するために、.envに以下の行を追加してください：
    `API_BASE_URL=http://localhost:9000`

<br/>

## API Webアプリの実行

API Webアプリは他の2つのWebアプリの**前に**実行する必要があります。他の2つのWebアプリのAPIインフラストラクチャを提供します。Webアプリをローカルで実行するには、conda環境で以下を実行してください：

```bash
# codeフォルダーに移動
cd code

# chainlitアプリを実行
python -m uvicorn api:app --reload --port 9000

# プロジェクトルートフォルダーに戻る
cd ..
```
<br/>

## Chainlit Webアプリの実行

Chainlit WebアプリはデータとチャットするためのメインWebアプリです。Webアプリをローカルで実行するには、conda環境で以下を実行してください：

```bash
# プロジェクトルートフォルダーにいることを確認してから、uiフォルダーに移動
cd ui

# chainlitアプリを実行
chainlit run chat.py
```
<br/>

## Streamlit Webアプリの実行

Streamlit Webアプリは文書を取り込み、生成用のプロンプトを構築するためのメインWebアプリです。Webアプリをローカルで実行するには、conda環境で以下を実行してください：

```bash
# uiフォルダーに移動
cd ui

# chainlitアプリを実行
streamlit run main.py
```
<br/>

### ChainlitとStreamlit Webアプリの設定ガイド

1. `.env`ファイルを適切に設定してください。このソリューションに含まれる`.env.sample`ファイルを参照してください。
1. Chainlit Webアプリで、`cmd index`を使用してインデックス名を設定してください。

<br/>
<br/>

### Azure Cloud用ローカル開発

迅速な開発反復とクラウドでのテストのために、`push.ps1`スクリプトを使用してDockerイメージのみをビルドし、Azure Container Registryにプッシュできます。リソースグループまたはアーキテクチャ内の他のコンポーネントを作成または変更することはありません。その後、Dockerイメージを**手動で**Webアプリに割り当てる必要があります。AzureポータルのWebアプリページに移動し、左側の`Deployment > Deployment Center`に移動し、右側の`Settings`に移動してから、`Tag`ドロップダウンに移動して正しいDockerイメージを選択します。

`push.ps1`スクリプトを編集し、Azure Container Registryエンドポイント、ユーザー名、パスワード、リソースグループ名、サブスクリプションIDの正しい値を入力してください。その後、スクリプトを実行するには、`Powershell`で以下の指示に従ってください。その時点でDocker Desktopバージョンがローカルにインストールされ、実行されていることが重要です。コマンドはプロジェクトのルートディレクトリから実行する必要があります：

```bash
# プロジェクトのルートフォルダーに移動
cd <project root>

# dockerイメージ更新スクリプトを実行
deployment/push.ps1
```

<br />
<p align="center">
<img src="images/pushacr.png" width="800" />
</p>
<br/>

<br/>

## コードインタープリター

このソリューションで利用可能なコードインタープリター：
1. Assistants API: OpenAI AssistantsAPIは、Azureで実行されるこのソリューションのデフォルトの標準コードインタープリターです。
1. Taskweaver: インストールと使用はオプションで、完全にサポートされています

<br/>

<br/>

## Taskweaverインストール（オプション）

TaskWeaverには**Python >= 3.10**が必要です。プロジェクトルートフォルダーから以下のコマンドを実行してインストールできます。以下のコマンドに**非常に注意深く**従い、**新しい**conda環境を作成することから始めてください：

```bash
# conda環境を作成
conda create -n mmdoc python=3.10

# conda環境をアクティベート
conda activate mmdoc

# プロジェクト要件をインストール
pip install -r requirements.txt

# リポジトリをクローン
git clone https://github.com/microsoft/TaskWeaver.git

# Taskweaverに移動
cd TaskWeaver

# Taskweaver要件をインストール
pip install -r requirements.txt

# Taskweaverプロジェクトディレクトリをルートフォルダーにコピーし、'test_project'と名前を付ける
cp -r project ../test_project/
```

<br/>

> **注記:**
> `test_project`ディレクトリ内に、入力が必要な`taskweaver_config.json`というファイルがあります。このリポジトリのルートフォルダーにある`taskweaver_config.sample.json`ファイルを参照し、GPT-4-TurboのAzure OpenAIモデル値を入力し、`taskweaver_config.json`に名前を変更してから、`test_project`内にコピー（または既存のものを上書き）してください。

<br/>

> **注記:**
> 同様に、このソリューションにはAutogenを使用する多数のテストノートブックがあります。ユーザーがAutogenで実験したい場合、この場合、`code`フォルダーの`OAI_CONFIG_LIST`ファイルを設定する必要があります。`OAI_CONFIG_LIST.sample`を参照し、正しい値で入力してから、`OAI_CONFIG_LIST`に名前を変更してください。

<br/>
<br/>

# 処理ステップ

`create_doc_chunks_with_doc_int_markdown`関数は、特にDocument Intelligenceサービスを利用する場合の文書処理にとって不可欠です。文書チャンクのMarkdown変換を処理するように設計されており、抽出されたデータがさらなる分析のために正しくフォーマットされることを保証します。この関数は様々な文書形式に適用可能で、テキスト、画像、表を処理でき、マルチモーダル情報抽出プロセスにおいて多用途です。その役割は、生の抽出データをよりアクセスしやすく分析可能な形式に構造化することで重要です。

`create_image_doc_chunks`関数は、マルチモーダル文書内の画像データの処理にとって不可欠です。特に画像関連コンテンツの抽出と組織化をターゲットにし、各画像をさらなる分析のための個別のチャンクとしてセグメント化します。この関数は画像データを含む様々な文書形式に適用可能で、視覚情報が正確にキャプチャされ、タグ付けや分析などの後続の処理ステップのために準備されることを保証することで、マルチモーダル抽出パイプラインで重要な役割を果たします。視覚コンテンツの処理を合理化するために、テキストや表から分離して画像モダリティのみを扱います。

`create_pdf_chunks`関数は、特にPDFファイルの文書取り込みプロセスの重要なステップです。入力PDF文書を個々のチャンクに分割し、パイプラインの後続段階で個別に処理されます。この関数はテキスト、画像、表を含むPDF文書内のすべてのモダリティに適用可能で、詳細な分析と抽出のための文書コンテンツの包括的な分解を保証します。その役割は基礎的で、他の関数による各モダリティの特化した処理の舞台を設定します。

`create_table_doc_chunks_markdown`関数は、文書内の表の処理、特にMarkdown形式への変換を担当します。`openpyxl`パイプラインの一部として`.xlsx`ファイルに適用されます。この関数は変換を処理するだけでなく、表が大きすぎる場合のチャンク化も管理し、Markdown表現が正確で管理しやすいことを保証します。表モダリティのみを処理し、文書取り込みプロセス中の表の構造とデータの保持に重要です。

`delete_pdf_chunks`関数は、特にPDFファイルの文書処理パイプラインの重要なステップです。メモリからPDFチャンクの一時ストレージを削除することで、システムリソースが効率的に管理され、不要なデータで負荷をかけられないことを保証します。この関数は、PDF文書から高解像度画像とテキストの初期抽出後、画像や表の後処理前に適用されます。PDFチャンクから抽出されたデータのクリーンアップを扱うため、すべてのモダリティ（テキスト、画像、表）に適用されます。

`extract_doc_using_doc_int`関数は、特に`.docx`と`.pdf`ファイルを処理するために調整された文書処理パイプラインの主要コンポーネントです。AzureのDocument Intelligence Serviceの機能を活用して文書から構造化データ（テキストと表を含む）を分析・抽出します。この関数は、文書コンテンツを洞察のためにさらに処理できる形式に変換するために重要で、テキストと表の両方のデータモダリティを扱うのに多用途です。

`extract_docx_using_py_docx`関数は、`.docx`ファイルからのコンテンツ抽出を処理するように設計されており、特にテキスト、画像、表に焦点を当てています。`python-docx`ライブラリを活用してこれらの要素にアクセス・抽出し、データが正確に取得され、さらなる処理に適した構造化形式で保存されることを保証します。この関数は取り込みパイプラインの初期段階で重要で、後続の分析と処理ステップの基礎を設定します。`.docx`ファイルに適用され、文書からテキスト、画像、表の3つのモダリティすべての抽出を担当します。

`extract_tables_from_images`関数は、文書内の画像ファイルから表を識別・抽出するように設計されています。画像モダリティ、特に画像ファイル内に埋め込まれた表などの視覚データ表現をターゲットにします。この関数は、視覚表データを構造化形式に変換してさらに処理や分析できるようにするために重要で、テキストと視覚情報の両方を扱うマルチモーダル文書処理パイプラインにおいて不可欠なステップです。表形式の情報が非テキスト形式で提示される文書に特に関連しています。

`extract_xlsx_using_openpyxl`関数は、`.xlsx`ファイルからのデータ抽出を処理するように設計されており、特に表の取得とさらなる処理のための様々な形式への変換に焦点を当てています。`openpyxl`ライブラリを活用してExcelファイルにアクセス・操作し、抽出されたデータがDataFrameなどのPythonフレンドリーな構造で正確に表現されることを保証します。この関数は、構造化情報が豊富なスプレッドシートデータの解析に重要で、取り込みパイプライン内の`.xlsx`ファイルのデータ抽出フェーズの主要ステップです。表モダリティを処理し、ExcelシートをMarkdown、プレーンテキスト、Pythonスクリプトに変換し、マルチモーダル情報抽出フレームワークに統合できるようにします。

`generate_analysis_for_text`関数は、特定のテキストチャンクと文書全体のコンテンツとの関係を分析するように設計されています。テキストチャンクで導入または拡張されたエンティティ関係を強調し、文書のトピックにコンテキストを追加する簡潔な分析を提供します。この関数はすべてのモダリティ（テキスト、画像、表）に適用可能で、文書コンテンツの包括的理解を保証します。より大きな文書構造内での各セクションの重要性についての洞察を提供することで、文書のメタデータの向上に重要な役割を果たします。

`generate_document_wide_summary`関数は、文書全体のコンテンツの簡潔な要約を作成することを担当します。主要情報を抽出し、不要な詳細なしに文書の本質が捉えられるように数段落で提示します。この関数はテキスト、画像、表を含むすべての文書形式に適用可能で、マルチモーダル情報抽出パイプラインの多用途コンポーネントです。インデックス作成と検索の両方の目的に有益な文書の素早い概要を提供する重要な役割を果たします。

`generate_document_wide_tags`関数は、PDF、DOCX、XLSXを含む様々な文書形式に適用可能な文書取り込みパイプラインの重要なコンポーネントです。文書全体から主要タグを抽出することを担当し、検索と取得機能の向上に不可欠です。この関数はテキストモダリティを処理し、文書内の重要なエンティティとトピックがタグとしてキャプチャされることを保証し、取り込まれたコンテンツの検索可能なインデックスの作成を支援します。

`generate_tags_for_all_chunks`関数は、PDF、DOCX、XLSXを含む様々な文書形式に適用可能なマルチモーダル情報抽出プロセスにとって不可欠です。3つすべてのモダリティ（テキスト、画像、表）を操作し、ベクトルストア内での向上した検索と取得のためにタグを抽出・最適化します。この関数は、コンテンツタイプに関係なく、文書の各チャンクが記述的タグのセットで正確に表現され、効率的なインデックス作成と後続の検索操作を促進することを保証します。

`pdf_extract_high_res_chunk_images`関数は、PDF文書の各チャンクから高解像度画像を抽出することを担当します。特にPDF形式の文書処理パイプラインの初期段階で重要な役割を果たし、後続の分析のために視覚データが詳細にキャプチャされることを保証します。この関数は画像モダリティに焦点を当て、文書チャンクを300 DPIのPNG画像に変換し、さらなる画像ベース処理タスクに使用されます。

`pdf_extract_images`関数は、PDF文書からの画像抽出を処理するように設計されています。PDF形式に適用され、マルチモーダル抽出コンテキスト内で動作し、特に画像モダリティを処理します。この関数は、PDFから視覚コンテンツを分離するという重要な役割を果たし、これはより広いマルチモーダル情報抽出プロセスでの後続の画像分析と理解に不可欠です。

`pdf_extract_text`関数は、特にPDFファイルを処理するために調整された文書処理パイプラインの重要なコンポーネントです。PDF文書の各ページからテキストコンテンツを抽出し、機械可読形式に変換することを担当します。この関数は、テキスト分析、検索インデックス作成、またはさらなるデータ抽出タスクを含む可能性がある後続段階にとって極めて重要です。テキストモダリティのみを操作し、PDF内に埋め込まれた豊富なテキスト情報が正確にキャプチャされ、下流処理に利用できるようになることを保証します。

`post_process_images`関数は、文書取り込みプロセス内の画像抽出操作からの出力を洗練するために不可欠です。特に画像の向上と明確化を処理し、視覚データが正確に表現され、後続の分析に使用できることを保証します。この関数は画像コンテンツを含む様々な文書形式に適用可能で、視覚データが主要コンポーネントであるマルチモーダル情報抽出において極めて重要な役割を果たします。画像をモダリティとして扱うように設計されており、テキストと表を処理する他の関数を補完します。

`post_process_tables`関数は、文書から抽出された表データの洗練を処理するように設計されています。表が存在するPDFや画像を含む様々な文書形式に適用されます。この関数の役割は、抽出された表情報の品質を向上させ、さらなる使用のために正確に表現・フォーマットされることを保証することです。特に「表」モダリティを扱い、検索可能なベクトルインデックスへの統合や分析計算のために表の後抽出処理に焦点を当てます。

<br/>
<br/>

# 貢献 - クレジット

**コアソリューション開発**:
* Agustin Mantaras Rodriguez – コア機能、インフラ、DevOps
* Angel Sevillano Cabrera - オーディオプロセッサー
* Mohamed Benaichouche – ユーザーインターフェース
* Riccardo Chiodaroli - APIサーバー、コードリファクタリング、アプリ開発
* Samer El Housseini – コア機能
* Wolfgang Knupp - デプロイメントテスト

<br/>
<br/>

**アドバイザー**:
* Andrew Mackay
* Fabrizio Ruocco
* Sergio Gonzalez Jimenez

<br/>
<br/>

**チュートリアルノートブック開発**:
* Ali Soliman - HTMLスクレイピング
* Evgeny Minkevich - セマンティックチャンク化
* Khalil Chouchen - Document Intelligence
* Sasa Juratovic - 表の分割と結合

<br/>
<br/>

**貢献ディスカッション**:
* Anna Kubasiak
* Clemens Siebler
* Farid El Attaoui
* Hooriya Anam
* Masha Stroganova
* Mooketsi Gaboutloeloe
* Nabila Charkaoui

<br/>
<br/>