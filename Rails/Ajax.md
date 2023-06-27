# Ajax(非同期通信)とは
[参考：Ajax](https://wa3.i-3-i.info/word12672.html)    
JavaScriptがサーバーとやり取りをして、ページを更新することなくデータを保存してページの一部だけ更新することが出来る。
***

# 方法
①remote:true形式
②ajax関数を使った形式
***

# ①remote:true形式
- form_for
- form_tag
- link_to
- button_to
に使える。    
⚠️form_with ではデフォルトで非同期通信をするように設定されている。
***

# リクエストされる形式を調べる
`$ request.format`で見れる。
~~~
class MessagesController < ApplicationController
 def create
    binding.pry # 追加
    @message = Message.new(message_params)
    # 省略
  end
end
~~~
コントローラーファイルにbinding.pry入れてターミナルで`$ request.format`すると
~~~
[JS形式]
[1] pry(#<MessagesController>)> request.format
=> #<Mime::Type:0x00007f91a4ad6730 @hash=-969871837971611020, 
@string="text/javascript", @symbol=:js,
@synonyms=["application/javascript", "application/x-javascript"]>


[HTML形式]
[1] pry(#<MessagesController>)> request.format
=> #<Mime::Type:0x00007f91a4ad6d70 @hash=-2181586491714069490, 
@string="text/html", @symbol=:html, @synonyms=["application/xhtml+xml"]>
~~~
***

# リクエスト形式で呼び出すものが変わる
[![Image from Gyazo](https://i.gyazo.com/f85e0b8787b095dfc162f866e0c2d310.png)](https://gyazo.com/f85e0b8787b095dfc162f866e0c2d310)
JS形式でリクエストを送信した場合は、アクション名.js.erb、    
HTML形式でリクエストを送信した場合は、アクション名.html.erbが呼び出される。
***

# JS形式とHTML形式で処理を分ける場合
`respond_do`を使う。  
~~~
class MessagesController < ApplicationController
  def create
    @message = Message.new(message_params)

    respond_to do |format|
      if @message.save
        ①format.html { redirect_to @message } # showアクションを実行し、詳細ページを表示
        ②format.js  # create.js.erbが呼び出される
      else
        format.html { render :new } # new.html.erbを表示
        format.js { render :errors } # errors.js.erbを表示
      end
    end
  end
end
~~~
通常のifとは違い、saveが成功した場合   
リクエストがHTMLなら①、JSなら②というように   
ここでも処理が分かれる。
***

# jsファイルの編集
[![Image from Gyazo](https://i.gyazo.com/aa9bca2a7cee71f7b67b73c847fbe763.png)](https://gyazo.com/aa9bca2a7cee71f7b67b73c847fbe763)
~~~
[create.js.erb]
$ ('セレクター').メソッド(引数)
~~~
***

# 例：ブックマーク機能のAjax化
[これ](https://github.com/Tarara33/TIL/blob/main/Rails/%E6%A9%9F%E8%83%BD/%E3%83%96%E3%83%83%E3%82%AF%E3%83%9E%E3%83%BC%E3%82%AF.md)をAjax化する。  

## link_to に remote:true つける
~~~
[app/views/boards/_bookmark.html.erb]
<%= link_to board_bookmarks_path(board_id: board.id), id: "js-bookmark-button-for-board-#{board.id}", class: 'float-right', method: :post, remote: true  do %>
  <%= icon 'far', 'star' %>
<% end %>


[app/views/boards/_unbookmark.html.erb]
<%= link_to bookmark_path(current_user.bookmarks.find_by(board_id: board.id)), id: "js-bookmark-button-for-board-#{board.id}", class: 'float-right', method: :delete, remote: true do %>
  <%= icon 'fas', 'star' %>
<% end %>
~~~
***

## jsファイル作る
アクションとしては、「bookmarks#create」,「 bookmarks#delete」なので   
app/views/bookmarks/配下に作る。
~~~
[app/views/bookmarks/create.js.erb]
$("#js-bookmark-button-for-board-<%= @board.id %>").replaceWith("<%= j(render('boards/unbookmark', board: @board)) %>");


[app/views/bookmarks/destroy.js.erb]
$("#js-bookmark-button-for-board-<%= @board.id %>").replaceWith("<%= j(render('boards/bookmark', board: @board)) %>");
~~~
***

### ⭐️ .html() と .replaceWith()
- .html()メソッド...要素の内部HTMLコンテンツを置き換えるために使用され、要素自体および子孫要素も置き換える。
- .replaceWith()メソッド...要素自体を他の要素やHTMLコンテンツで置き換えるために使用され、子孫要素は置き換えない。
***
今回のケースでは  
.html使うと、指定した要素はそのままなので、<a>タグの中にネストしたような形で表示された。
[![Image from Gyazo](https://i.gyazo.com/94cc5e1960a09f73d9932441eae3be18.png)](https://gyazo.com/94cc5e1960a09f73d9932441eae3be18)
***
.replaceWithを使うと、指定要素自体を置き換えるのでネストされずキレイに表示させることができた。
[![Image from Gyazo](https://i.gyazo.com/04595ce1082c11e06094d63635631e16.png)](https://gyazo.com/04595ce1082c11e06094d63635631e16)
***

### ⭐️ jメソッド
引数として与えられたJavaScriptの文字列を適切にエスケープして、JavaScript内で正しく解釈されるようにする。  
***

## bookmarksコントローラーにインスタンス変数入れる
「board」をビューファイル(create.js.erbとdestroy.js.erb)に渡すため、インスタンス変数する。
ajaxで処理を行うので、必要ないredirect_backの記述を削除。
~~~
[app/controllers/bookmarks_controller.rb]
class BookmarksController < ApplicationController

  def create
    @board = Board.find(params[:board_id])
    current_user.bookmark(@board)
  end

  def destroy
    @board = current_user.bookmarks.find(params[:id]).board
    current_user.unbookmark(@board)
  end
end
~~~
***


