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
