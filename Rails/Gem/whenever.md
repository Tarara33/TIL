# cron    
cronは、UNIX系の OSで動いているプログラム。  
事前に「○時になったら○○のコマンドを実行」と指示を出しておくと、  
その時間になったときに指定しておいたプログラムを動かしてくれる。  
crontabというファイルで実行する時間など定義する。    
- [cronについて](https://wa3.i-3-i.info/word11748.html)    
- [crontabについて](https://wa3.i-3-i.info/word11752.html)
***

# whenever
cronの設定を、rubyの簡単な文法で扱えるようにしたライブラリが whenever。  
コレを使えば crontabに記述する内容を ruby言語で書けるようになる。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'whenever', require: false

$ bundle
~~~
⭐️ require: false    
Railsの内部でwheneverを使うわけではないため、Railsの実行時に読み込まないようにするためつける。
***

## 実行ファイルの作成
~~~
$ bundle exec wheneverize .

=>
[add] writing `./config/schedule.rb'
[done] wheneverized!
# コレが出ればOK!
~~~
config/schedule.rbが生成される。
***

# schedule.rbの編集
初期設定
~~~
[config/schedule.rb]

# Rails.root(Railsメソッド)を使用するために必要
① require File.expand_path(File.dirname(__FILE__) + '/environment')

# cronを実行する環境変数(:development, :product, :test)
# 環境変数'RAILS_ENV'にセットされている変数またはdevelopmentを指定
② rails_env = ENV['RAILS_ENV'] || :development

# cronを実行する環境変数をセット
③ set :environment, rails_env

# cronのログの吐き出し場所
④ set :output, "#{Rails.root}/log/cron.log"

# .zshrcとrbenvのパスを指定するrakeを定義
⑤ job_type :rake, "source /Users/[ユーザー名]/.zshrc; export PATH=\"$HOME/.rbenv/bin:$PATH\"; eval \"$(rbenv init -)\"; cd :path && RAILS_ENV=:environment bundle exec rake :task :output"
~~~
***

## ①
カレントのファイルから見た相対パスを絶対パスに変換して、config/配下にある environment.rb を読み込むためのコード。    
これによって、Railsの環境設定を読み込んで、Rails.root や Rails.env などのメソッドを wheneverの中で使えるようになる。    
  
なぜそれらのメソッドを使えるようにするの？？  
=> Railsの環境や設定に応じてタスクを分けたり、Railsのプロジェクト内のパスを簡単に指定できるから。  
***

## ②
このコードの意味は、Cronジョブがどの Rails実行環境で実行されるかを制御し、それに応じて適切な設定を適用できるようになる。  
これにより、開発環境、本番環境などで異なるスケジュールや設定を持つCronジョブを管理しやすくなる。
***

## ③
②で設定した、環境変数を cronの実行をする環境としてセットしている。
***

## ④
cronのログをはくファイルの設定。  
コレを設定しないと cronはログを出さないのでエラーなど探しにくい！
***

## ⚠️ ⑤
⚠️ cronは.zshと rbenvの環境では動いてくれない!!  
デフォルトの状態では、cronから実行されるコマンドはログインシェルの環境変数を引き継がない。  
そのため、.zshrcで設定した PATHや rbenvの設定は反映されない。  
    
【解決方法】
- cronのジョブで直接必要な環境変数を設定する。  
- シェルスクリプトを作成してその中で環境を設定し、そのシェルスクリプトを cronから実行する。  
    
今回は直接設定した。   
環境変数PATHを設定して、その後で指定したパスに移動し、RAILS_ENVを設定して rakeタスクを実行している。
  
⚠️ 多くのサイトでは見本のコードを書いているが、自分は見本コードだと cron.logに(Bundler::RubyVersionMismatch)と、  
cronを動かすRubyが、元々のPCに入っているRubyのバージョン(2.6.10)を使用してしまい、エラーが出た。  
エラーが出る場合は、
~~~
job_type :rake, %Q{export PATH="$HOME/.rbenv/shims:$PATH"; cd :path && RAILS_ENV=:environment bundle exec rake :task :output}
~~~
に書き直してみて！！
***

### job_type
ジョブの種類とそのジョブを実行するためのコマンドのテンプレートを定義するもの。
wheneverでは次の4つのjob_typeと呼ばれる4種類の処理を行うことができ、時間も好きに設定できる。
