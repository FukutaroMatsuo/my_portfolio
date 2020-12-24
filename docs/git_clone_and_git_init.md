# GitHub からアプリをクローンして複製する（初期化）

自作のアプリをポートフォリオとして転職活動を行っています。

個人情報がふんだんに含まれていたため、そのあたりを一掃して、アプリの機能のみ維持した状態で公開用のアプリを再作成するための、**複製**と**初期化**手順です。

# 流れ

- 複製したいアプリを clone
- 必要によっては、clone したディレクトリ名を変更（app_v2 としました）
- 個人情報関連を編集（ここはコミット履歴に残したくない）
- ディレクトリの git の情報を削除
- 新規のリモートリポジトリを作成
- 初期化し、作成したリモートリポジトリにプッシュ

という感じです。

# コマンド

```shell
# GitHub上で複製したいアプリのclone する際のURLをコピー
git clone コピーしたURL

# ファイルを編集し、一旦.gitを削除
rm -rf .git

# リポジトリを作成し、新規作成時の指示に従う
# 初期化し、再度pushまで
git init
git add .
git commit -m 'コミットメッセージ'
git branch -M main
git remote add origin 作成したリモートリポジトリのURL
git push origin HEAD
```

完了。

余談ですが、 `git push origin HEAD`とすると、現在のブランチ名で push してくれて便利！