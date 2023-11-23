[参考](https://zenn.dev/shita1112/books/cat-hotwire-turbo/viewer/abstract)  
  
# Hotwireとは
Rails7から Railsのデフォルトになった、モダンな Webアプリケーションを作るための新しいアプローチ。  
Hotwireを使うと、フォーム・リンクからのリクエストは全て Fetch APIを利用した非同期リクエストになる。  
この fetchに対して、サーバーは HTMLをレスポンスする。
  
- Turbo  
Hotwireの中核となる JavaScriptのライブラリ

- Stimulus  
Turboと相性が良い JavaScriptのライブラリ

- Strada  
Hotwireを使ってモバイルアプリケーションを開発する際に利用するライブラリ
  
という3つの技術から構成される。  
***

### ❓ Ajaxとの違い
どちらもサーバーからデータを非同期に取得するための手段。  
  
Hotwireは HTMLを直接返すのに対して、  
Ajaxは JSONなどのデータ形式を返す点で違う。  
また、Hotwireは Turbolinksや Stimulus.jsなどのフレームワークを組み合わせて、    
SPA(シングル　ページ　アプリケーション)のようなユーザ体験を提供する。  
***

# Turbo
Turboを使うと JavaScriptを書かずに SPA風のアプリケーションを実現できるようになる。
Turboは以下の3つの技術から構成される。

- Turbo Drive
body要素全体を置換する。

- Turbo Frames
body要素の一部のみを更新する（特定の HTMLタグなど）

- Turbo Streams
`Turbo Frames`が一箇所のみの更新なのに対し、複数箇所更新できる

- (Turbo Native)
これは Nativeアプリとのブリッジで利用するもので、Web開発では利用しない。
***

# 使い方
## 導入
JSのライブラリ importmapか esbuildで導入が違う。  
rails7系は デフォルトで gem入ってるので特にインストール必要ないが、必要なものはこんな感じ。
~~~
💛 importmapの場合
[Gemfile]
gem 'importmap-rails'
gem 'turbo-rails'
gem 'stimulus-rails'


🧡 esbuildの場合
[Gemfile]
gem 'turbo-rails'
gem 'stimulus-rails'
+
[app/javascript/application.js]
import "@hotwired/turbo-rails"
~~~
***

# コードの書き方
今回は例として ブックマークボタンを hotwireで実装する。

## ① 部分的に変えたいところを決める
Viewファイルの中から部分的に変えたいところを `turbo_frame_tag`メソッドで囲む。  
~~~
[app/views/items/show.html.erb]

<div id="bookmark-button-<%= item.id %>">
  <% if current_user.bookmark?(item) %>
    <%= render 'bookmarks/unbookmark', {item: item} %>
  <% else %>
    <%= render 'bookmarks/bookmark', {item: item} %>
  <% end %>
</div>
~~~
***

## 定義
コントローラーなどで `respond_to｀メソッドを使い、分岐させる(Ajaxと同じ)

例えば、フォローボタンの「フォロー/アンフォロー」の切り替え
~~~
[app/controllers/follow_controller.rb]

def create
  @user = User.find(params[:followed_id])
  current_user.follow(@user)
  respond_to do |format|
    format.html { redirect_to @user }
    format.turbo_stream
  end
end

def destroy
  @user = Relationship.find(params[:id]).followed
  current_user.unfollow(@user)
  respond_to do |format|
    format.html { redirect_to @user, status: :see_other }
    format.turbo_stream
  end
end
~~~

ここでの
~~~
format.html { redirect_to @user }
format.turbo_stream
~~~
は if elseのようなもので、リクエストのフォーマットが HTML形式なら format.html...を処理、  
リクエストのフォーマットが turbo_stream形式なら format.turbo_streamを読み込む。
***

##  Viewファイル
「アクション名.turbo_stream.erb」とすると、format.turbo_streamが実行される。

なので例では、createと destroyなので
「create.turbo_stream.erb」と「destroy.turbo_stream.erb」を用意する。
~~~
[create.turbo_stream.erb]

<%= turbo_stream.update "follow_form" do %>
  <%= render partial: "users/unfollow" %>
<% end %>
<%= turbo_stream.update "followers" do %>
  <%= @user.followers.count %>
<% end %>


[destroy.turbo_stream.erb]

<%= turbo_stream.update "follow_form" do %>
  <%= render partial: "users/follow" %>
<% end %>
<%= turbo_stream.update "followers" do %>
  <%= @user.followers.count %>
<% end %>
~~~
「id = "follow_form"」の部分を `<%= render partial: "users/unfollow" %>`に置き換えるという意味。
  
#### ⭐️ turbo_stream.update
非同期的な更新アクションを示す方法の一つ。
***

# gemもある
turbo-railsという gemもある。
***
