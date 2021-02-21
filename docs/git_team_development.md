# 個人開発時の擬似チーム開発ワークフロー

個人開発するさいに、意識している Git/GitHub の使い方は以下の 5 点

- 実装したい機能を Issue に記載
- 機能単位でブランチを切る
- Push前にPullを行う
- PullRequest
- リモートリポでのMerge

```shell
# ブランチを切って移動
git checkout -b ブランチ名（機能に関係したものにする）

# 作業後、変更すべきでないファイルが混ざっていないかの確認
git status

# 差分の確認
git diff

# ステージングは関連する部分をまとめて、細かくなりすぎないように
git add ファイル名 or .

# ステージングされているかの確認
git status

# コミット
git commit -m 'コミットメッセージ'

# 誰かがリモートリポに変更を加えている可能性があるため、プッシュする前に必ずリモートリポのmainをPullする
git pull origin main

# 本来ならここでコンフリクトする可能性があるので、その場合は修正して再度 add & commit

# プッシュ
git push origin HEAD

# GitHubのリモートリポのページにブランチが作成されているのでプルリクの作成

# タイトルの記載、コメント部分に該当Issueを紐付ける記述も可能
Closes #Issueの番号

# Issueと紐付けてプルリクを作成する

# 作成後は、変更分を適宜プッシュしていく

# ブランチでの作業終了後にリモートリポでプルリクエストをmainにマージする

# ローカルリポのmainをリモートリポのmainからPullしてきて最新のものにする
git checkout main(master)
git pull origin main
```
