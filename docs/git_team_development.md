# 個人開発時の擬似チーム開発ワークフロー

個人開発するさいに、意識している Git/GitHub の使い方は以下の 4 点

- 実装したい機能を Issue に記載
- 機能単位でブランチを切る
- PullRequest
- Merge

```shell
# ブランチを切る
git checkout -b ブランチ名（機能に関係したものにする）

# 差分の確認
git diff

# ステージングは関連する部分をまとめて、細かくなりすぎないように
git add ファイル名 or .

# ステージングされているかの確認
git status

# コミット
git commit -m 'コミットメッセージ'

# プッシュ + リモートブランチの作成
git push --set-upstream origin HEAD（HEADは現在のブランチ）

# GitHubのリモートリポジトリのページにブランチが作成されているのでPullRequestの作成
# タイトルの記載、コメント部分に該当Issueを紐付ける記述
Closes #Issueの番号
# Issueと紐付けてプルリクエストを作成する
# 作成後は、変更分を適宜プッシュしていく

# ブランチでの作業終了後にリモートでプルリクエストをmaster(main)にマージする

# ローカルでもmaster(main)にマージする
git checkout master(main)
git merge 作業したブランチ名（この瞬間が気持ちいい）
```
