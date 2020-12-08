# docker の学習

## 基本コマンド

- `docker ps` : 現在**起動中**のコンテナの確認
- `docker ps -a` : 現存するコンテナの確認
- `docker pull` : docker hub からイメージを取得
- `docker create` : 取得したイメージからコンテナを作成
- `docker start` : 作成したコンテナの起動
- `docker run` : 上記すべてを一括で行うことができる
- `docker restart <container ID>` : コンテナの再起動
- `docker stop <container ID>` : コンテナの停止 ※使わないコンテナは`rm`コマンドで削除
- `docker rm <container ID>` : コンテナの削除
- `docker rm -f <container ID>` : コンテナの強制削除

## 【同じ名前のコンテナがすでに起動していた】docker: Error response from daemon: Conflict.

このエラーの解決ポイント

- ~~`docker ps -a`コマンドで起動中のコンテナの確認~~
- `docker ps`コマンドで起動中のコンテナの確認
- `docker rm <continer ID>`で起動中のコンテナの削除
- `restart`が設定されている場合は、`docker rm -f <container ID>`で削除
- ~~これだけだと起動できなかったので、Mac の docker アイコンからリスタートして、再起動で解決した~~
- ひとつ上の項目は、無くても今日は起動できた^^;

## コンテナのシェルへの接続

- `docker attach` : exit で抜けたらコンテナが停止するので、`Ctr+p+q` を同時押し
- `docker exec` : exit で抜けても停止されないので基本こっち

## 自動ビルドできない

1. docker hub の該当リポジトリに入る
2. Builds に入る
3. configure automated builds ボタンを押下
4. create and build ボタン押下で完了
