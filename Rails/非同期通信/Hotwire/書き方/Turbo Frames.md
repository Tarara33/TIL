# Turbo Frames
#### ⭐️ おさらい
Turbo Framesは画面の**一部分**を変更する。

[![Image from Gyazo](https://i.gyazo.com/8a4e9d72720f48df1c95df72c161964f.png)](https://gyazo.com/8a4e9d72720f48df1c95df72c161964f)
***

# 書き方
- `<turbo-frame>`を使う。  
- id を同じにする。
~~~
[hello.html.erb]
<div class="container">
  <div class="row">
    <div class="col">
      <h1>挨拶</h1>
    </div>
  </div>

  <div class="row m-3">
    ⭐️<turbo-frame 🧡id='hotwire-frame'>
      <div class="col">
        <h3>こんにちは</h3>
        <p>これはお昼の挨拶です。</p>
        <%= link_to 'こんばんは', goodnight_tops_path, class: 'btn btn-primary' %>
      </div>
    </turbo-frame>
  </div>
</div>


[goodnight.html.erb]
⭐️<turbo-frame 🧡id='hotwire-frame'>
  <div class="col">
    <h3>こんばんは</h3>
    <p>これは夜の挨拶です。</p>
    <%= link_to 'こんにちは', greet_tops_path, class: 'btn btn-primary' %>
  </div>
</turbo-frame>


[コントローラー]
def hello; end
def goodnight; end
~~~
***

## ヘルパーメソッド
`<turbo-frame>`のヘルパーメソッドは、`<%= turbo_frame_tag %>`    
こちらを使う場合も、idは揃える！
~~~
[hello.html.erb]
<div class="container">
  <div class="row">
    <div class="col">
      <h1>挨拶</h1>
    </div>
  </div>

  <div class="row m-3">
  ⭐️<%= turbo_frame_tag 🧡'hotwire-frame' do %>
      <div class="col">
        <h3>こんにちは</h3>
        <p>これはお昼の挨拶です。</p>
        <%= link_to 'こんばんは', goodnight_tops_path, class: 'btn btn-primary' %>
      </div>
    <% end %>
  </div>
</div>


[goodnight.html.erb]
⭐️<%= turbo_frame_tag 🧡'hotwire-frame' do %>
  <div class="col">
    <h3>こんばんは</h3>
    <p>これは夜の挨拶です。</p>
    <%= link_to 'こんにちは', greet_tops_path, class: 'btn btn-primary' %>
  </div>
<% end %>
~~~
💡 `<%= turbo_frame_tag %>` で生成される HTMLは`<turbo-frame>`なので、  
どちらか片方は`<%= turbo_frame_tag %>`、もう片方は`<turbo-frame>`とお互い揃えなくても動く。
***

# 引数にオブジェクトを渡す
例えば、一覧画面でページ遷移せずに編集したいとき。  

[![Image from Gyazo](https://i.gyazo.com/999367bad55e27567e60640353dd6035.png)](https://gyazo.com/999367bad55e27567e60640353dd6035)

[![Image from Gyazo](https://i.gyazo.com/eb17c8db815300eea5b8894b8702d0d5.png)](https://gyazo.com/eb17c8db815300eea5b8894b8702d0d5)

その場合、上写真の検証でもあるが、「cat_100, cat_99, cat_98...」など idが並んでおり、  
その中で cat_100を編集したい場合、`<turbo-frame id='cat_100'>`とユニーク idの形になる必要がある。

[![Image from Gyazo](https://i.gyazo.com/fed55a25462813b052b3796cd69a5032.png)](https://gyazo.com/fed55a25462813b052b3796cd69a5032)
***

## ユニーク idの付け方
- オブジェクトを渡す。`(<%= turbo_frame_tag オブジェクト名 do %>)`
- `dom_id`をつけて渡す。`(<%= turbo_frame_tag dom_id(オブジェクト名) do %>)`
- 式展開で渡す。`(<%= turbo_frame_tag "オブジェクト名_#{オブジェクト名.id}" do %>)`

以下３つは同じ動きをする。
~~~
[app/views/cats/_cat.html.erb]

<%= turbo_frame_tag cat do %>
<%= turbo_frame_tag dom_id(cat) do %>
<%= turbo_frame_tag "cat_#cat.id}" do %>

⚠️ パーシャルなので @catとしていない。

[生成 HTMLこんな感じ]
<turbo-frame id="cat_1">...</turbo-frame>
~~~
***

## ❓ dom_id
Railsのヘルパーメソッドで、特定の ActiveRecordオブジェクトに対応する DOM要素の IDを生成してくれる。    
これを使うことで、オブジェクトに基づいた一意の IDを持つ HTML要素を簡単に作成できるようになる。  
これを使えば、JavaScriptや CSSでその要素を簡単に特定できるようになる!
***

# リクエストからレスポンスの流れ
1. クライアント側    
`<turbo-frame>`内からのリンクなので、リクエストヘッダーに  
「Turbo-Frame: hotwire-frame」を付与する（これでTurbo Frameリクエストになる）。

2. サーバー側  
サーバー側はリクエストヘッダーに Turbo-Frameが存在すると Turbo Frameリクエストだと判断する。  
Turbo Frameリクエストの場合、高速化のためにレイアウトテンプレート（application.html.erb）のレンダリングはせずに、  
メインテンプレート（hello.html.erb）だけをレンダリングしてレスポンスする。

3. クライアント側  
クライアント側はレスポンス（hello.html.erbのレンダリング結果）を受け取り、`<turbo-frame>`の  
idが一致する`<turbo-frame id="hotwire-frame">`部分（一覧部分）だけを置換する。  
それ以外の部分に関しては、レスポンスはされるが利用されずに捨てられる。
***
