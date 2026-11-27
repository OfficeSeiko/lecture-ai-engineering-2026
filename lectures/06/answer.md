# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：
  - app.agent：search_materials
  - app.mcp_agent：glossary_search_glossary
  - 宿題実施後は glossary_get_lecture_summary も追加された。
- STEP 2 の [tool call]：
  - [tool call]   glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：
  - [tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：
  - [tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 versioning overview'})
- 気づき・疑問：
  - MCPを利用することで、エージェント本体のコードを変更せず、設定ファイルへの追記だけで外部ツールを追加できることを確認した。
  - 自作のglossaryサーバー、公開されているfetchサーバー、リモートのAWS Knowledgeサーバーという3種類の接続方法を確認できた。
  - 宿題実施後の2個のツールが、fetchの追加で3個、AWS Knowledgeの追加で8個に増加した。
  - fetchではWebページの取得と日本語での要約、AWS Knowledgeでは公式ドキュメントの検索と回答生成に成功した。
  - MCPによる標準化はAIエージェントの拡張性を高める一方、外部への情報送信やツールの実行権限、接続先の信頼性を管理する必要があると感じた。
  - 今回は宿題を先に実施し、その後に演習を実施した。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと：
  - app/mcp_server.py に server.tool()(tools.get_lecture_summary) を1行追加した。
  - 宿題の実施時点では、エージェント側のコードと設定ファイルは変更しなかった。
  - ツール一覧の表示、質問による動作確認、全テストの実行を行った。
- 結果：
  - 宿題の動作確認時点では、git status で変更されたファイルが app/mcp_server.py のみであることを確認した。
  - モデルに見えているツールが1個から2個に増えた。
  - glossary_get_lecture_summary が新たに公開された。
  - 「第2回では何を学びますか？」と質問したところ、以下のツール呼び出しを確認できた。
  - [tool call] glossary_get_lecture_summary({'lecture': 2})
  - gemini-3.5-flash-lite がツールの戻り値を利用し、第2回の講義内容を回答した。
  - モデル応答は2回、入力1,274 tokens、出力175 tokensであった。
  - pytest の結果は21 passed in 4.81sであり、すべてのテストに合格した。
- 気づき・疑問：
  - MCPを利用することで、エージェント側を変更せずに、サーバー側の変更だけで利用可能なツールを拡張できることを確認した。
  - エージェントが起動時にツール一覧を取得する仕組みにより、ツール提供側と利用側を分離できる点が興味深かった。
  - 実際の業務への導入にあたっては、ツールごとの実行権限やアクセス制御、利用可能なツールの管理方法についても検討する必要があると感じた。
