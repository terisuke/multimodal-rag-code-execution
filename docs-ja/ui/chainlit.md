# Research CoPilot v1へようこそ

このドキュメントは、Research CoPilotソリューション用のテストツールで利用可能な主要コマンドとオプションについて説明しています。

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