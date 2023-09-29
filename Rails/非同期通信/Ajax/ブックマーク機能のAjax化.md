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
