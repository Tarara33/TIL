# Chapter3

## ルーティングってどういうものですか？
通信において、どの道を行けば良いか教えてくれる仕組み。   
アプリを使うユーザーの画面から「URL,HTTP」を受け取って、それに対してリクエストを処理する    
コントローラとアクションを特定することをルーティングと呼ぶ。
***

## config/routes.rbでresources :tasksと書くとどのようなことが出来ますか？
resourcesメソッドに含まれる
-　index...一覧
-　show...詳細
-　new...新規作成
-　create...登録
-　edit...編集
-　update...更新
-　destroy...削除    
の７機能が使えるルート一式を定義する    
***

### resourcesのオプション
- only...指定したアクションのみ作成する
- except...指定したアクション以外を作成する
~~~
resources :tasks, only: [:index]
=> indexアクションルートのみ作成

resources :tasks, except: [:delete, :edit, :update]
=> destroy,edit,update以外の４アクションのルートを作成する
~~~
***

## RailsでHTML要素の<a href="#">新規登録</a>をつくりたい時はどういうメソッドを使いますか？
`link_to`というヘルパーメソッドを使う。    
~~~
<%= link_to "新規登録","#" %>
~~~
***

## コントローラーでインスタンス変数を使う時ってどういう時ですか？
アクションからビューに受け渡したいデータがある時。
~~~
[tasks_controller.rb]
def new
  @task = Task.new
end
=> このnewアクションで作ったtaskをビューのnew.htmlでも表示させたい！

[tasks/new.html]
<%= form_with (model:@task, local:true do |f|
  <%= f.label :name %>
  <%= f.text_field :name %>

  <%= f.submit "Create" %>
<% end %>
~~~
***

## コントローラーでブラウザに返したいViewの名前を省略できる時はどういう時ですか？
アクションには役割が２つあり、１つは設定された動きを行うこと（def で定義された動き）    
もう１つはブラウザに画面（view）を返す。    
その返したい画面（view）の記述は省略できるがその場合「app/view/コントローラ名/アクション名.html」の画面を返す。
***

## レンダーとリダイレクトの違いはなんですか？
- render...アクションに続けてビューを表示させる。
- redirect_to...登録処理などアクションの後に、ビューを表示せず、      
続けて別のURLに案内するというアクションを行ってからビューを出す。
***

## コントローラーで書かれるparams[:id]はどういうものですか？
paramsはリクエストパラメーター。   
params[:id]はリクエストから得られるid、つまりリクエストされたURL(〇〇/[タスクのid])の[タスクのid]が格納されてる。
***

## パーシャルってどういうことが出来てどういう時に使いますか？
入力フォームなどビューで同じ部分がある時に共通化させることをいう。   
まず、「app/view/コントローラ名」の中にあるビューファイルのところに新規作成する。     
⚠️ファイル名を`_`から始める。   
`_form.html.erb`などとつけて共通の部分のHTMLを記述。    

呼び出し側は、呼び出したい行で
~~~
<%= render partial:"ファイル名"　%>
または、省略して
<%= render "ファイル名"　%>

例：　<%= render "shared/header" %> (.erbいらない)
~~~
で呼び出せる
***

### オプション
locals: { '部分テンプレート内で使う変数': '変数に入れる値' }
~~~
<%= render partial:"ファイル名",locals: {task: こんにちは} %>
=> テンプレート内の「task」という変数に「こんにちは」が代入される

<%= render partial:"ファイル名",locals: {task: @task} %>
=> テンプレート内の「task」という変数に呼び出し元で定義した変数「@task」が代入される
~~~
⚠️locals使う時は 「partial:」を省略できない
***




