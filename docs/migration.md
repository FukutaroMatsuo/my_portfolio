完全に自分用の書き殴りです。  
誤り等ございましたら、コメント下さい！

## ロールバック

- `rails db:migrate:down VERSION=20190611235049`
  - `VERSION`で指定した箇所まで戻れる
- `rails db:rollback STEP=3`
  - 現在の migration ファイルから遡って、`STEP`で指定した回数戻れる

## カラムの追加

1. `rails g migration Addカラム名Toテーブル名 @@@:integer @@@:string`
2. `rails db:migrate`

- Add 以下はキャメルケースで記載
- カラム名のところは何を書いてもいいが、追加したいカラム名の複数形に統一しておく
- テーブル名にはカラムを追加するテーブル名の**複数形**を書く
- `@@@`はカラム名を**単数形**で記入

以下は user テーブルにふりがなを追加したときの例。

```ruby
class AddKanaToUser < ActiveRecord::Migration[5.2]
  def change
    add_column :users, :kana, :string
  end
end
```

## カラム型の変更

`rails g migration change_data_<カラム名>_to_<テーブル名の複数形>`

```ruby
class ChangeColumnToUsers < ActiveRecord::Migration[5.2]
  def change
  end
end
```

上記のように空の migration ファイルが作成されるので、下記のように編集

```ruby
class ChangeColumnToUser < ActiveRecord::Migration[5.2]
  def up
    change_column :users, :detail, :text
  end

  def down
    change_column :users, :detail, :string
  end
end
```

## 注意点

**カラムの追加**と**カラム型の変更**では、方法がやや異なっていて、migration ファイルを触る時は、追加(up)するだけでなく、削除(down)するときのことも考えなければならない。  
つまり、**カラムの追加**では、change で書いても rollback 時など down する際にもよしなにしてくれるが、**カラム型の変更**で change を使ってしまうと、up 時は良くても、down 時に rollback できないというようなことが起こってしまう。ということ。  
簡単なことだが、ここを理解したときに、migration ファイルに対する怖さが一気に消えた。

## おまけ

これからは上記の様に書くとして、これまでに書いてしまった箇所を rollback するさいはどうすればいいのか...  
これは簡単で、今ある migration ファイルの change と記載している箇所を、up に上書き変更して、その下に down 時の動作を追記すればよい。down 時の動作は変更前のカラム型のこと。  
なんとなく、migration ファイルを上書きするのは怖かったが、これで問題なく動作した。
