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
