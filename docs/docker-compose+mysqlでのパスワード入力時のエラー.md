RailsをAPIモードで Docker + MySQL を使用してWebアプリ開発をしています。

環境構築時に、
`docker-compose up` して dbにログインしようとすると
`ERROR 1045 (28000): Access denied for user`と
ログインができないエラーに遭遇したので、私が解決できた方法を記載しておきます！

### 解決法

私の場合は、docker-compose.ymlで 

```docker-compose.yml
db-data:/var/lib/mysql:cached
```
このようにしてdbデータをローカルに保存してマウントしていたのですが、
ここに試しに作っていた、いくつかのvolumeが残っていたため、
そちらの環境変数が優先され、現在のパスワードが反映されていないという問題でした。

MySQL公式に
> 既にデータベースを格納しているデータディレクトリとともにコンテナを起動した場合は、下記の変数はどれも影響を及ぼさないことに留意してください。

とあるようです。
詳しくは [こちら](https://qiita.com/hanyuTransfer/items/4584d0a4d85f0f78cfb6) の記事をご覧ください。

つまり、
マウント先のローカルのdbを一旦削除して、再度作り直せばOKということです。

### 手順

```terminal
docker-compose down
docker volume ls
docker volume rm (lsで表示されたvolume名をスペース区切りで並べる)
docker-compose up
```

で、作り直して再度ログインすると、問題なくログインできました！！

上記の手順で、
`docker volume rm`時にもしかすると、削除できない場合があるかもしれません。
その時は、

```terminal
docker system prune
docker volume rm ○○
```
とするとうまくいくかも知れません！！
