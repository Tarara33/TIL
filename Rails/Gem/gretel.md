[公式](https://github.com/lassebunk/gretel)

# 機能
パンくずリストを作る    
※ 現在ページまでの道のりみたいなやつ
[![Image from Gyazo](https://i.gyazo.com/601f22abd2e726cb848865480e3b5bba.png)](https://gyazo.com/601f22abd2e726cb848865480e3b5bba)
***

# 使い方
## Gem導入
~~~
[gemfile]
gem 'gretel'

$ bundle
~~~
***

## 設定ファイルの作成
~~~
$ rails g gretel:install
~~~
configファイル配下に「breadcrumbs.rb」が作られる。
***

# 設定ファイルの編集
基本的な書き方はコレ
~~~
crumb ":ページ名" do
  link "ビューに表示される名前", "リンクされるURL"
  parent :親のページ名（現在の前のページ）
end
~~~
- ページ名はここで設定したパンくずの名前。    
他のパンくず設定で親を定義するときや、ビューファイルでパンくずを呼び出す際の名前になる。
    
- linkの部分ではパンくずリストに表示されるテキストとリンク先を設定する。
URLは rails routesで調べた〇〇_path使える。  
    
- parentの部分では現在ここで作ったページの親ページは何かを設定する。
***

## 大親の設定
パンくずリストの大元の親を設定する
~~~
[config/breadcrumbs.rb]

crumb :root do
  link "HOME", root_path
end
=> コレの親はいないので parentはつけない。
~~~

アイコンをつけることもできる
[![Image from Gyazo](https://i.gyazo.com/8ef03715c2fb345abef3bfd02de45ce7.png)](https://gyazo.com/8ef03715c2fb345abef3bfd02de45ce7)
~~~
crumb :admin_dashboard do
  link '<i class="fa fa-dashboard"></i> Home'.html_safe, admin_dashboard_path
end
~~~
***

## リンクに貼る URLに idが含まれる場合
詳細ページや編集ページなどで「users/1」など URLに id含む場合は
~~~
[config/breadcrumbs.rb]

crumb :user_show do |user|
  link "#{user.name}さんの詳細, user_path(user)
  parent :root
end
~~~
⚠️ ページの内容やパンくずリストがリクエストによって変わる時は、ブロックを渡して動的に設定する必要がある。
***

## 最後のパンくずの場合
このページより先にページはない場合は linkの URLを**つけない**
~~~
[config/breadcrumbs.rb]

crumb :user_edit do |user|
  link "ユーザー編集"
  parent :user_show
emd
~~~
***

# ビュー設定
パンくずリストを表示させたい箇所に下記のコードを記述する。
だいたい全部のビューに表示させるので「application.html.erb」などに書くのが多い。
~~~
<%= breadcrumbs separator: "区切り文字" %>
~~~
そして、上記の場所に表示されるパンくずを下記のコードのように各ビューファイルに指定する必要がある。
~~~
<% breadcrumb :設定したcrumb名 %>
~~~
💡 設定元は複数形、挿入元は単数系。
***

### ブロック渡しているパンくずリストのビューファイル
~~~
[config/breadcrumbs.rb]

crumb :user_edit do |user|
  link "ユーザー編集", edit_user_path(user)
  parent :user_show
emd
~~~
この場合、挿入元のビューファイルで
~~~
<% breadcrumb :user_edit, @user %>
~~~
と、インスタンス変数渡す。
***


### ⚠️ 区切り文字を > で表示したい
例えば、「>」などで表示したい場合、
~~~
<%= breadcrumbs separator: ">" %>
~~~~
このままだと「>」は HTMLの閉じタグの意味を持つのであまりよろしくない。    
なので今回は特殊文字の&rsaquo;を使用する。
~~~
<%= breadcrumbs separator: "&rsaquo;" %>
~~~
***

