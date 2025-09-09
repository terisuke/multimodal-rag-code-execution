> **注記:**
> このリポジトリとそのREADMEは作業中です。ここでのインストールと実行の指示は（まだ）包括的でない可能性があり、コードは時々壊れる可能性があります。

> **注記:**
> PowerPointプレゼンテーション[こちら](images/RAG-CE%20Pres.pptx)をご確認ください。

> **注記:**
> デモノートブック[こちら](notebooks/demo_notebook.ipynb)から始めてください。

<br/>

# Research CoPilot: マルチモーダルRAGとコード実行
マルチモーダル文書解析とRAG・コード実行：GPT4-V、TaskWeaver、Assistants APIを使用したテキスト、画像、データ表の処理:

1. この作業は、データ表現と情報抽出を最大化するために、テキスト、画像、データ表を抽出してマルチモーダル分析文書を処理することに焦点を当て、GPT-4モデルとの互換性のためにPythonコード、Markdown、Mermaidスクリプトなどの形式を活用します。
1. テキストはプログラムで文書から抽出され、より良い検索性のための構造とタグ抽出を改善するように処理され、数値データは後で使用するために生成されたPythonコードを通じて取得されます。
1. 画像とデータ表は、情報が検索可能で計算、予測、機械学習モデルの適用にコードインタープリター機能を使用できるように、複数のテキストベース表現（詳細なテキスト説明、Mermaid、画像用Pythonコード、表用の様々な形式を含む）を生成するように処理されます。

<br/>

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

## 重要な発見

1. GPT-4-Turboは128kトークンの大きなウィンドウで非常に有用
1. GPT-4-Turbo with Visionは非構造化文書形式からの表の抽出に優れている
1. GPT-4モデルは様々な形式（Python、Markdown、Mermaid、GraphViz DOTなど）を理解でき、これは情報抽出の最大化に不可欠でした
1. 生成プロンプトが通常のユーザークエリと比較して非常に長いため、タグに基づくベクトルインデックス検索の新しいアプローチが必要でした
1. オープンエンドの分析質問を行うためにTaskweaverとAssistants APIのコードインタープリターが導入されました

<br/>
<br/>

# ソリューションステージ

このソリューションは3段階のプロセスを実装します：
1. データ抽出のための取り込みステージ
1. コード実行を伴う検索機能を可能にする検索ステージ
1. ビジネスや業界概要などのカスタムテーラード出力を作成する生成ステージ

<br />

## 取り込みプロセス

<br />
<p align="center">
<img src="images/Ingestion.png" width="800" />
</p>
<br/>

<br/>

## 検索プロセス

<br />
<p align="center">
<img src="images/Search.png" width="800" />
</p>
<br/>

<br/>
<br/>

# インストール

以下は、このソリューションのセットアップとインストール方法をユーザーに説明します。

<br/>

## Azureリソース要件
このリポジトリを使用する前に、以下のリソースを作成する必要があります：

1. Azure OpenAIリソースでのGPT-4-TurboとGPT-4-Visionモデル
1. AI Searchリソース（セマンティック検索を有効化）
1. Web Appリソース
1. Azure Visionリソース

<br/>

## インフラストラクチャとアプリケーションのデプロイメント
ユーザーには2つのオプションがあります：
1. ユーザーはChainlit Webアプリをコンピューターでローカルに使用できます
1. ユーザーはソリューションをAzure Web Appにデプロイできます。`deployment`フォルダーで、ユーザーは`deployment.sh`bashスクリプトを使用してWebアプリをデプロイできます。

## Azureアーキテクチャレビュー：

このリポジトリには、ゾーン冗長性を持つAzure App Servicesベースラインアーキテクチャと、現在（**2024年1月時点**）GPT-4 Visionモデルの実行をサポートしている4つの地域にデプロイされたAzure Open AiリソースをデプロイするためのBicepコードが含まれています。

![Deployment](https://github.com/Azure-Samples/multimodal-rag-code-execution/assets/41587804/28ebacc8-3042-47cc-8995-492a5e305df4)

## アーキテクチャコンポーネント

### ネットワーキング

- **仮想ネットワーク**: Azure内の基本的なネットワーキングバックボーン。
- **Azure Web Application Firewallを備えたApplication Gateway サブネット**: Webの脆弱性からアプリを保護します。
- **App Service統合サブネット**: Azureサービスの統合専用。
- **プライベートエンドポイントサブネット**: Azureサービスへのプライベート接続用のネットワークインターフェースをホスト。

### セキュリティとID

- **マネージドID**: セキュアなサービス間認証のためのAzure Active DirectoryIDの提供を自動化。
- **Azure Active Directory**: ユーザーIDと権限を管理。
- **Azure Key Vault**: アプリケーションのシークレット、キー、証明書をセキュア化。

### コンピューティングとストレージ

- **App Service**: アプリケーション機能を提供するすべてのユーザーインターフェースをホスト
- **App Serviceインスタンス ゾーン1、2、3**: 異なるゾーン間での高可用性を保証。
- **Azure Container Registries**: アプリケーション用のDockerコンテナイメージを保存
- **Azure Storage**: 取り込まれたすべての文書とプロンプトテンプレートをホストするスケーラブルなクラウドストレージサービスを提供。

### 監視と運用

- **Azure Monitor/Application Insights**: 高度な分析と機械学習駆動の洞察を提供。
- **プライベートDNSゾーン**: Azureネットワーク内での内部名前解決を可能にします。
- **監視**: アプリケーションとインフラストラクチャの健全性、パフォーマンス、使用状況を追跡。

## メリット

1. **セキュリティ**: プライベート接続、Bastion Host、jumpbox VM、DDoS保護、WAF、keyvaultを使用したセキュアなシークレット管理で強化。
2. **スケーラビリティ**: App Servicesは変動する負荷に動的に適応可能。
3. **可用性**: 継続的な運用を保証するため可用性ゾーン間に分散。
4. **パフォーマンス**: サービスの近接性によるルーティングの最適化と低レイテンシ操作。
5. **管理しやすさ**: 統合された監視とIDサービスによる簡素化された管理。
6. **コンプライアンス**: データを保護することで規制コンプライアンスをサポート。
7. **コスト効率**: PaaSソリューションによりオーバーヘッドと運用コストを削減。

このアーキテクチャは、ミッションクリティカルなアプリケーションに堅牢なセキュリティ、高いスケーラビリティ、中断のない可用性を必要とする組織向けに設計されています。

## 前提条件

1. [Azureアカウント](https://azure.microsoft.com/free/)があることを確認
1. デプロイメントは、User Access AdministratorまたはOwnerなど、[ロール](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles)を割り当てる十分な権限を持つユーザーによって開始される必要があります。
1. [Azure CLI がインストールされている](https://learn.microsoft.com/cli/azure/install-azure-cli)ことを確認
1. bicepコードはbicepバージョン：**v0.26.54**でテストされています。少なくともこのバージョンがあることを確認してください。[az Bicep tools がインストールされている](https://learn.microsoft.com/azure/azure-resource-manager/bicep/install)ことを確認
1. スクリプトはデフォルトでローカルにdockerを使用してコンテナイメージをビルドするため、**Dockerをインストールする**必要があります。そうしないとWebアプリのデプロイメントが失敗します
https://docs.docker.com/desktop/install/windows-install/

> **重要！**
>
> このデプロイメントでは、ユーザーがCognitive Servicesの使用を承認する必要があります。残念ながら、今日の時点ではこれは手動で行う必要があります。
>
> Cognitive Serviceを作成したことがない場合は、以下の手順に従ってください：
>
> 1. [Azure Portal](https://portal.azure.com/)に移動
> 2. 任意のCognitive Serviceを作成
> 3. 作成プロセス中に使用条件を承認
>>

## ネットワーキング

インフラストラクチャは、プライベートエンドポイントの背後にすべてのPaaSコンポーネントをデプロイします。したがって、以下のVnetとサブネットが必要です：

| 名前                     | デフォルト値   |
|--------------------------|-----------------|
| Vnetアドレス             | '10.0.0.0/16'   |
| Application Gatewayサブネット | '10.0.1.0/24' |
| App Servicesサブネット      | '10.0.0.0/24'   |
| プライベートエンドポイントサブネット | '10.0.2.0/27'   |
| DevOpsエージェントまたはjumpbox VMサブネット     | '10.0.2.32/27'  |
| Azure Bastionサブネット     | '10.0.3.0/27'  |

## ニーズに合わせたネットワーク設定の調整：
付属のdeploy.shファイルで、以下の変数が宣言されていることがわかります：

- vnetAddressPrefix: '10.0.0.0/16'
- appGatewaySubnetPrefix: '10.0.1.0/24'
- appServicesSubnetPrefix: '10.0.0.0/24'
- privateEndpointsSubnetPrefix: '10.0.2.0/27'
- agentsSubnetPrefix: '10.0.2.32/27'

**これらを**必要な値で**調整してください**。これらの設定はIP割り当てに関するMicrosoftのベストプラクティスに従っているため、**CDRを減らさないでください**。

## Azure Open AIデプロイメントに関する注記
このソリューションは、取り込みプロセスを加速するために4つのOpen AIデプロイメントを必要とします。
2024年1月時点で、GTP4-Visionモデルのサポートされている場所は：

| サポートされている地域（2024年1月）                   |
|--------------------------|
| オーストラリア東部            |
| 日本東部 |
| スウェーデン中部      |
| スイス |

これらの地域へのデプロイ中にデプロイメントスクリプトが失敗する場合、以下の理由のいずれかが原因である可能性があります（他の理由もあります）：

1. **ケース1:** GPT4モデルのクォータが不足している。
**解決策:** モデルのより多くのクォータを要求する作業が必要です。

2. **ケース2:** いずれかの地域がGPT4-Vモデルをサポートしなくなった。
**解決策:** スクリプト実行時にサポートされている地域を調査する必要があります。

## AI検索デプロイメントに関する注記
このソリューションは検索結果を改善するためにSemantic Rankerを使用し、2024年1月時点でサポートされている場所は：

| サポートされている地域2024年1月 |
|---------------------------|
| カナダ中部            |
| カナダ東部               |
| 米国中部                |
| 米国東部                   |
| 米国東部2                 |
| 米国中北部          |
| 米国中南部          |
| 米国中西部           |
| 米国西部                   |
| 米国西部2                 |
| 米国西部3                 |

https://azure.microsoft.com/en-gb/explore/global-infrastructure/products-by-region/?products=search

**デフォルトの地域は米国東部**です。異なるバージョンを選択するには、パラメーターaiSearchRegionをdeplateに渡すだけです：

```bash
aiSearchRegion='eastus'
PREFIX=dev
MMSYS_NO_PATHCONV=1 az deployment group create --template-file ./infra-as-code/bicep/main.bicep \
    --resource-group $RESOURCE_GROUP \
        --parameters appGatewayListenerCertificate=$appGatewayListenerCertificate \
                 namePrefix=$PREFIX \
                 aiSearchRegion=$aiSearchRegion \
                 vnetAddressPrefix=$vnetAddressPrefix \
                 appGatewaySubnetPrefix=$appGatewaySubnetPrefix \
                 appServicesSubnetPrefix=$appServicesSubnetPrefix \
                 privateEndpointsSubnetPrefix=$privateEndpointsSubnetPrefix\
                 agentsSubnetPrefix=$agentsSubnetPrefix \
```

## インフラストラクチャの再デプロイに関する注記
Open AIとComputer visionはクォータを持つサービスであることを覚えておいてください。したがって、リソースグループを削除してからソリューション全体を再デプロイする場合、Open AIとComputer Visionはデフォルトでソフト削除されます。Azure Portalに移動し、Azure Open AIサービスまたはComputer Visionに移動し、削除されたリソースの管理オプションを選択して、もう使用していないリソースをパージする必要があります。

## インフラストラクチャのデプロイ

コマンドラインからインフラストラクチャをデプロイするには、以下の手順が必要です。

1. Azure CLIとBicepがインストールされているコマンドラインツールで、このリポジトリのルートディレクトリ（AppServicesRI）に移動します

1. 必要に応じてログインしてサブスクリプションを設定します

```bash
  az login
  az account set --subscription xxxxx
```

1. App gateway証明書の取得
   Azure Application Gatewayは、Azureリソースのマネージドアイデンティティを使用したAzure Key VaultでのセキュアTLSをサポートします。この構成により、標準TLSプロトコルを使用したネットワークトラフィックのエンドツーエンド暗号化が可能になります。本番システムでは、公開ルート証明機関（CA）によってバックアップされた公開署名証明書を使用します。ここでは、**デモンストレーション目的**で自己署名証明書を使用します。

   デフォルトのインストールでは、contoso.comをCNとして、パスワードなしのダミー証明書を作成します。
   >
   **これはセキュアではなく、組織は適切な証明書を提供し、組織のニーズに合わせてApplication Gateway設定を手動で実行する必要があります**
   >>
     
     bashスクリプトを作成：

   - このデプロイメントの残りの部分で使用されるドメインの変数を設定します。

     ```bash
     export DOMAIN_NAME_APPSERV_BASELINE="contoso.com"
     ```

   - クライアント向けの自己署名TLS証明書を生成します。

     :warning: このスクリプトで作成された証明書を実際のデプロイメントに使用しないでください。自己署名証明書の使用は、説明目的での使いやすさのためのみに提供されています。App Serviceソリューションでは、_開発目的であっても_、TLS証明書の調達と生涯管理に関する組織の要件を使用してください。

     ドメイン用にAzure Application GatewayによってWebクライアントに提示される証明書を作成します。
     
     **非Windowsユーザー**

     ```bash
     openssl req -x509 -nodes -days 365 -newkey rsa:2048 -out appgw.crt -keyout appgw.key -subj "/CN=${DOMAIN_NAME_APPSERV_BASELINE}/O=Contoso" -addext "subjectAltName = DNS:${DOMAIN_NAME_APPSERV_BASELINE}" -addext "keyUsage = digitalSignature" -addext "extendedKeyUsage = serverAuth"
     openssl pkcs12 -export -out appgw.pfx -in appgw.crt -inkey appgw.key -passout pass:
     ```
     **Windowsユーザー**
     1. **パラメーターの置換**: -subjを次のように --> -subj "//O=Org\CN=Name"
     2.  パスでエラーが発生する場合、**MMSYS_NO_PATHCONV=1** はbashが既存のパスを実行パスにロードしているためです。次のようにMMSYS_NO_PATHCONV=1を使用してopensslコマンドを呼び出す必要があります --> MMSYS_NO_PATHCONV=1 openssl req -x509 -rest of the comand...
     
     opensslインストールの場所でwindows_pathを**置換**します。
   
       ```bash     
     windows_path="C:\Program Files\FireDaemon OpenSSL 3\bin"
     export OPENSSL_CONF=$windows_path
     MMSYS_NO_PATHCONV=1 openssl req -x509 -nodes -days 365 -newkey rsa:2048 -out appgw.crt -keyout appgw.key -subj "\CN=${DOMAIN_NAME_APPSERV_BASELINE}//O=Contoso" -addext "subjectAltName = DNS:${DOMAIN_NAME_APPSERV_BASELINE}" -addext "keyUsage = digitalSignature" -addext "extendedKeyUsage = serverAuth"
     MMSYS_NO_PATHCONV=1 openssl pkcs12 -export -out appgw.pfx -in appgw.crt -inkey appgw.key -passout pass:
     ```

     組織からの証明書を使用したか、上記から生成したかに関係なく、後でKey Vaultに適切に保存するために証明書（`.pfx`として）をBase64エンコードする必要があります。

     ```bash
     export APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV_BASELINE=$(cat appgw.pfx | base64 | tr -d '\n')
     echo APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV_BASELINE: $APP_GATEWAY_LISTENER_CERTIFICATE_APPSERV_BASELINE
     ```

<br/>

## Taskweaverインストール
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

## コードインタープリター

このソリューションで利用可能なコードインタープリター：
1. Taskweaver: 完全にサポートされています
1. Assistants API: OpenAI AssistantsAPIは現在サポートされています。Azureバージョンはリリース時に間もなく続きます。

<br/>
<br/>

# Webアプリ

このソリューションの一部として実装されている2つのWebアプリがあります。StreamlitWebアプリとChainlit Webアプリです。

1. Streamlit Webアプリには以下が含まれます：
    * WebアプリはAzure Machine Learning（推奨）またはWebアプリ自体でのPythonサブプロセスを使用して（ローカルテスト専用）取り込みジョブを作成する文書を取り込むことができます。
    * Streamlitアプリの2番目の部分は生成です。"プロンプト管理"ビューにより、ユーザーはサブセクションを含む複雑なプロンプトを構築し、Cosmosに保存し、ソリューションを使用してこれらのプロンプトに基づいて出力を生成できます
1. Chainlit Webアプリは取り込んだ文書とチャットするために使用され、検索の監査証跡、マルチモーダルサポート（画像と表を表示可能）付きの答えの参照セクションなどの高度な機能を持ちます。

<br/>

## Chainlit Webアプリの実行

Chainlit WebアプリはデータとチャットするためのメインWebアプリです。Webアプリをローカルで実行するには、conda環境で以下を実行してください：
```bash
# appフォルダーに移動
cd ui

# chainlitアプリを実行
chainlit run chat.py
```
<br/>

## Streamlit Webアプリの実行

Streamlit Webアプリは文書を取り込み、生成用のプロンプトを構築するためのメインWebアプリです。Webアプリをローカルで実行するには、conda環境で以下を実行してください：
```bash
# appフォルダーに移動
cd ui

# chainlitアプリを実行
streamlit run main.py
```
<br/>

### ChainlitとStreamlit Webアプリの設定ガイド

1. `.env`ファイルを適切に設定してください。このソリューションに含まれる`.env.sample`ファイルを参照してください。
1. Chainlit Webアプリで、`cmd index`を使用してインデックス名を設定してください。

<br/>

### Chainlit Webアプリでサポートされているコマンド

以下は、Research CoPilotソリューション用のテストツールで利用可能な主要コマンドとオプションについて説明しています。

| **コマンド**           | **使用方法**                                                                                       |
|:--------------------- |:------------------------------------------------------------------------------------------------|
| **cmd index**         | `cmd index`と入力してAI Searchインデックスの名前を変更します。                                      |
| **cmd password**      | `cmd password`と入力してPDFパスワードを変更します（PDFがパスワード保護されている場合）。              |
| **cmd tag_limit**     | `cmd tag_limit`と入力して検索用のクエリあたりの生成タグの上限を変更します。                           |
| **cmd topN**          | `cmd topN`と入力して検索実行時に取得するトップN件の結果数を変更します。                              |
| **cmd pdf_mode**      | `cmd pdf_mode`と入力してPDF抽出モードを変更します。許可される値は'gpt-4-vision'または'document-intelligence'です。 |
| **cmd docx_mode**     | `cmd docx_mode`と入力してdocx抽出モードを変更します。許可される値は'document-intelligence'または'py-docx'です。 |
| **cmd threads**       | `cmd threads`と入力してスレッド数を変更します。取り込み中のマルチスレッド処理を可能にします。.envファイルでAZURE_OPENAI_RESOURCE_xとAZURE_OPENAI_KEY_xが適切に設定されていることを確認してください。 |
| **cmd delete_dir**    | `cmd delete_dir`と入力して取り込みが再開された場合の既存出力ディレクトリ削除を有効または無効にします。|
| **cmd ci**            | `cmd ci`と入力して使用するコードインタープリターを変更します。許可される値は"NoComputationTextOnly"、"Taskweaver"、"AssistantsAPI"、または"LocalPythonExec"です。 |
| **cmd upload**        | `cmd upload`と入力して取り込み用の文書ファイルをアップロードします。                                  |
| **cmd ingest**        | `cmd ingest`と入力してアップロードしたファイルの取り込みプロセスを開始します。                       |
| **cmd prompts**       | `cmd prompts`と入力して利用可能なすべての生成プロンプトを表示します。                               |
| **cmd gen**           | `cmd gen`と入力して既存のプロンプトから生成します。                                                 |
| **クエリ**             | プレーン英語でクエリを入力し、レスポンスを待ちます。                                               |

<br/>
<br/>