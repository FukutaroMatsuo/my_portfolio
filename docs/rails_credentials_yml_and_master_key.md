# Rails の credentials.yml.enc と master.key の関係性について（undefined method `[]' for nil:NilClass (NoMethodError)）

職場で運用している Rails で作成した、顧客と収益アプリの公開用 clone を作成する際に、題名のところでハマったのでメモ用として残しておく。

## 経緯

1. GitHub のプライベートリポジトリからローカルにソースコードを clone
2. プライバシー情報を削除後、`.git`ファイルを削除して、アプリ名（folder 名）を`_v2`へと変更
3. `git init`して、リモートリポジトリへプッシュ
4. CircleCi と連携させて、master に merge されたタイミングで自動テストを組む
5. RDS を本番環境の db にしたいので、ローカルで`credentials.yml`に、RDS の情報を打ち込む設定を行う(このとき、`credentials:edit`コマンドを一度使用して、なぜか一度 credential ファイルを削除、`credeitials:edit`コマンドで再作成という謎の手順を踏んでいます)
6. ECR にコンテナデプロイして、ECS のタスクを実行しようとしたところ、ECR のコンテナが起動していない
7. 開発用の Dockerfile を push していたため、`rails s`コマンドを起動しておらず追記
8. 一度、開発環境で動作確認しようと思い、`docker build`すると、サーバーが起動しないエラー発生（undefined method `[]' for nil:NilClass (NoMethodError)）

ここまでが経緯です。

AWS は一度お試しアプリで、本番環境として自動デプロイを組んだことがあるという程度で、サービスに関してよく理解しないまま進めていました。

## 何が問題だったのか

問題は、普通に 5 番目の項目ですね。笑

少し詳しくみると、

- credential.yml.enc は、master.key を使用して暗号化、復号される
- `git push`時に、master.key はプッシュされない（.gitignore に記載されているため）

つまり、clone してきた時点で master.key は無かった。 そして、一回目の`credentials:edit`コマンド時に master.key が作成された。

しかも、一度 credentials.yml ファイルを削除して、再度`credentials:edit`コマンドを行っていたので、master.key との整合性が取れていない credentials.yml が作成されていた。

そのため暗号化も復号もできておらず、一度作成していた RDS の設定も消えていた。という感じですかね？多分

## どう解決したか

この場合は、

- 一度、master.key と credentials.yml を削除
- `credentials.edit`コマンドで 2 つのファイルを再作成

という工程でいけました！

結構ありがちなエラーみたいで調べたらたくさん情報が出てきたけど、いい勉強になった。  
同じようなケースの方は私の屍を超えていってください！！
