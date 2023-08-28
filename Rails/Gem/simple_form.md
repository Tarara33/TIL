# 機能
フォームを簡単に作れるようになる。
form_withよりも簡単に作れる(らしい)
***

# 導入
~~~
[gemfile]
gem 'simple_form'

$ bundle
~~~
***

## インストール
~~~
$ rails g simple_form:install

# bootstrapを適用する場合は bootstrapオプションを付ける
$ rails g simple_form:install --bootstrap
~~~
***

# 書き方例
~~~
<%= simple_form_for @user do |f| %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.button :submit %>
<% end %>
~~~
form_withと同じで、モデルや URLをつけられる。
⭐️ inputだけでも生成される HTMLには labelもある。
***

# オプション
オプションは色々あるので、[github](https://github.com/heartcombo/simple_form)みると良い。
  
# as
基本的に 「input」を使ってフォームを作成するが、  
textareaだったり、fileや hidden、などタイプを指定したい場合に使う。  
[ここ](https://github.com/heartcombo/simple_form#available-input-types-and-defaults-for-each-column-type)にタイプ色々載ってる。
~~~
<%= f.input :body, as: :text %>
<%= f.input :image, as: :file %>
<%= f.input :image_hidden, as: :hidden %>
~~~
***

## ラジオボタン
モデルで enumを使ってる場合  
~~~
enum eyecatch_align: { left: 0, center: 1, right: 2 }
~~~
フォームではこのように書くと、enumの選択肢出してくれる。  
~~~
<%= f.input_field :eyecatch_align, as: :radio_buttons %>
~~~
[![Image from Gyazo](https://i.gyazo.com/e997d6b929257f612185dcffec67143a.png)](https://gyazo.com/e997d6b929257f612185dcffec67143a)  
  
さらに 「config/locals/enums.ja.yml」などで日本語化していると、日本語で選択肢出してくれる。
~~~
ja:
  enums:
    article:
      eyecatch_align:
        left: '左寄せ'
        center: '中央'
        right: '右寄せ'
~~~
[![Image from Gyazo](https://i.gyazo.com/1805fc9365d9560639d00c33e5f75ba9.png)](https://gyazo.com/1805fc9365d9560639d00c33e5f75ba9)
***

## カレンダーで日付指定
`:date_time_picker`をつける。
~~~
<%= f.input :published_at, as: :date_time_picker %>
~~~
***

# collection
[セレクトボックス](https://github.com/Tarara33/TIL/blob/main/Rails/%E6%A9%9F%E8%83%BD/%E3%82%BB%E3%83%AC%E3%82%AF%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9.md)を作る。    
~~~
[実装例]
<%= f.input :category_id, as: :select2, collection: ['A', 'B', 'C'] %>
<%= f.input :category_id, as: :select2, collection: Category.pluck(:name, :id) %>
~~~
💡 select2はプラグインで見た目を綺麗にしてる。
***

# label
## labelの非表示
`label: false`をつける。
~~~
<%= f.input :title, label: false %>

[生成 HTML]
<input class="form-control string required" type="text" value="ruby on rails" name="article[title]" id="article_title">
~~~
こうすると、生成される HTMLに labelがなくなる。
***

## label名の変更
`label: 'ラベル名'`をつける
~~~
<%= f.input :title, label: 'ラベルだよ' %>

[生成 HTML]
<label class="control-label string required" for="article_title"><abbr title="required">*</abbr> ラベルだよ</label>
<input class="form-control string required" type="text" value="ruby on rails" name="article[title]" id="article_title">
~~~
💡 「config/locals/activerecord.ja.yml」に指定した名前より、オプションの方が優先される。
***

# placeholder
`placeholder: ''`をつける
~~~
<%= f.input :title, placeholder: 'タイトル' %>

[生成 HTML]
<label class="control-label string required" for="article_title"><abbr title="required">*</abbr> タイトル</label>
<input class="form-control string required" placeholder="タイトル" type="text" value="ruby on rails" name="article[title]" id="article_title">
~~~
***

# input_html
`input_html: {追加するもの}`をつける
~~~
<%= f.input :title, input_html: {class: 'TARARA'} %>

[生成 HTML]
<label class="control-label string required" for="article_title"><abbr title="required">*</abbr> タイトル</label>
<input class="form-control string required TARARA" type="text" value="ruby on rails" name="article[title]" id="article_title">
~~~
***

