# 1クリックデプロイメント

このソリューションをデプロイする新しい方法は、1クリックデプロイメントを使用することです。これがこのソリューションをデプロイする推奨方法です。

## 前提条件

- Azureサブスクリプションを持ち、その中でリソースを作成できる必要があります。少なくともリソースグループが必要で、ユーザーはそれに対する`Owner`権限を持っている必要があります。

## デプロイメント手順（Azureポータル）

1. "Deploy to Azure"ボタンをクリック

    [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/?feature.customportal=false#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fmultimodal-rag-code-execution%2Fmain%2Fdeployment%2Finfra-as-code-public%2Fbicep%2Fmain-1click.json)

1. パラメーターを入力

    このデプロイメントには特別なパラメーターはありません。デプロイメントをカスタマイズするためのオプションパラメーターが利用可能です（下記参照）。通常、既存のAzure OpenAIリソースを再利用するために`openAIName`と`openAIRGName`のみが使用されます。

    既存のコンテナレジストリが設定されていない場合、平均デプロイメント時間は10分です。

## カスタマイゼーション

以下のパラメーターがカスタマイゼーションに利用できます：

- `openAIName`と`openAIRGName`: 新規作成の代わりに再利用するAzure OpenAIリソースの名前とリソースグループ。
レジストリが作成され、GitHubリポジトリをクローンしてイメージがビルドされ、プッシュされます。
- `namePrefix`: デプロイメントによって作成されるすべてのリソースのプレフィックス。デフォルトは`dev`。
- `newOpenAILocation`: 新しいAzure OpenAIリソースの場所。デフォルトは空で、`openAIName`が空白でない限り無視されます。
- `skipImagesBuild`: リモートリポジトリからイメージをビルドしない場合は`True`。デフォルトは`False`。`True`に設定すると、デプロイメントはコンテナレジストリを作成せず、手動でイメージをビルドする必要があります（下記参照）。
- `useWebApps`: イメージをホストするためにContainer Appの代わりにAzure WebAppsをプロビジョンする場合は`True`。デフォルトは`False`。

## デプロイメント手順（ローカル）

1. GitHubリポジトリをクローン

    ```bash
    git clone https://github.com/Azure-Samples/multimodal-rag-code-execution

    cd multimodal-rag-code-execution
    ```

1. デプロイメントスクリプトを実行

    PowershellまたはGit Bashのいずれかで以下のコマンドを実行できます。

    ```bash
    az login
    
    az upgrade

    az bicep upgrade

    az group create --name multimodal-rag-code-execution --location <location>
    
    ## [オプション1] 既存のOPENAIリソースを使用
    ## 既存のOpenAIリソースには'gpt-4o'という名前のモデルと'text-embedding-3-large'という名前の別のモデルが必要です。OAIリソースの名前とそのリソースがあるRG名を提供してください
    az deployment group create --resource-group multimodal-rag-code-execution --template-file deployment/infra-as-code-public/bicep/main-1click.bicep --parameters aiSearchRegion=eastus openAIName=<OAI_NAME> openAIRGName=<OAI_RG_NAME>

    ## [オプション2] 新しいOPENAIリソースを作成
    az deployment group create --resource-group multimodal-rag-code-execution --template-file deployment/infra-as-code-public/bicep/main-1click.bicep --parameters aiSearchRegion=eastus newOpenAILocation=eastus

    ```

### デプロイメントのスクリーンショット

Azureポータルで、デプロイメントに移動：

<br />
<p align="center">
<img src="../images/depl-image3.png" width="800" />
</p>
<br/>

デプロイされるすべてのリソースを確認：

<br />
<p align="center">
<img src="../images/depl-image2.png" width="800" />
</p>
<br/>

### トラブルシューティング

以下のスクリーンショットが表示される場合は、API Webアプリを確認し、その環境変数をチェックして、再起動を試してください。

確認する変数：

- DOCKER_REGISTRY_SERVER_URL
- DOCKER_REGISTRY_SERVER_USERNAME
- DOCKER_REGISTRY_SERVER_PASSWORD

問題が解決しない場合は、WebAppデプロイメントセンターに移動し、以下を確認してください：

- コンテナレジストリが管理者認証情報で接続されている
- 「継続的デプロイメント」オプションが有効になっている

<br />
<p align="center">
<img src="../images/depl-image.png" width="800" />
</p>
<br/>

## Webアプリイメージの更新

Webアプリイメージを更新する方法は2つあります：

- **オプション1**: リモートgitリポジトリからの再ビルド
- **オプション2**: ローカルからの再ビルド

### オプション1: リモートgitリポジトリからの再ビルド

これは、単純に最新の機能とバグ修正を取得する必要がある場合の推奨アプローチです。

1. Azureポータルに移動してCloud Shell（Bash）を開く
1. `update_webapps_from_github.sh`スクリプトをCloud Shellにコピー
1. `chmod +x update_webapps_from_github.sh`を実行
1. `./update_webapps_from_github.sh <resource-group-name>`を実行

イメージは順次ビルドされ、コンテナレジストリにプッシュされることに注意してください。このプロセスには約10分かかります。また、スクリプトは新しいイメージを使用するためにAPI WebAppを再起動します。

### オプション2: ローカルからの再ビルド

これは、コードに変更を加えた場合、またはリモートリポジトリからビルドしないことを選択した場合にイメージを更新する推奨方法です。

1. GitHubリポジトリをクローン

    ```bash
    git clone https://github.com/Azure-Samples/multimodal-rag-code-execution

    cd multimodal-rag-code-execution
    ```

1. コードに必要なすべての変更を加える
1. PowerShellを使用して`publish-aca.ps1`スクリプトを実行（WebAppsの場合は`publish-webapp.ps1`）

    ```powershell
    # リポジトリのルートから実行することを確認
    .\deployment\publish-aca.ps1 -RG <resource-group-name>
    ```

    **注記**
    - スクリプトはAzure CLIがインストールされ、正しいサブスクリプションにログインしていることを前提としています
    - ターゲットリソースグループはデプロイメントスクリプトによって作成されている必要があります
    - イメージは順次ビルドされ、コンテナレジストリにプッシュされます。このプロセスには約10分かかります。
    - すべてのコンテナ/Webアプリは新しいイメージを使用するために再起動します

## ローカル開発

デプロイしたリソースを使用してローカルで開発したい場合は、まずサービスプリンシパルを作成し、リソースグループとAML Workspaceに対してContributor権限を割り当てる必要があります。そうしないと、APIはAML Workspaceにアクセスできません。

その後、`.env`ファイルで以下の環境変数を設定してください：

```env
AML_PASSWORD=
AML_SERVICE_PRINCIPAL_ID=
AML_TENANT_ID=
```

`set-sp-secret.sh`スクリプトを使用して、適切な権限を持つSPとシークレットを作成できます（Cloud Shellで実行可能）：

```bash
./set-sp-secret.sh <sp-name> <resource-group-name>
```