# rails-template

# 使い方
## 環境情報
- MySQL
- yarn

## 1. 事前準備/確認
1. dockerを利用するのでdockerは各自の環境に用意してください。
2. `.node-version`、`.ruby-version`はそれぞれ好きなバージョンを指定してください。
3. `docker-compose.yml`の内容を確認。必要に応じてデータベース名などを更新してください。

## 2. 利用するRailsのバージョンを指定する
`Gemfile`の`gem 'rails', ''`でバージョンを指定してください。

## 3. imageを作成する
`docker-compose build --no-cache`

## 4. Railsのプロジェクトを作る
`docker-compose run --rm --no-deps rails rails new . --force --database=mysql`

## 5. imageを更新する
`docker-compose build --no-cache`
