## Welcomeページまで

- リポジトリをクローン
```git
git clone git@github.com:tanikento/rails5.2.1_on_docker.git
```

- ディレクトリ移動
```shell
cd rails5.2.1_on_docker
```

- 新規Railsアプリを作成
```docker
docker-compose run web rails new . --force --no-deps --database=mysql --skip-bundle
```

- database.yml を修正(./config/database.yml)
```yml
default: &default
  # 省略
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: <%= ENV.fetch('DATABASE_HOST') { 'localhost' } %>
  port: <%= ENV.fetch('DATABASE_PORT') { 5432 } %>
  username: <%= ENV.fetch('DATABASE_USER') { 'root' } %>
  password: <%= ENV.fetch('DATABASE_PASSWORD') { 'root' } %>
```

- dockerイメージをビルド & up
```docker
docker-compose up -d --build
```

- コンテナに入る
```shell
docker/shell
```

- DB作成
```ruby
bin/rails db:create
```

- マイグレーション
```ruby
bin/rails db:migrate
```

- 確認
```
localhost:3000
```

## scafold

- scafold
```ruby
bin/rails g scaffold article title:string
```

## Action Text

- ACtion Textをインストール
```ruby
bin/rails action_text:install
```

- マイグレーション
```ruby
bin/rails db:migrate
```

## リッチテキスト適応

- Model(./app/models/article.rb)
```ruby
class Article < ApplicationRecord
    has_rich_text :content
end
```

- Controler(./app/controllers/articles_controller.rb)
```ruby
class ArticlesController < ApplicationController
    # 省略
    def article_params
        params.require(:article).permit(:title, :content)
    end
end
```

- View(./app/views/articles/\_form.html.erb)
```ruby
<%= form_with(model: article, local: true) do |form| %>
    <!-- 省略 -->
    <div class="field">
        <%= form.label :content %>
        <%= form.rich_text_area :content %>
    </div>
<% end %>>
```

- 確認
```
localhost:3000/articles
```

## Docker

- コンテナに入る
```shell
docker/shell
```

- コンテナ止める
```shell
docker/stop
```

- コンテナ再起動
```shell
docker/start
```
