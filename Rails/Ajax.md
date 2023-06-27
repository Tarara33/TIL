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
