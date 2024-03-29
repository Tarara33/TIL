# link_toメソッド
link_toは`<a>`タグを作るためのヘルパーメソッド    
`<%= link_to "表示文字列", "URL or パス" %>`　と   
第一引数に表示される文字列、第二引数にリンク先にURLもしくはパス
***

## 第二引数に何を使うか
- pathメソッド    
ターミナルで`rails or bin/rails routes`とすると
~~~
Prefix   Verb   URI Pattern                                                                              Controller#Action
    root GET    /                                                                                        tasks#index
   tasks GET    /tasks(.:format)                                                                         tasks#index
         POST   /tasks(.:format)                                                                         tasks#create
new_task GET    /tasks/new(.:format)                                                                     tasks#new
...省略
~~~
routes.rbファイルにあるやつを出してくれるので一番左の`Prefixにある文字＋_path`で作れる。   
また、idが必要な場合も`Prefixにある文字＋_path(インスタンス名)`とする。    
⚠️３番目のPOSTは上のGETと同じPrefixなので省略されてる。

***
- モデルのインスタンスを使う
  - 指定したインスタンスのクラス名をスネークケースに変更  
  - `_path`をつける
***

-コントローラ名 + アクション名で指定する(Prefixない場合)    
`<%= link_to "表示文字列", controller: :posts, action: :show, id: 100 %>`と   
routesのコントローラー名＃アクションのところを使う
***

## オプションつけれる
- classの指定    
bootstrapなど使うときは`<%= link_to "表示文字列", "", class: "btn btn-primary" %>などと書く
***

- targetをつける（別タブで開く）    
link_to で target 属性を指定するには、target: 属性と rel: 属性を記述する     
`<%= link_to "Google 検索！", "https://google.com", target: "_blank", rel: "noopener" %>`
***

- HTTPメソッドをGET以外にする   
~~~
Prefix   Verb   URI Pattern                                                                              Controller#Action
    root GET    /                                                                                        tasks#index
   tasks GET    /tasks(.:format)                                                                         tasks#index
         POST   /tasks(.:format)                                                                         tasks#create
new_task GET    /tasks/new(.:format)                                                                     tasks#new
...省略
~~~
ここでいうVerbがHTTPメソッドにあたる    
デフォルトではGETだがPOSTやDELETEなど変えたいときは`method: :変えたいHTTPメソッド名`とする   
`<%= link_to "削除", post_path(1), method: :delete %>`
***

- confirm ダイアログを表示する    
link_to でこれを実現する場合は、dataオプションに、キー=confirm、値=表示するメッセージとするハッシュを指定する   
`<%= link_to "削除", post_path(1), method: :delete, data: { confirm: '本当に削除しますか？' } %>`
***

- マウスイベント   
マウスイベントに対する Javascript による処理を記述することもできる。    
`<%= link_to "クリック！", onclick: 'alert("メッセージ");' %>`    
onclick以外にも設定できるので調べて使う
***

- 画像にリンクをつける    
⚠️aタグの中に別のタグを含めるように生成する場合は、link_toにブロック(do 〜 end)を渡す必要がある
⚠️これまでは第1引数が表示文字列、第2引数がリンク先だったが、do 〜 end を指定するときは第1引数がリンク先となる
~~~
<%= link_to "URL or パス" do %>
  ここにタグを記述
<% end %>
~~~
***

## path　と　urlの違い
- `_path`...相対パス、redirect_to以外で使用する
- `_url`...絶対パス、redirect_toの時にセットで使用する(HTTPの仕様上、リダイレクトのときに完全なURLが求められるので)
***
