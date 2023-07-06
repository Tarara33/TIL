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
[app/controllers/posts_controller.rb]

def index
  @posts = Post.all
end

これを

def index
  @q = Post.ransack(params[:q])
  @posts = @q.result(distinct: true)
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
例えば絞り込み要件が「xxxというコメントがついている掲示板を取得する」場合にdistinct: trueがないと結果がおかしくなる。    
掲示板Aに対して「ruby1」というコメントと「ruby2」というコメントがあったとし、この(distinct: true)を入れなかったとき、    
rubyでコメントを検索した場合に、掲示板Aが2回取得されて検索結果が2件になってしまうからである。
***

## View編集




