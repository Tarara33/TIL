[公式](https://github.com/rubyconfig/config)

# 機能
環境ごとに違う定数を使える。    
ここでいう環境とは、ローカル環境・開発環境・本番環境・テスト環境など。        
    
たとえばホストの情報を管理したい時などに使える。    
メールを送る下準備としてホスト情報を設定しておく必要があるが、    
ホスト情報は開発環境、ステージング環境、本番環境で異なる。    
    
- 開発環境...localhost:3000        
- ステージング環境...staging.runteq.jp        
- 本番環境...runteq.jp
これを素直に実装しようとすると以下のようになる。
~~~
if Rails.env.development?
  host = 'localhost:3000'
elsif Rails.env.staging?
  host = 'staging.runteq.jp'
elsif Rails.env.production?
  host = 'runteq.jp'
end
~~~
これでも動くが管理対象の値が他にも出てきた場合、各所でこのような条件分岐をするのはあまり綺麗ではない。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'config'

$ bundle
~~~
***

## configファイルの生成
~~~
$ rails g config:install
~~~
作成されたファイルと役割    
[![Image from Gyazo](https://i.gyazo.com/4478d915fee1d7a129cc41fe79a15cfe.png)](https://gyazo.com/4478d915fee1d7a129cc41fe79a15cfe)
***

## 試しに設定
今回は開発環境でホスト設定する
~~~
[config/settings/development.yml]

default_url_options:
=> Railsアプリケーションで使用されるデフォルトのURLオプションを設定するためのオプション。
  host: 'localhost:3000'
~~~
***

