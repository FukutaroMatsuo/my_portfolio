[導入](https://kitsune.blog/rails-devise)  
[adminのみ、CLUDを行えるようにする](https://qiita.com/iamdaisuke/items/79d60b3c23e465ae6460)  
[2つ目よりもこっちの方で可能というか、参考になる](https://whatsupguys.net/programming-school-dive-into-code-learning-49/)

`/rails_projects/rehakaigi/app/controllers/users/registrations_controller.rb`で、beforeactionとprotect以下を編集し、新規登録を不可にした。

## deviceの初期設定  

- gem file に追加 

```ruby
gem 'devise' 
gem 'cancancan' 
gem 'rails_admin', '~> 2.0' 
gem 'rolify' 
gem 'devise-i18n' 
gem 'devise-i18n-views' 
```  
- `bundle install`[導入](https://kitsune.blog/rails-devise)  
[adminのみ、CLUDを行えるようにする](https://qiita.com/iamdaisuke/items/79d60b3c23e465ae6460)  
[2つ目よりもこっちの方で可能というか、参考になる](https://whatsupguys.net/programming-school-dive-into-code-learning-49/)

/rails_projects/rehakaigi/app/controllers/users/registrations_controller.rb  
で、beforeactionとprotect以下を編集し、新規登録を不可にした。

## deviceの初期設定  

- gem file に追加 

```ruby
gem 'devise' 
gem 'cancancan' 
gem 'rails_admin', '~> 2.0' 
gem 'rolify' 
gem 'devise-i18n' 
gem 'devise-i18n-views' 
```  
- `bundle install`
- `rails g devise:install`で、関連ファイルをインストール
- rootページの設定
- application.html.erbに、下記の記述(後で別の表示に変更予定) 

```ruby
<body>
  <p class="notice"><%= notice %></p>
  <p class="alert"><%= alert %></p>
  <%= yield %>
</body>
```
- deviceのviewを変更したいので、`rails g devise:views`を実行
- 認証用モデルとmigtrationファイルを作成する  
  `rails g devise user`このとき、user以外でも可
- `rails db:migrate`
- `/users/sign_in`にアクセスして表示の確認

## deviceの日本語化
- config/application.rbを開いて、下記のように追加

```ruby
module App
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 5.2
    config.i18n.default_locale = :ja ＜（追加）

    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.
  end
```
- `rails g devise:views:locale ja`を実行して、完成
- config/locales/devise.views.ja.ymlを開いて、文字をいじることも可能

## viewのカスタマイズ
- config/initializers/devise.rbを開いて、下記のように追記

```ruby
config.scoped_views = true
```
- `rails g devise:views users`を実行すると、編集可能
- controllerを編集したい場合は、`rails g devise:controllers users`を実行すると可能

## admin画面の表示
- `rails g rails_admin:install`を実行し、Enter押下
- 一応、/adminへアクセスして、動作の確認
- 管理者権限の設定、`rails g cancan:ability`を実行しモデルを作成
- model/ability.rbを開いて、下記を追記(admin権限のあるuserのみ、ログインできて、全てのCRUDを行える)

```ruby
if user.try(:admin?)
  can :access, :rails_admin
  can :manage, :all
end
```
- config/initializers/rails_admin.rbの、deviceとcancanのコメントアウトを外す

```ruby
RailsAdmin.config do |config|
 
  ### Popular gems integration
 
  ## == Devise ==
  config.authenticate_with do
    warden.authenticate! scope: :user
  end
  config.current_user_method(&:current_user)
 
  ## == Cancan ==
  config.authorize_with :cancan
 
  ## == Pundit ==
  # config.authorize_with :pundit
 
  ~(省略)~
 
  end
end
```
- `rails g migration add_column_to_users`を実行し、作成されたmigrationファイルを下記のように追記

```ruby
class AddColumnToUsers < ActiveRecord::Migration[5.1]
  def change
    add_column :users, :admin, :boolean, default: false
    add_column :users, :name, :string <ここでnameを追加しても良い
  end
end
```
- `rails db:migrate`
- 一応、schema.rbを確認し、usersテーブルにadminがあるか確認
- ユーザの作成とadminの付与

```ruby
rails c

user = User.new(email: "", password: "", name: "", admin: "true")

user.save
```
- 一応、ログイン/ログアウト時の動作の確認をしておく

## 管理者ページの日本語化
- config/locales/rails_admin.ja.ymlを新規作成
- https://gist.github.com/mshibuya/1662352 をコピーして貼り付け[導入](https://kitsune.blog/rails-devise)  
[adminのみ、CLUDを行えるようにする](https://qiita.com/iamdaisuke/items/79d60b3c23e465ae6460)  
[2つ目よりもこっちの方で可能というか、参考になる](https://whatsupguys.net/programming-school-dive-into-code-learning-49/)

/rails_projects/rehakaigi/app/controllers/users/registrations_controller.rb  
で、beforeactionとprotect以下を編集し、新規登録を不可にした。

## deviceの初期設定  

- gem file に追加 

```ruby
gem 'devise' 
gem 'cancancan' 
gem 'rails_admin', '~> 2.0' 
gem 'rolify' 
gem 'devise-i18n' 
gem 'devise-i18n-views' 
```  
- `bundle install`
- `rails g devise:install`で、関連ファイルをインストール
- rootページの設定
- application.html.erbに、下記の記述(後で別の表示に変更予定) 

```ruby
<body>
  <p class="notice"><%= notice %></p>
  <p class="alert"><%= alert %></p>
  <%= yield %>
</body>
```
- deviceのviewを変更したいので、`rails g devise:views`を実行
- 認証用モデルとmigtrationファイルを作成する  
  `rails g devise user`このとき、user以外でも可
- `rails db:migrate`
- `/users/sign_in`にアクセスして表示の確認

## deviceの日本語化
- config/application.rbを開いて、下記のように追加

```ruby
module App
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 5.2
    config.i18n.default_locale = :ja ＜（追加）

    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.
  end
```
- `rails g devise:views:locale ja`を実行して、完成
- config/locales/devise.views.ja.ymlを開いて、文字をいじることも可能

## viewのカスタマイズ
- config/initializers/devise.rbを開いて、下記のように追記

```ruby
config.scoped_views = true
```
- `rails g devise:views users`を実行すると、編集可能
- controllerを編集したい場合は、`rails g devise:controllers users`を実行すると可能

## admin画面の表示
- `rails g rails_admin:install`を実行し、Enter押下
- 一応、/adminへアクセスして、動作の確認
- 管理者権限の設定、`rails g cancan:ability`を実行しモデルを作成
- model/ability.rbを開いて、下記を追記(admin権限のあるuserのみ、ログインできて、全てのCRUDを行える)

```ruby
if user.try(:admin?)
  can :access, :rails_admin
  can :manage, :all
end
```
- config/initializers/rails_admin.rbの、deviceとcancanのコメントアウトを外す

```ruby
RailsAdmin.config do |config|
 
  ### Popular gems integration
 
  ## == Devise ==
  config.authenticate_with do
    warden.authenticate! scope: :user
  end
  config.current_user_method(&:current_user)
 
  ## == Cancan ==
  config.authorize_with :cancan
 
  ## == Pundit ==
  # config.authorize_with :pundit
 
  ~(省略)~
 
  end
end
```
- `rails g migration add_column_to_users`を実行し、作成されたmigrationファイルを下記のように追記

```ruby
class AddColumnToUsers < ActiveRecord::Migration[5.1]
  def change
    add_column :users, :admin, :boolean, default: false
    add_column :users, :name, :string <ここでnameを追加しても良い
  end
end
```
- `rails db:migrate`
- 一応、schema.rbを確認し、usersテーブルにadminがあるか確認
- ユーザの作成とadminの付与

```ruby
rails c

user = User.new(email: "", password: "", name: "", admin: "true")

user.save
```
- 一応、ログイン/ログアウト時の動作の確認をしておく

## 管理者ページの日本語化
- config/locales/rails_admin.ja.ymlを新規作成
- https://gist.github.com/mshibuya/1662352 をコピーして貼り付け[導入](https://kitsune.blog/rails-devise)  
[adminのみ、CLUDを行えるようにする](https://qiita.com/iamdaisuke/items/79d60b3c23e465ae6460)  
[2つ目よりもこっちの方で可能というか、参考になる](https://whatsupguys.net/programming-school-dive-into-code-learning-49/)

/rails_projects/rehakaigi/app/controllers/users/registrations_controller.rb  
で、beforeactionとprotect以下を編集し、新規登録を不可にした。

## deviceの初期設定  

- gem file に追加 

```ruby
gem 'devise' 
gem 'cancancan' 
gem 'rails_admin', '~> 2.0' 
gem 'rolify' 
gem 'devise-i18n' 
gem 'devise-i18n-views' 
```  
- `bundle install`
- `rails g devise:install`で、関連ファイルをインストール
- rootページの設定
- application.html.erbに、下記の記述(後で別の表示に変更予定) 

```ruby
<body>
  <p class="notice"><%= notice %></p>
  <p class="alert"><%= alert %></p>
  <%= yield %>
</body>
```
- deviceのviewを変更したいので、`rails g devise:views`を実行
- 認証用モデルとmigtrationファイルを作成する  
  `rails g devise user`このとき、user以外でも可
- `rails db:migrate`
- `/users/sign_in`にアクセスして表示の確認

## deviceの日本語化
- config/application.rbを開いて、下記のように追加

```ruby
module App
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 5.2
    config.i18n.default_locale = :ja ＜（追加）

    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.
  end
```
- `rails g devise:views:locale ja`を実行して、完成
- config/locales/devise.views.ja.ymlを開いて、文字をいじることも可能

## viewのカスタマイズ
- config/initializers/devise.rbを開いて、下記のように追記

```ruby
config.scoped_views = true
```
- `rails g devise:views users`を実行すると、編集可能
- controllerを編集したい場合は、`rails g devise:controllers users`を実行すると可能

## admin画面の表示
- `rails g rails_admin:install`を実行し、Enter押下
- 一応、/adminへアクセスして、動作の確認
- 管理者権限の設定、`rails g cancan:ability`を実行しモデルを作成
- model/ability.rbを開いて、下記を追記(admin権限のあるuserのみ、ログインできて、全てのCRUDを行える)

```ruby
if user.try(:admin?)
  can :access, :rails_admin
  can :manage, :all
end
```
- config/initializers/rails_admin.rbの、deviceとcancanのコメントアウトを外す

```ruby
RailsAdmin.config do |config|
 
  ### Popular gems integration
 
  ## == Devise ==
  config.authenticate_with do
    warden.authenticate! scope: :user
  end
  config.current_user_method(&:current_user)
 
  ## == Cancan ==
  config.authorize_with :cancan
 
  ## == Pundit ==
  # config.authorize_with :pundit
 
  ~(省略)~
 
  end
end
```
- `rails g migration add_column_to_users`を実行し、作成されたmigrationファイルを下記のように追記

```ruby
class AddColumnToUsers < ActiveRecord::Migration[5.1]
  def change
    add_column :users, :admin, :boolean, default: false
    add_column :users, :name, :string <ここでnameを追加しても良い
  end
end
```
- `rails db:migrate`
- 一応、schema.rbを確認し、usersテーブルにadminがあるか確認
- ユーザの作成とadminの付与

```ruby
rails c

user = User.new(email: "", password: "", name: "", admin: "true")

user.save
```
- 一応、ログイン/ログアウト時の動作の確認をしておく

## 管理者ページの日本語化
- config/locales/rails_admin.ja.ymlを新規作成
- https://gist.github.com/mshibuya/1662352 をコピーして貼り付け
- `rails g devise:install`で、関連ファイルをインストール
- rootページの設定
- application.html.erbに、下記の記述(後で別の表示に変更予定) 

```ruby
<body>
  <p class="notice"><%= notice %></p>
  <p class="alert"><%= alert %></p>
  <%= yield %>
</body>
```
- deviceのviewを変更したいので、`rails g devise:views`を実行
- 認証用モデルとmigtrationファイルを作成する  
  `rails g devise user`このとき、user以外でも可
- `rails db:migrate`
- `/users/sign_in`にアクセスして表示の確認

## deviceの日本語化
- config/application.rbを開いて、下記のように追加

```ruby
module App
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 5.2
    config.i18n.default_locale = :ja ＜（追加）

    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.
  end
```
- `rails g devise:views:locale ja`を実行して、完成
- config/locales/devise.views.ja.ymlを開いて、文字をいじることも可能

## viewのカスタマイズ
- config/initializers/devise.rbを開いて、下記のように追記

```ruby
config.scoped_views = true
```
- `rails g devise:views users`を実行すると、編集可能
- controllerを編集したい場合は、`rails g devise:controllers users`を実行すると可能

## admin画面の表示
- `rails g rails_admin:install`を実行し、Enter押下
- 一応、/adminへアクセスして、動作の確認
- 管理者権限の設定、`rails g cancan:ability`を実行しモデルを作成
- model/ability.rbを開いて、下記を追記(admin権限のあるuserのみ、ログインできて、全てのCRUDを行える)

```ruby
if user.try(:admin?)
  can :access, :rails_admin
  can :manage, :all
end
```
- config/initializers/rails_admin.rbの、deviceとcancanのコメントアウトを外す

```ruby
RailsAdmin.config do |config|
 
  ### Popular gems integration
 
  ## == Devise ==
  config.authenticate_with do
    warden.authenticate! scope: :user
  end
  config.current_user_method(&:current_user)
 
  ## == Cancan ==
  config.authorize_with :cancan
 
  ## == Pundit ==
  # config.authorize_with :pundit
 
  ~(省略)~
 
  end
end
```
- `rails g migration add_column_to_users`を実行し、作成されたmigrationファイルを下記のように追記

```ruby
class AddColumnToUsers < ActiveRecord::Migration[5.1]
  def change
    add_column :users, :admin, :boolean, default: false
    add_column :users, :name, :string <ここでnameを追加しても良い
  end
end
```
- `rails db:migrate`
- 一応、schema.rbを確認し、usersテーブルにadminがあるか確認
- ユーザの作成とadminの付与

```ruby
rails c

user = User.new(email: "", password: "", name: "", admin: "true")

user.save
```
- 一応、ログイン/ログアウト時の動作の確認をしておく

## 管理者ページの日本語化
- config/locales/rails_admin.ja.ymlを新規作成
- https://gist.github.com/mshibuya/1662352 をコピーして貼り付け  

## 新規登録を不可にする  
user modelから、`registerable`の部分をコメントアウトする

```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable, # ←これ
         :recoverable, :rememberable, :validatable
end

```

admin userは、adminpageから、新規作成が行えるため、特に問題なし