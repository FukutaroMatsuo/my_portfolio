アプリ作成時に毎回調べている気がするので、備忘録的にまとめてみました。
下記の`app名`の箇所には適宜アプリ名を入力して下さい。

※間違いがありましたら変更しますのでコメント頂けると嬉しいです^^

## 新規ディレクトリ作成 〜 hello world!!まで  

まず、アプリの土台となるディレクトリを作ります。
さらに、touchコマンドで、2つの空ファイルを作成します。

``` terminal
$ mkdir app名 && cd app名
$ touch Gemfile Gemfile.lock
```  

私は、VScodeを使用して開発しているので
code コマンドを使用して起動しています。
ちなみに、code コマンドは起動と作成をしてくれます。

Gemfileを編集します。

```terminal
$ code Gemfile
```

```ruby:Gemfile
source 'https://rubygems.org'
gem 'rails', '~>5.2'
```

Dockerfileを作成して編集します。

```terminal
$ code Dockerfile
```

```ruby:Dockerfile

FROM ruby:2.5
RUN apt-get update
RUN apt-get install -y \ 
    build-essential \
    libpq-dev \
    nodejs \
    postgresql-client \
    yarn \
    vim

WORKDIR /app名
COPY Gemfile Gemfile.lock /app名/
RUN bundle install
```

docker-compose.ymlファイルを作成して編集します。


```terminal
$ code docker-compose.yml
```

```ruby:docker-compose.yml
version: "3"

volumes:
  db-data:

services:
  web:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - ".:/app名"
    environment:
      - "DATABASE_PASSWORD=postgres"
    tty: true
    stdin_open: true
    depends_on:
      - db
    links:
      - db

  db:
    image: postgres
    volumes:
      - "db-data:/var/lib/postgresql/data"
    environment:
      - "POSTGRES_HOST_AUTH_METHOD=trust"
      - "POSTGRES_USER=postgres"
      - "POSTGRES_PASSWORD=postgres"
```

コンテナの起動を行い、webコンテナに入って、rails new します。

```terminal
$ docker-compose up --build -d
$ docker-compose exec web bash
$ rails new . --force --database=postgresql
```
rails new で作成された database.yml ファイルに追記します。

```ruby:database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db  #追記
  user: postgres  #追記
  port: 5432  #追記
  password: <%= ENV.fetch("DATABASE_PASSWORD") %>  #追記
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
```

```terminal
$ rails db:migrate
$ rails s -b 0.0.0.0
```
Chromeの検索バーに、localhost:3000 と入力してアクセスするとhello worldが表示されているかと思います！