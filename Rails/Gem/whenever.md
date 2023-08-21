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

# schedule.rbの編集 (初期設定)
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

【job-type】    
- command (bashコマンド実行)  
任意のコマンドを実行する。  
:taskと :outputを指定して、そのコマンドの出力をログに書き出す。  
    
- rake (rakeタスク実行)  
Rakeタスクを実行する。  
:pathで指定したディレクトリに移動してから、:environment_variableに設定した環境で、bundle exec rake :taskコマンドを実行する。  
    
- runner (Rails内のメソッド実行)  
Railsの runnerを使って、Rubyのコードを実行する。  
:pathで指定したディレクトリに移動してから、 bin/rails runner -e :environment ':task'コマンドを実行する。

- script (scriptの実行)
スクリプトを実行する。  
:pathで指定したディレクトリに移動してから、:environment_variableに設定した環境で、bundle exec script/:taskコマンドを実行する。
    
これらはデフォルトでは以下のコードと同じ挙動をする。 
~~~
job_type :command, ":task :output"
job_type :rake,    "cd :path && :environment_variable=:environment bundle exec rake :task --silent :output"
job_type :runner,  "cd :path && bin/rails runner -e :environment ':task' :output"
job_type :script,  "cd :path && :environment_variable=:environment bundle exec script/:task :output"
~~~
💡 設定はタスク処理外でして、コマンド使うのはタスク処理内という感じ！
***

# schedule.rbの編集 (タスク設定)
タスクはハローと出力させるというもの。
~~~
[lib/tasks/hello.rake]
namespace :greet do
  desc 'ハローと出力する'
  task :say_hello do
    puts 'HELLO!'
  end
end
~~~
コレを schedule.rbに書いていく。
~~~
[config/schedule.rb]

# Rails.root(Railsメソッド)を使用するために必要
require File.expand_path(File.dirname(__FILE__) + '/environment')

# cronを実行する環境変数(:development, :product, :test)
# 環境変数'RAILS_ENV'にセットされている変数またはdevelopmentを指定
rails_env = ENV['RAILS_ENV'] || :development

# cronを実行する環境変数をセット
set :environment, rails_env

# cronのログの吐き出し場所
set :output, "#{Rails.root}/log/cron.log"

# .zshrcとrbenvのパスを指定するrakeを定義
job_type :rake, %Q{export PATH="$HOME/.rbenv/shims:$PATH"; cd :path && RAILS_ENV=:environment bundle exec rake :task :output}


#1時間ごとに（:hour）実行する先程設定した rakeタスクを記入
every 1.minute do
  rake 'hello:say_hello'
end
~~~
この　rakeが先ほど job-typeで設定した「job_type :rake」の内容が使われる！
***

## 時間設定
- １分ごと
~~~
every 1.minute
~~~
  
- １時間ごと
~~~
every 1.hour do
~~~
  
- １日ごと
~~~
every 1.day do
~~~
時間指定してないので、デフォルトで　00:00にタスク実行される。
もし時間設定する場合はこちらのようにする。
~~~
every 1.day, at: ['4:30 am', '6:00 pm'] do
=> 毎日4:30と 18:00にタスク実行
~~~

- 曜日ごと
~~~
every :sunday do 
~~~
***

# ⭐️ crontabに反映させる
- wheneverの設定更新
~~~
$ bundle exec whenever --update-crontab

=>
[write] crontab file updated
コレが出たら、crontabが正常に更新されてる。
~~~

- 設定内容にエラーがないか確認
~~~
$ bundle exec whenever

=>
0 * * * * /bin/bash -l -c 'export PATH="$HOME/.rbenv/shims:$PATH"; cd /Users/sarina/workspace/runteq/rails_ouyo/27722_Tarara33_runteq_curriculum_advanced && RAILS_ENV=development bundle exec rake article_status:change_to_be_published >> /Users/sarina/workspace/runteq/rails_ouyo/27722_Tarara33_runteq_curriculum_advanced/log/cron.log 2>&1'

## [message] Above is your schedule file converted to cron syntax; your crontab file was not updated.
## [message] Run `whenever --help' for more options.

この二つの[message]は出力されるのが正常
(crontabにupdateされてないよ(--update-crontabつけてないから)　と　ヘルプコマンドの案内メッセージ)
~~~

- 設定されているcronを見る
~~~
$ crontab -l

=>
# Begin Whenever generated tasks for: /Users/sarina/workspace/runteq/rails_ouyo/27722_Tarara33_runteq_curriculum_advanced/config/schedule.rb at: 2023-08-21 20:43:37 +0900
0 * * * * /bin/bash -l -c 'export PATH="$HOME/.rbenv/shims:$PATH"; cd /Users/sarina/workspace/runteq/rails_ouyo/27722_Tarara33_runteq_curriculum_advanced && RAILS_ENV=development bundle exec rake article_status:change_to_be_published >> /Users/sarina/workspace/runteq/rails_ouyo/27722_Tarara33_runteq_curriculum_advanced/log/cron.log 2>&1'

# End Whenever generated tasks for: /Users/sarina/workspace/runteq/rails_ouyo/27722_Tarara33_runteq_curriculum_advanced/config/schedule.rb at: 2023-08-21 20:43:37 +0900
~~~

- crontabの設定削除（定期実行を辞めたいとき）
~~~
$ bundle exec whenever --clear-crontab

=>
[write] crontab file

この後
$ crontab -lを実行すると空白が返ってくる。

もし再度実行させたい場合は
$ bundle exec whenever --update-crontabをまた実行する。
~~~
***

# ログの出力
cronは実行がうまくいってる時は特に cron.logにログはたまらない。
なので動いてるか確認したい場合は、タスク処理の中に「puts」などで出力させる。
~~~
[lib/tasks/hello.rake]
namespace :greet do
  desc 'ハローと出力する'
  task :say_hello do
    puts 'HELLO!'
  end
end


[log/cron.log]
HELLO!
HELLO!
HELLO!
~~~
***

~~~
