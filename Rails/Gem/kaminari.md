[公式](https://github.com/kaminari/kaminari)

# 機能
ページネーションを作る。  
投稿一覧などのデータを複数のページに分割して表示する方法。
[![Image from Gyazo](https://i.gyazo.com/2f9ce27d791865cd6149f9f7edd3193a.png)](https://gyazo.com/2f9ce27d791865cd6149f9f7edd3193a)

※ will_paginateという同じような機能を持つ gemもある。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'kaminari'

$ bundle
~~~
***

## kaminariの設定する
### 方法①　configファイルで細かく設定する
~~~
$ rails g kaminari:config


[config/initializers/kaminari_config.rb]
Kaminari.configure do |config|
  # ①config.default_per_page = 25
  # ②config.max_per_page = nil
  # ③config.window = 4
  # ④config.outer_window = 0
  # ⑤config.left = 0
  # ⑥config.right = 0
  # ⑦config.page_method_name = :page
  # ⑧config.param_name = :page
  # ⑨config.max_pages = nil
  # ⑩config.params_on_first_page = false
end
~~~
コメントアウト消して設定していく
***

### 設定項目
①デフォルトのページあたりの表示件数    
②ページあたりの表示件数の最大（デフォルトは nil、つまり無制限）    
~~~
[例]
Kaminari.configure do |config|
 ①config.default_per_page = 25
 ②config.max_per_page = 10
end
~~~
この場合は、デフォルトでは25件表示できるが、②で10件に制限されてるので、    
実際にページに表示されるのは10件となる。    
    
例えば、何らかの方法でユーザーが100件の投稿を取得した時、    
①の設定はあくまでもデフォルト値なのでイレギュラーがあればそれに従うため 100件取得して表示するが、    
②を設定しておくことで100件取得しても結果として表示は10件になる。      
このようにユーザーが意図しない動きを指定してもサーバーの負荷を守れる。  
***

③表示中のページの左右何ページ分のリンクを表示するかを指定。    
④先頭ページ、及び最終ページから何ページ分のリンクを表示するかを指定。    
⚠️ left、right が指定された場合は、そちらの値が優先される。    
⑤先頭ページから何ページ分のリンクを表示するかを指定。    
⑥最終ページから何ページ分のリンクを表示するかを指定。
~~~
[例]
Kaminari.configure do |config|
  config.window = 4
  config.outer_window = 0
  config.left = 3
  config.right = 2
end
~~~
現在ページが11とするとこうなる。    
[![Image from Gyazo](https://i.gyazo.com/2f9b02afff006d300eb230e7cf589d71.png)](https://gyazo.com/2f9b02afff006d300eb230e7cf589d71)
***
 
⑦モデルに追加されるページ番号を指定するスコープの名前    
デフォルトは「page」
~~~
@posts = Post.page <= これ
~~~
⑧ページ番号を渡すために使用するリクエストパラメータの名前    
デフォルトは「page」
~~~
@posts = Post.page(params[:page] <= これ)
~~~
***

⑨表示するページ数の最大値
[![Image from Gyazo](https://i.gyazo.com/e4c9f5d2b6aedaf50dda29c5570c13b1.png)](https://gyazo.com/e4c9f5d2b6aedaf50dda29c5570c13b1)
***

⑩最初のページでparamsを無視するか、しないか
ちょっとまだ謎
***

### 方法②コントローラーで直接設定
~~~
[app/controllers/board_controller.rb]

def index
  @posts = Post.page(params[:page]).per(20)
=> 表示件数を20で設定
~~~
***

# コントローラー編集
kaminariを使うアクションのモデルに`page(params[:page])`をつける。
~~~
def index
  @users = User.all
end

↓

def index
  @users = User.page(params[:page])
end
~~~
***

# Viewの編集
~~~
$ rails g kaminari:views bootstrap4(フレームワーク名)
=> 用意されてるフレームワークは
bootstrap4
bourbon
bulma
foundation
materialize
pure css ※指定するコマンドはpurecss
semantic ui ※指定するコマンドはsemantic_ui
~~~
これを打つと「app/views/kaminari/」配下にファイルができる。  
⚠️ bootstrapなどバージョンが変わるやつは、最新バージョンあるかなどチェックした方がいい！

~~~
[app/views/board/index.html.erb]
<%= paginate @boards %>
=> ページネーション出すメソッド

<%= page_entries_info @boards %>
=> 「今3ページ目で、201件目から300件目まで表示している」みたいな表示を出すメソッド
~~~
***

# ⭐️GemでもView変えられた
` gem 'bootstrap4-kaminari-views'`　をインストールすることで、    
`$ rails g kaminari:views bootstrap4`　をしなくてもデザインがいい感じになった。    
おおざっぱなデザイン設定ならこれでいいみたい。    
これを使う場合はView編集はこうなる。
~~~
[app/views/board/index.html.erb]
<%= paginate @boards, theme: 'twitter-bootstrap-4 %>
~~~
theme:...特定のテーマを設定するオプション。
***

# 国際化
[i18n](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/i18n/%E5%9F%BA%E6%9C%AC.md)の導入をして、「config/locals/ja.yml」に記載。
~~~
[config/locals/ja.yml]

ja:
  views:
    pagination:
      next: 次へ
      previous: 前へ
      first: 最初
      last: 最後
~~~
***
