# まえがき

railsで、作成したwebアプリをherokuへデプロイする際にdbがうまく反映されず、詰まったので簡単に備忘録(自戒)として残しておく。


# 経緯

開発環境でdbにカラムを追加する際に、一度作ったカラムと同じ名前のものを作成しようとしたことで、migrationが途中で中断され、中途半端なmigraionファイルが作成された。`db:rollback`して`db:migrate`するもエラーが出現して解決方法がわからず、ググると、`db:reset`して、`dn:migrate`する方法や`rails db:migrate:reset`などの方法が書かれていたが、resetコマンドに恐怖を感じたこと、開発環境では問題なく動作していたことで、「ま、いっか」と開発を続けてしまった。

そのことを忘れて、herokuへデプロイしてしまい、createアクション時にエラーが出現して、そのことを思い出して、解決方法をググった。

# 開発環境での解決

初めは、`db:reset`して、`db:migrate`したが、同じカラムが存在しているというエラーが出現したため、半ば諦めつつも、`rails db:migrate:reset`コマンドを行うと、すんなり通った。

これもググった結果だが、migrationする順番の違いで、初めの操作はうまくいかなかったのかな？恐らく

# 本番環境での解決

[heroku上のDBをリセットする](https://qiita.com/G-awa/items/792b7cd60205823f7ac0)を参考にして、

```ruby
$ heroku pg:reset DATABASE_URL

$ heroku run rails db:reset

$ heroku run rails db:migrate
```

としたが、ここで何故かheroku上で同じエラーが出現。
原因がわからず、heroku上でadminに入り、カラムを確認すると、以前と同じ状態。

もう一度慎重に、上記を繰り返したが、それでもだめ...

途方にくれ、さらにググると、以下の記事を発見。

[Herokuでデータベーススキーマを変更したとき→db:migrateしてheroku restartする](https://seinzumtode.hatenadiary.jp/entry/20120527/1338102726)

上記を参考に、`heroku restart`すると、正常に動作した^^

# 最後に

初学者の私は、db関連のエラーに拒否反応が出てしまい、問題を先送りにしてしまっていた。
そんな私への自戒として、この記事を残しておく。

間違っている箇所や、もっと簡単な方法などございましたら、ご教示くだされば幸いです。

