# MkDocs(Docker + docker-compose)

## サイト用のディレクトリを作成 & 移動
```shell
$ mkdir docs && mkdir docs/img
$ touch docker-compose.yml mkdocs.yml
```

## docker-compose.yml
```yaml
version: "3"

services:
  mkdocs:
    image: squidfunk/mkdocs-material
    ports:
      - 8000:8000
    volumes:
      - .:/docs
```

## mkdocs.yml
```yaml
site_name: 
site_description: 
site_author: Fukutaro matsuo
copyright: Copyright &copy 2020 Fukutaro matsuo

theme:
  name: material
  features: navigation.tabs
  language: ja
  logo: img/
  favicon: img/
  icon:
    repo: fontawesome/brands/github
  palette:
    scheme: slate
    primary: indigo
    accent: amber
  font:
    text: Noto Sans
    code: Inconsolata

markdown_extensions:
  - admonition
  - codehilite:
      guess_lang: false
  - toc:
      permalink: true
  - footnotes
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji
      emoji_generator: !!python/name:materialx.emoji.to_svg

extra:
  search:
    language: "jp"

nav:
  - home: index.md
```

Build & Up
```shell
$ docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material build
$ docker-compose up
```
[localhost:8000](localhost:8000)にアクセス

GitHub PagesへのDeploy
```shell
$ docker run --rm -it -v ~/.ssh:/root/.ssh -v ${PWD}:/docs squidfunk/mkdocs-material gh-deploy
```