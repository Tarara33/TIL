[公式](https://github.com/zmbacker/enum_help)

# 機能
[enum](https://github.com/Tarara33/TIL/blob/main/Rails/Model/enum/enum.md) の国際化。
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'enum_help'

$ bundle
~~~
***

## 国際化設定
「config/locals/ja.yml」に書く。
~~~
[config/locals/ja.yml]

ja:
  enums:
    user:(enum設定してるモデル名)
      role:(enum設定してるカラム名)
        general: '一般'
        admin: '管理者'
~~~
***

## コンソールで確認
enum_helpを導入すると、便利なヘルパーも追加される。
基本`モデル名.カラム名(複数形)`で使う。  
~~~
[rails c]

$ User.roles
=> {"general"=>0, "admin"=>1}

$ User.roles_i18n
=> {"general"=>"一般", "admin"=>"管理者"}

$ User.roles_i18n.invert
=> {"一般"=>"general", "管理者"=>"admin"}  ※キーとバリューが入れ替わる！
~~~
***

# View(フォームで使う)
性別の enumを例とする。

## ⚠️ フォームの形式によって、リクエストに入れるべき値が違う。
[enum](https://github.com/Tarara33/TIL/blob/main/Rails/Model/enum/enum.md)の部分でも書いてるが、  
登録フォーム と 検索フォーム(Ransack使用) でリクエストに入れるべき値が変わる。  

- 登録フォーム  
`{man: 0, woman: 1}`の文字列の方。
  
- 検索フォーム(Ransack使用)     
`{man: 0, woman: 1}`の整数値の方。
***
  
## セレクトボックス(登録フォームの場合)
ブラウザには日本語、リクエストは英語文字列にする必要がある。  
なので、`User.roles_i18n.invert`した後の`{"男性"=>"man", "女性"=>"woman"}`を使う。  
[セレクトボックス](https://github.com/Tarara33/TIL/blob/main/Rails/View/%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E7%B3%BB/%E3%82%BB%E3%83%AC%E3%82%AF%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9.md)の動きでは、ハッシュ形式だと、キーが表示・値がリクエストに入る。
~~~
<%= form_with model: @user do |f| %>
  <%= f.select :gender, User.genders_i18n.invert, {} %>
~~~
***

## セレクトボックス(検索フォームの場合)
ブラウザには日本語、リクエストは整数値にする必要がある。 
なので mapメソッド使う。
~~~
<%= search_form_for @q, url: users_path do |f| %>
  <%= f.select :gender, User.genders_i18n.invert.map{ |key, value| [key, User.genders[value]] } %>
~~~
`User.genders["man"]`をすると、0が返ってくる。
***
