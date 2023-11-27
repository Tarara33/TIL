# ❓ respond_to
Hotwireを使いたての時に、このような書き方があった。
~~~
[コントローラーファイル]

def show
  respond_to do |format|
    format.html
    format.turbo_stream
  end
end
~~~
私はてっきり、  
「画面の一部更新する部分は、turbo_streamファイルの中身を使い、それ以外は htmlファイルを使う」  
なのでどちらとも使うためにこのように書いているのかと思った。  

結果、そんな感じだが、基本的には書かなくてもよかった。
***

# 💡 リクエストによって読み込むファイルが違う
[リクエストの違い](https://github.com/Tarara33/TIL/blob/main/Rails/%E9%9D%9E%E5%90%8C%E6%9C%9F%E9%80%9A%E4%BF%A1/Hotwire/%E3%83%A1%E3%83%A2/%E3%81%9D%E3%82%8C%E3%81%9E%E3%82%8C%E3%81%AE%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88%E5%BD%A2%E5%BC%8F.md)


[![Image from Gyazo](https://i.gyazo.com/0acdb2d9cc8cde0657c4d63dba0f6b97.png)](https://gyazo.com/0acdb2d9cc8cde0657c4d63dba0f6b97)

Hotwireの Turbo Driveは、HTMLと Turbo Streamの両方のレスポンスに対応していて、  
リクエストの形式に応じて適切なビューテンプレートをレンダリングする。

そして、Railsでは、通常、リクエストのフォーマットに応じて適切なテンプレートを自動的に探してくれるので、  
基本的には、明示的に respond_toで書かなくてもいい。
***

## ❓ 明示的に書くとき
例えば、ユーザーがフォームを送信した後に、フォームの一部だけを更新するような場合。  
Railsの Turbo Driveの機能を使えば、フォーム送信後のリダイレクトを避けて、直接その部分だけを Turbo Streamで更新できる。  
そのためには、respond_toブロック内で format.htmlと format.turbo_streamを使って、  
HTMLリクエストには通常の HTMLレスポンスを、Turbo Streamリクエストには .turbo_stream.erbテンプレートでレスポンスするように処理を分ける必要がある。  

つまり、ユーザーがフォームを送信すると、  
サーバーは Turbo Stream形式で応答して、ページの一部だけを更新することができる。
***

## ❓ どのようにリクエストを Turbo Stream形式にするの？
`<turbo-frame>`タグ内にリンクやフォームを書くか、  
リンクなどに、`data: { turbo_frame: 'id名' }`属性を書く。
~~~
[greet.html.erb]
<div class="container">
  <div class="row">
    <div class="col">
      <h1>挨拶</h1>
    </div>
  </div>

  <div class="row m-3">
  ⭐️<turbo-frame id='hotwire-frame'>
      <div class="col">
        <h3>こんにちは</h3>
        <p>これはお昼の挨拶です。</p>
        <%= link_to 'こんばんは', goodnight_tops_path, class: 'btn btn-primary', data: { turbo_frame: 'hotwire-frame' } %>
      </div>
  ⭐️</turbo-frame>
  </div>
</div>


[goodnight.turbo.stream.erb]
⭐️<turbo-frame id='hotwire-frame'>
  <div class="col">
    <h3>こんばんは</h3>
    <p>これは夜の挨拶です。</p>
    <%= link_to 'こんにちは', greet_tops_path, class: 'btn btn-primary', data: { turbo_frame: 'hotwire-frame' } %>
  </div>
⭐️</turbo-frame>
~~~
⭐️の中身だけ変わる。
***
