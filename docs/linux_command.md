### Linux

```shell
ファイルの強制削除 : rm -r

ディレクトリの強制削除 : rm -rf
```

### Git

```shell
変更履歴の確認 : git log

直前のコミットをやり直す : git commit --amend

追加変更したファイルを前回のコミットに含める : git commit --amend --no-edit

pushできないとき : git push -f origin HEAD
```

### Docker/docker-compose

```shell
ビルドして、デタッチドモード起動 : docker-compose up --build -d

ファイル指定 : docker-compose -f docker-compose.prod.yml build or push or up
```

### heloku-cli

```shell
heroku token 取得 : heroku auth:token

heroku account 取得 : heroku accounts

heroku account 切り替え : heroku accounts:set AccountName
```

### AWS-cli

```shell
AWSのログイン : aws ecr get-login --no-include-email

ECRにリポジトリを作成 : aws ecr create-repository --repository-name XXX
```

### ECS-cli

```shell
クラスターのupdateとファイル指定 : ecs-cli compose --file docker-compose.prod.yml up --cluster-config XXX --force-update
```
