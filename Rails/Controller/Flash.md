# フラッシュメッセージを出す
「application_controller.rb」に
~~~
add_flash_types :success, :info, :warning, :danger

など使いたいタイプを記述。
railsのデフォルトはalertとnoticeのみ。
~~~
⚠️Bootstrapに用意されているスタイルのフラッシュを定義してるので、Bootstrapないと出ない。
また、これを定義することで
~~~
redirect_to login_path, flash {sccess: 'ユーザーを作成しました'}
が
redirect_to login_path, sccess: 'ユーザーを作成しました'
に省略できる。
~~~
***

## パーシャルファイル「`_flas_message.html.erb`」を作る
~~~
[_flash_message.html.erb]

<% flash.each do |message_type, message| %> => flashがハッシュ形式なので|key, value|を設定してeachする。
  <div class="alert alert-<%= message_type %>"><%= message %></div>
<% end %>
~~~
上でいう「<%= message_type %>」には中身としては「:success」などが入ってる。    
⚠️「<%=」で表示させないとBootstrapが表示されないという意味になり、色つかないので注意。    
これをレイヤーに読み込ませると
全てのviewに適応されるので、いちいちこれを各viewファイルに書かなくて済む。
***

# flash　と　flash.now
[![Image from Gyazo](https://i.gyazo.com/63bfa5260ed0c7df5a2e4fed28d0f2e7.png)](https://gyazo.com/63bfa5260ed0c7df5a2e4fed28d0f2e7)
redirect_toは次のアクションになるので、「flash.now」だとメッセージ表示見れない。    
renderは指定したviewsを呼び出すだけなので、アクションではない。そのため「flash」にすると予期せぬ動きをする。    
１　createアクションの実行    
２　表示したページでメッセージを表示（1回目）   
３　〇〇アクションの実行（例　ページ遷移）   
４　遷移したページでメッセージを表示（2回目）（寿命を迎える）  => ページ変わってもまだあるよ状態     
５　〇〇アクションの実行（例　ページ遷移）     
６　遷移したページでメッセージは表示されない    
***
