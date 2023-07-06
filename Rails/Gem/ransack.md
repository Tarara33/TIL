# 機能
検索機能をつける。
[![Image from Gyazo](https://i.gyazo.com/dda13493618f4c0910d89b8dc6bb1ca5.png)](https://gyazo.com/dda13493618f4c0910d89b8dc6bb1ca5)
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'ransack'
$ bundle
~~~
***

# コントローラー編集
例えば、一覧画面で検索する場合は、indexアクション内を編集する。    
~~~
[app/controllers/boards_controller.rb]

def index
  @boards = Board.all
end

これを

def index
  @q = Board.ransack(params[:q])
  @boards = @q.result(distinct: true)
end
~~~
***

### ⭐️:q
ransack の検索条件を指定するためのパラメータ名。  
***

### ⭐️params([:q])
検索フォームなどのユーザー入力がこのパラメータに含まれる。

### ⭐️result
@q の検索条件に基づいてデータベースのクエリを実行し、検索結果を取得。
***

### ⭐️distinct
検索結果から重複したレコードを削除するためのオプション。     
「関連する子テーブルの情報を条件に絞り込んで、親テーブルの検索結果を表示するとき」のケースに必要。    
例えば絞り込み要件が「xxxというコメントがついている掲示板を取得する」場合にdistinct: trueがないと結果がおかしくなる。    
掲示板Aに対して「ruby1」というコメントと「ruby2」というコメントがあったとし、この(distinct: true)を入れなかったとき、    
rubyでコメントを検索した場合に、掲示板Aが2回取得されて検索結果が2件になってしまうからである。
***

## View編集
パーシャルファイル作ると便利。
~~~
[app/views/boards/_search.html.erb]
<%= search_form_for @q, url: url do |f| %>
   <%= f.search_field ⭕️:title_or_body_cont, class: 'form-control', placeholder: '検索ワード' %>
   <%= f.submit '検索', class: 'btn btn-primary' %>
<% end %>


[app/views/boards/index.html.erb]
<%= render 'search', q: @q, url: boards_path %>
~~~
***

### ⭐️search_form_for
railsの from_withとかの ransack版
***

### ⭐️url: url
どこからでも呼び出せる汎用性の高いパーシャルファイルになる。    
検索結果をどの画面(url)に出すかを指定する。
~~~
一覧画面で使うとき
<%= render 'search', q: @q, url: boards_path %>

ブックマーク画面で使うとき
<%= render 'search', q: @q, url: bookmarks_boards_path %>
~~~
***

### ⭐️search_field
text_fieldの ransack版
***

## ⭕️ 検索対象の書き方
基本、**`カラム名_Search Matchers`とかく**    
例では「:title_or_body_cont」と書いているが、    
意味としては、コントローラーでransackメソッドつけてる`(@q = Board.ransack(params[:q]))`    
「Board」の検索対象のカラム名を〇〇を含むという形で検索してる。   
この書き方だと、タイトルと本文いずれから検索文字〇〇が含まれてる投稿を検索する。
***

## Search Matchers
[一覧](https://activerecord-hackery.github.io/ransack/getting-started/search-matches/)    
    
- cont
値を含む。    
例`:title_cont`
    
- イコール
完全一致。    
例`:age_eq`    
    
- titleカラムとbodyカラムに両方に検索文字を含む    
例`:title_and_body_cont`    
***

