[公式](https://github.com/bkeepers/dotenv)

# dotenv-rails
環境変数を管理してくれる gem
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'dotenv-rails'

$ bundle
~~~
***

## .envファイルの作成
手動でルートディレクトリ配下に「.env」ファイルを作成する。
***

## .gitignoreの編集
githubにあげたら終わるので、注意！
~~~
[.gitignore]

/.env
~~~
***

## .envファイルに環境変数書いてみる
~~~
[.env]

KEY = '123456'
PATH = 'abcde'
~~~
こんな感じで書いていく。
***

## 環境変数設定できてるかコンソールで確認
~~~
[rails c]
$ ENV['KEY']
=> '123456'

$ ENV['PATH']
=> 'abcde'
~~~
こんな感じで出てきたら OK!  

#### なので、コード内で使うときは`ENV[変数名]`の形で書く
***
