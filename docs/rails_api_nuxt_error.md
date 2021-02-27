# Rails-api + Nuxt での初回起動時のエラー解決
題名の通り、`Rails-api + Nuxtjs + Postgresql`を使用してアプリを作成していると、初回のNuxt起動時にエラーが発生したので備忘録的に残しておく。

インストール、アンイストールコマンドは全てフロントエンド記載のコンテナに入ってコマンドを実行しています。

```shell
# 入り方
docker-compose exec front sh
```

## エラー1
```shell
Error: /app/.nuxt/client.js: Cannot find module '@babel/preset-env/lib/utils'
```
となっており、どうやら`@babel/preset-env`のエラーだと書いてある。

## 解決策
```shell
# 現在のものをアンインストール
npm uninstall @babel/preset-env

# 少し古いバージョンをインストール
npm install @babel/preset-env@7.12.13
```

これで動作した。  
動作したが、どこが問題となっていたかの把握は現状できていない。  

引き続き、学習を継続していき原因がわかったら追記予定。


## エラー2
```shell
ERROR  in ./node_modules/vuetify/src/components/VDataTable/VDataTableHeader.sass
Module build failed (from ./node_modules/sass-loader/dist/cjs.js):
SassError: Expected ".
```

とまぁ、`sass-loder`の読み込みがうまくいっていない感じです。

## 解決策
```shell
# 現在のsass-loderをアンイストール
npm uninstall -D sass-loader

# 少し古いバージョンをインストール
npm i -D sass-loader@7.1.0
```

今回は、`version 7.1.0`で機能しましたが、もっと良い解決方法がある可能性は大いにあります。

しかし、バージョンの齟齬によるエラー多いですね...