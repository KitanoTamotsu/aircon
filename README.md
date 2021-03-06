#### 開発メモ
ワークフロー
<br><img width="600" src="https://user-images.githubusercontent.com/40127279/127756738-3b43bea3-7326-4bd3-854b-d02487b4e58b.png">

### 1.NatureRemoのAPI
　突然ですが、URLでNatureRemoをコントロールすることができます
<br>　Tokenの取得方法やAPIの詳細はインターネットで調べてください
<br>　APIはcURLのPOSTで操作が可能ですのでターミナルで遊んでみてください
<br>　[NatureRemoのapi](https://swagger.nature.global)
<br>　ちなみにTokenは個々のNatureRemoへのアクセス用になるので、取扱注意です
<br>　（公開するとあなたの家電を第3者がコントロールできちゃいますよ）
### 2.export禁止変数
　Alfredではtokenのような情報はexport禁止の変数として管理できます
<br>　ワークフロー右上の[χ]をクリックして環境変数の一覧を表示させてください
<br>　インストール後には、valueが空欄になっていますので、ご自身のNatureRemoのトークンを
<br>　貼り付けてください。引用符は不要です
<br>
<br>　またNatureRemoでは家電ごとにアプライアンスIDが割り当てられますが、
<br>　今回対象のエアコンのIDもexport禁止変数としています
<br>　ご自身の環境のNatureRemoで情報を取得して設定してください
<br>　GET "https://api.nature.global/1/appliances"でアプライアンス（家電）情報が確認できます
<br>　もち、トークンは必要です
### 3.スクリプトフィルターとキーワードのハイブリッド
　airconというキーワードインプットとScriptFilterを同時に利用しています
<br>　ScriptFilterは引数なしで、NatureRemoから室温と湿度を取得してAlfred出力に表示させます
<br>　キーワードインプットは引数によって処理を振り分けています
<br>　こうすると、airconの入力で室温等の情報を表示しつつ、キーワードの入力を待つ形に
<br>　なります
<br>　
<br>　スクリプトフィルターとキーワードのハイブリッドの場合、どのようなフローにするのが
<br>　最適なのかわかっていませんが、いったんShowAlfredオブジェクトを利用して、
<br>　ariconを再帰するように設定しています
<br>　引数なしでエンターキーを押したときに途切れないようするためです
<br>
<br>　ShowAlfredオブジェクト
<br>　<img width="435" src="https://user-images.githubusercontent.com/40127279/127756841-633e02e0-9603-44e7-afc5-6c62966a2a0c.png">

### 4.入力パラメータによって処理を分岐させる（conditionalユーティリティ）
　conditionalユーティリティを開いてみてください
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/127756861-3cdd3cb6-ebb8-4223-9526-a015a9f1699c.png">
<br>　1行目の条件はパラメータがない状態を意味します（『is equal to』と『（空欄）』）
<br>　なお『then』のあとのテキスト（『再表示』）はalfredワークフローをわかりやすくする
<br>　ための表示用文字列です
<br>　ソースコードを書くような感じに見えますが勘違いしないようにご注意くださいね
<br>　2行目は引数『off』、3行目は引数『dry』の場合の分岐です
<br>　それ以外が全て冷房の処理になりますが、冷房温度の範囲のチェックはRunScriptで
<br>　行っています
### 5.室温と湿度の取得
　NatureRemoのAPIで室温と湿度を取得しています
<br>　created_atという項目に取得した日時があるので、サブタイルとして表示させてみました
<br>　ちょっと遅れている時間でしたので、たぶんグリニッジ時刻なのかな
<br>　ということでJST（日本時刻）に変換しましょう
<br>　興味があったらみてください。9時間補正の簡単な変換機能があるのですね
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/127756805-bc839492-3d72-4911-b7cd-05e1bef87aae.png">

### 6.エアコン操作
　3つのRunScriptでそれぞれエアコンの操作をしていますが、基本APIをcURLで
<br>　呼んでいるだけです
<br>　NatureRemoのAPIとにらめっこをして、カスタマイズしてみてください　
<br>　
<br>　冷房のRunScript
<br>　 <img width="600"  src="https://user-images.githubusercontent.com/40127279/127756888-8c897cfd-2766-4357-986b-a39b8bfa01e2.png">
#### 背景
　はやくも夏日の声を聞くようになったので、エアコン操作をつくってみました
<br>　Lesson10と18でTVのリモコンを実装しているので、操作部分は簡単でした
<br>　一方でNatureRemoの携帯アプリをみると室温と湿度が表示されてたので、
<br>　気になってAPIを見てみたら、案の定、情報があり、Alfredで表示させてみました
<br>　
#### 取扱説明
### 機能:
　NatureRemoのAPIを使ってエアコンを操作する
### インストール:
　1.[Alfredworkflow](https://github.com/KitanoTamotsu/aircon/releases/download/1.0/aircon.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
<br>　3.NatureRemoのアクセス用トークンとエアコンのアプライアンスIDを入力し変数を設定（インストール後で可能）
<br>　　※ワークフロー右上の[χ]をクリックして設定
<br>　4.スクリプトは設定できる値に合わせて修正が必要かも
<br>　　※ちなみに私のエアコンは、NatureRemoで"Mitsubishi AC 209"として設定されていました
### 使い方:
　キーワード『aircon』で起動
<br>　例）
<br>　　aircon（パラメータなし）　室温・湿度の表示
<br>　　aircon　off　　電源オフ
<br>　　aircon　dry　　除湿オン
<br>　　aircon　数字　　冷房オン（温度設定）
<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

