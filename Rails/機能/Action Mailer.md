[参考](https://railsguides.jp/action_mailer_basics.html#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)  
[参考](https://qiita.com/annaaida/items/81d8a3f1b7ae3b52dc2b)  
  
# Action Mailer
Ruby on Rails に組み込まれているメール送信機能のこと。  
Action Mailer を使うと、Ruby on Rails からメールを送信してくれる。  

- メールマガジンの一斉送信  
- ウェブサイトに会員登録した時の thank youメール  
- お問い合わせフォームの記入内容が管理者にメールでも送信される

など用途は幅広い！
***

# Mailer作成
今回は、管理者に対して、記事の投稿数などのメールを送る。
~~~
$ rails g mailer ArticleMailer(メーラークラス名)
~~~
- 大元の ApplicationMailerクラスが「app/mailers/」配下に作られる。  
- 今回使う ArticleMailerクラスが「app/mailers/」配下に作られる。  
- メール文の作成ファイルのテンプレートが「app/views/layouts/」配下に作られる。  
- 今回使う ArticleMailerのメール文作成ファイルを置く「app/views/article_mailer」ディレクトリができる。  
- rspecファイルができる(プレビュー機能で使う)  
***

# ApplicationMailer
全 Mailer共通の設定を書く。
~~~
[app/mailers/application_mailer.rb]

class ApplicationMailer < ActionMailer::Base
  default from: 'from@example.com'
  layout 'mailer'
end
~~~
共通の処理・設定を記述する場合には defaultメソッドを使用する。  
layout はメールの作成文のテンプレートを指定している。  
  
[![Image from Gyazo](https://i.gyazo.com/fcea4624032207f21b41b79b1bfd1f60.png)](https://gyazo.com/fcea4624032207f21b41b79b1bfd1f60)
***

### 💡 ちなみに...
~~~
default from:    "管理人 <from@example.com>"
~~~
こうすると、メールの送信元に '管理人'という文字が入る。
***

# 〇〇Mailer
rails g で作った Mailerクラスのファイルは そのMailer個別の設定をする。
~~~
[app/mailers/article_mailer.rb]

class ArticleMailer < ApplicationMailer
  def repost_summary
    @published_article_count = Article.published.count
    @article_published_yesterday = Article.published_at_yesterday 　　#published_at_yesterdayメソッドは scopeで作成しているもの。
    mail(to: 'admin@exsample.com', subject: '公開記事の集計結果')
end
~~~
@で定義されてる２つのインスタンス変数は、メール文を作る Viewファイルで使われる。  
mailメソッドを使って、宛先(to)と件名(subject)を指定している。
***

#### ⭐️ mailメソッド
メールを送信するための詳細設定を行うことができる。    
メールの宛先、差出人、件名、本文などを指定することができる。  
***


# Mailerのテンプレート
## mailer.text.erb と mailer.html.erb
「app/views/layouts/」配下にあるこの２つのファイル。  
なぜ２種類あるかというと、text型で送るのか html型で送るのかで２つ用意されている。
  
⚠️ レイアウトファイルを削除したり変更してはだめ。
***

#### mailer.text.erb  
- text型。  
- **基本的にコチラが使われる。**
- 全てのメールクライアントで表示が保証されている。    
- 文字しか使えないので、リッチな表現ができないこと    
~~~
[app/views/layouts/mailer.text.erb]

= yield
~~~
***
  
#### mailer.html.erb  
- html型。    
- 画像やリンク、テーブルなどを使ってリッチな表現ができる。  
- 顧客によってhtmlメールを嫌がる（容量が大きい、ガラケーで表示できない等）方がいる。
~~~
[app/views/layouts/mailer.html.erb]

<html>
  <body>
    <%= yield %>
  </body>
</html>
~~~
***

# メール文作成
今回は
- 公開済の記事の総記事数  
- 昨日公開された記事の件数とタイトル  
- もし機能公開されたものがなければ「昨日公開された記事はありません」と表示する。
***

「app/views/article_mailer」ディレクトリ配下に ArticleMailerクラスで作った「repost_summaryメソッド」の名前の入った  
- repost_summary.text.erb  
- repost_summary.html.erb  
を作成する。(どちらか使う方だけの作成でもOK)
~~~
[app/views/article_mailer/repost_summary.text.erb]

公開済の記事数: <%= @published_article_count %>件

<% if @article_published_yesterday.present? %>
  昨日公開された記事数:<%= @article_published_yesterday.count %>件
  <% @article_published_yesterday.each do |article| %>
    タイトル:<%= @article_published_yesterday.title %>
  <% end %>
<% else %>
  昨日公開された記事はありません
<% end %>
~~~
~~~
[app/views/article_mailer/repost_summary.html.erb]

<html>
  <body>
    <p>公開済の記事数: <%= @published_article_count %> 件</p>

    <% if @article_published_yesterday.present? %>
      <p>昨日公開された記事数: <%= @article_published_yesterday.count %> 件</p>
      <ul>
        <% @article_published_yesterday.each do |article| %>
          <li>タイトル: <%= article.title %></li>
        <% end %>
      </ul>
    <% else %>
      <p>昨日公開された記事はありません</p>
    <% end %>
  </body>
</html>
~~~
それぞれ、ArticleMailerの repost_summaryメソッドで定義したインスタンス変数を使っている。
***

# プレビューで確認する
今作ったメール文を確認する方法。

### ① spec/mailers/previews配下のファイルを編集
  rails g で Mailerを作成したときに一緒に作成されている。  
確認したい Mailerのクラス名のファイルを選んで編集する。
~~~
[spec/mailers/previews/article_mailer_preview.rb]

🩵# Preview all emails at http://localhost:3000/rails/mailers/article_mailer
class ArticleMailerPreview < ActionMailer::Preview
  def report_summary
    ArticleMailer.report_summary
  end
end
~~~
確認したいメール文のメソッドを書く。
***

### ② URLへ飛ぶ
🩵部分に書いてある URLへ飛ぶ。
[![Image from Gyazo](https://i.gyazo.com/0d8c0efc097766a22d42e9c580bcb81c.png)](https://gyazo.com/0d8c0efc097766a22d42e9c580bcb81c)
***

# メールの送信
