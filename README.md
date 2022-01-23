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

# おすすめライブラリ
以下は『使い方』の手順完了後に行ってください。

## ridgepole
https://github.com/ridgepole/ridgepole

### 使用方法
#### 1. Gemfileに書く
`gem 'ridgepole', '~> 1'`

※ バージョンは任意

#### 2. imageの更新
`docker-compose build --no-cache`

#### 3. スキーマの作成
ここでは`Schemafile`はディレクトリで管理します。Rakeコマンドにしてしまうのもいいかもしれません。

`docker-compose run --rm rails bundle exec ridgepole -c ./config/database.yml --export -o ./db/Schemafile`

#### 4. 使用してみる
以降の手順は飛ばしても問題ありません。

`db/schemas/users.schema.rb`に以下を追加。（`schemas`フォルダは作成してください）

```ruby
create_table :users, force: :cascade, charset: 'utf8mb4' do |t|
  t.string  :name, null: true
  t.integer :age,  null: true
  t.string :address, null: true
end
```

その後`db/Schemafile`に以下を追加。

```
require 'schemas/users.schema.rb'
```

適用。

`docker-compose run --rm rails bundle exec ridgepole -c ./config/database.yml --file ./db/Schemafile --apply`

#### 参考
- https://jpndev.com/post/672124

## RuboCop
https://rubocop.org/

### 使用方法
#### 1. Gemfileに書く
```ruby
gem 'rubocop', require: false
gem "rubocop-performance", require: false
gem "rubocop-rails", require: false
gem "rubocop-rspec", require: false
```

#### 2. imageの更新
`docker-compose build --no-cache`

#### 3. 必要なファイルの生成
`docker-compose run --rm rails rubocop --auto-gen-config`

#### 4. 設定ファイルの更新
```yaml
inherit_from: .rubocop_todo.yml

require:
  - rubocop-performance
  - rubocop-rails
  - rubocop-rspec

AllCops:
  NewCops: enable
```

#### 5. 使用してみる
`docker-compose run --rm rails rubocop`

#### 参考
- https://nishinatoshiharu.com/insatall-rubocop/