## 新規リポジトリを作成する際のGitの使い方

```shell
# gitコマンドでアプリディレクトリの作成と.gitフォルダの作成
git init app名

# アプリディレクトリへ移動
cd app名

# ファイルの作成
code ファイル名

# ステージング
git add ファイル名 or .

# コミット
git commit -m 'コミットメッセージ'
```

## 既存のディレクトリをGitリポジトリにする

```shell
# 既存のディレクトリへ移動
cd app名

# 初期化
git init

# ステージング
git add .

# コミット
git commit -m 'コミットメッセージ'
```

## 既存のリモートリポジトリからローカルにGitリポジトリを作成する（cloneバージョン）
cloneは、既存のリモートリポジトリからローカルにリポジトリをコピーして持ってくることができる。

```shell
# 該当のリモートリポジトリから、ローカルにcloneする
git clone URL

# ステージング
git add .

# コミット
git commit -m 'コミットメッセージ'
```

## 既存のリモートリポジトリからローカルにGitリポジトリを作成する（forkバージョン）
forkは、既存のリモートリポジトリから自分のGitHubアカウントにリモートリポジトリのコピーを作成することができる。その後はcloneと同じ要領で作業を行う。
主に、オープンソースのプロジェクトに参加する際などに使用する方法。

```shell
# ForkしたいGitHub上のページにアクセスする

# Forkボタンを押して、自分のGitHubリポジトリにコピーを作成する

# 自分のGitHubリポジトリから、ローカルにCloneする
git clone URL

# fork元のURLをローカルリポの情報に登録しておく
git remote add upstream URL

# ステージング
git add .

# コミット
git commit -m 'コミットメッセージ'

# fork元のリモートリポジトリからpullしてくる
git pull upstream main

# プッシュ
git push origin HEAD

# 自分のGitHub上でプルリクエストを作成
```