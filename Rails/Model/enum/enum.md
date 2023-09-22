[参考](https://zenn.dev/gottsu/articles/66b3ebe0f9e5a6#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)  
[国際化はこちら](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/enum_help.md)   
  
# enumとは
enumは、特定のフィールドの値を予め定義した固定のセットから選択するためのデータ型。      
  
例えば Userモデルに性別を表す genderカラムがあったとして、  
その値を'男性'、'女性'、'その他'の3つから選ぶようにするときに役立つ。  
***

# 設定の仕方
## enum使うカラムは integer型にする
enum使うカラムを追加する。　(すでにあればそれ使う)
~~~
$ rails g migration AddGenderToUser gender:integer
~~~
***

## モデルファイルで設定
カラムを持つモデルファイルにて設定。
~~~
[app/models/user.rb]

enum gender: {man: 0, woman: 1}
~~~
これで設定完了！
***

# rails cで確認
~~~
[rails c]

$ user = User.new(gender: 1)
$ user.gender
=> "woman"

$ user = User.new(gender: "woman")
$ user.gender
=> "woman"
~~~
enumで 1=womanと定義づけてるので、登録は gender: 1でも　womanでもできる。
***

# フォーム
## ⚠️ フォームの形式によって、リクエストに入れるべき値が違う。
- 登録フォームの場合
選んだ値を英語の文字列（"admin"や"user"など）としてパラメータにセットする必要がある。  
つまりここでは`{man: 0, woman: 1}`のキー(man, woman)の方。

- 検索フォームの場合
Ransackで enumを検索する場合、パラメータには enumの整数値（0や1など）をセットする必要がある。  
つまりここでは`{man: 0, woman: 1}`の値(0, 1)の方。
***

# ラジオボタンの場合(登録フォーム)
~~~

<%= form_with model: @user do |f| %>
  <div class="mb-3">
    <%= f.radio_button :gender, :man, class: "btn-check", id: "gender-man" %>
    <%= f.label :man, class: "btn btn-outline-primary", for: "gender-man" %>
    <%= f.radio_button :gender, :woman, class: "btn-check", id: "gender-woman" %>
    <%= f.label :woman, class: "btn btn-outline-primary", for: "gender-woman" %>
  </div>
~~~
[![Image from Gyazo](https://i.gyazo.com/91a5a960fbba3f1c4533585c82493cf9.png)](https://gyazo.com/91a5a960fbba3f1c4533585c82493cf9)
***

# セレクトボックス(登録フォーム)
🩵の部分は　`モデル名.カラム名(複数形).keys`となっている。
~~~
<%= form_with model: @user do |f| %>
  <div class="mb-3">
  <%= f.select :gender, 🩵User.genders.keys, {} %>
  </div>
~~~
[![Image from Gyazo](https://i.gyazo.com/2eb64b7f6d4adbeda8acade365db69d8.png)](https://gyazo.com/2eb64b7f6d4adbeda8acade365db69d8)
***

# セレクトボックス(検索フォーム)
ブラウザ表示はキー(man, woman)だが、リクエストは値(0, 1)としなければいけない。    
なので mapを使って、表示とリクエストに入れる形を分ける。  
詳しくは[セレクトボックス](https://github.com/Tarara33/TIL/blob/main/Rails/View/%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E7%B3%BB/%E3%82%BB%E3%83%AC%E3%82%AF%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9.md)で
~~~
<%= search_form_for @q, url: users_path do |f| %>
  <%= f.select :gender, User.genders.map{ |key, value| [key, value] } %>
~~~
***
