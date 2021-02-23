# Rails-api + Nuxt での初回起動時のエラー解決
題名の通り、`Rails-api + Nuxtjs + Postgresql`を使用してアプリを作成していると、初回のNuxt起動時にエラーが発生したので備忘録的に残しておく。

# エラー内容
```vue
Error: /app/.nuxt/client.js: Cannot find module '@babel/preset-env/lib/utils'
```
となっており、どうやら`@babel/preset-env`のエラーだと書いてある。

# 解決策
```vue
# 一度最新のものをアンインストール
npm uninstall @babel/preset-env

# 少し古いバージョンをインストール
npm uninstall @babel/preset-env@7.12.13
```

これで動作した。  
動作したが、どこが問題となっていたかの把握は現状できていない。  

引き続き、学習を継続していき原因がわかったら追記予定。