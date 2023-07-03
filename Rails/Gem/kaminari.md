# 機能
ページネーションを作る。  
投稿一覧などのデータを複数のページに分割して表示する方法。
[![Image from Gyazo](https://i.gyazo.com/2f9ce27d791865cd6149f9f7edd3193a.png)](https://gyazo.com/2f9ce27d791865cd6149f9f7edd3193a)
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'kaminari'
$ bundle
~~~
***

# kaminariの設定する
## 方法①　configファイルで細かく設定する
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
⚠️left、right が指定された場合は、そちらの値が優先される。    
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
 

