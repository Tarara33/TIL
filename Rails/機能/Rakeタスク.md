[参考](https://autovice.jp/articles/177)

# Rakeタスクとは
Ruby ＋　Make　 = Rakeという意味。    
Rubyで書かれたコードをタスクとして作成しておき必要に応じて呼び出し実行する事が出来る機能。  
例えば、データベースのマイグレーション、テストデータの投入、定期的なバッチ処理など、    
一連の手順をまとめて実行することができ、頻繁に行う作業や複雑な作業を自動化するのに役立つ。
***

# 作り方
基本的に Gemは rails newしたときに入ってるがなければ追加。
~~~
gem 'rake'
~~~

今回は、例として Helloと表示するタスクを作る。
~~~
$ rails g task ファイル名

[例]
$ rails g task hello
~~~
***

## タスクファイルの編集
中身の初期設定はこちら。
~~~
[lib/tasks/hello.rake]

namespace :greet do
end
~~~

コレに当てはめて実装する。
~~~
namespace :タスクのファイル名 do
  desc 'タスクの説明'
  task :タスクの名称 do
    # タスクの処理内容
  end
end


[例]
namespace :greet do
  desc 'ハローと出力する'
  task :say_hello do
    puts 'HELLO!'
  end
end
~~~
***

## タスクの確認
`$ rails -T`で自分で作成した Rakeタスクを含む Rakeタスクの一覧が表示される。
~~~
$ rails -T

rails hello:say_hello                            # helloと表示する
~~~
💡 一覧の中には、Railsに組み込まれている Rakeタスクも含まれている。  
例えば、rails db:migrateなどいつも使用しているコマンドもあり、    
その実態は、Railsに組み込まれている Rakeタスクだということがわかる。  
***

# ⭐️ タスクの処理がデータベースを使う場合
例えば、ユーザーモデルのインスタンスを作る などデータベースを使う場合は
taskの名称の後に　`:environment`つける。
~~~
[lib/tasks/user.rake]

namespace :user do
  desc 'ユーザーを作る'
  task create_user: :environment do
    User.create(name: 'Tarara')
  end
end

または
task :create_user　=> :environment do
とも書ける。
~~~
⚠️ :environmentはタスクの名称の「:」の位置注意！
***

# 引数渡すタスク
~~~
[引数渡すタスク]

namespace :greet do
  desc 'Hello + nameの表示'
  task :say_hello, [:name] do |_, word|
    puts "HELLO! #{word.name}"
  end
end
~~~
⚠️ Rakeタスクで引数を複数受け取る場合、スペースは開けずにカンマ区切りにする。    
***

### ❓ ブロックの中身の _ はなに？？
まず、タスクに引数を渡す時はシンボルで渡すのが一般的らしい。  
そのため、ブロックを渡すと、ブロックの第一引数は「タスク自体」が入っていて、使わない。  
実際タスクにコードを書いて表示させてみると  
~~~
namespace :hello do
  desc 'hello+name'
  task :hello_name, [:name] do |task, word|
    puts task
  end
end

rails 'hello:hello_name[Tarara]'
=>
hello:hello_name
~~~
となった。  
そして _は通常、ブロック内で使われない変数であることを示すために使われる。  
そのため、ブロックの第一引数を _で書いている。(名前他につけてももちろんいい)  
***

### ❓ なぜブロックの第二引数をそのまま使えないの？？
なぜ `#{word.name}`としているのか？  
`#{word}`ではダメなのか？？  
  
その答えとしては、ブロックの第二引数にはオブジェクトが入るから。  
試しに表示させてみると
~~~
namespace :hello do
  desc 'hello+name'
  task :hello_name, [:name] do |task, word|
    puts word
  end
end

$ rails 'hello:hello_name[Tarara]'
=>
#<Rake::TaskArguments name: Tarara>
~~~
となった。  
そのため `#{word.name}`として抜き出している。
***

# メモ: タスク内で他のタスクを実行する場合
~~~
namespace :tasks do
  task :task_1, [:word] do |_, args|
    puts "Hello, #{args.word}"
  end

  task :task_2 do
    Rake::Task['tasks:task_1'].invoke('Rails')
  end
end
~~~
***

# Rakeタスクの実行
~~~
$ rails タスクのファイル名:タスク名

[例]
$ rails hello:hello_name

[引数渡す場合]
$ rails hello:hello_name[Tarara]

[Zsh環境だとクォーテーションで囲まないと no matches found:出るかも]
$ rails 'hello:hello_name[Tarara]'
~~~
***

# タスクを自動で実行させる
今のままだと、自分でターミナルで叩かないと実行されない。  
コレを自動化するには[コレ](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/whenever.md)が必要。
***
