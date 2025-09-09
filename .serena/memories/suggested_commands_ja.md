# 推奨開発コマンド

## 環境セットアップ
```bash
# conda環境を作成
conda create -n mmdoc python=3.10
conda activate mmdoc

# 依存関係をインストール
pip install -r requirements.txt  # code/コンポーネント用
pip install -r ui/requirements.txt  # UIコンポーネント用
```

## アプリケーションの実行

### APIサーバー（最初に実行必須）
```bash
cd code
python -m uvicorn api:app --reload --port 9000
cd ..
```

### Chainlit Webアプリ（メインチャットインターフェース）
```bash
cd ui
chainlit run chat.py
```

### Streamlit Webアプリ（文書取り込みとプロンプト管理）
```bash
cd ui
streamlit run main.py
```

## 開発コマンド
```bash
# テストの実行
pytest

# コードフォーマット（pre-commitが設定されている場合）
pre-commit run --all-files

# 依存関係のチェック
pip check
```

## 設定
- `.env.sample`を`.env`にコピーして、必要な値をすべて設定
- ローカル開発の場合、.envに`API_BASE_URL=http://localhost:9000`を設定
- Azure OpenAI、Cognitive Search、その他のAzureサービスを必要に応じて設定

## Docker開発
```bash
# イメージのビルドとプッシュ（PowerShellが必要）
deployment/push.ps1
```

## TaskWeaverセットアップ（オプション）
```bash
git clone https://github.com/microsoft/TaskWeaver.git
cd TaskWeaver
pip install -r requirements.txt
cp -r project ../test_project/
# test_project/内のtaskweaver_config.jsonを設定
```