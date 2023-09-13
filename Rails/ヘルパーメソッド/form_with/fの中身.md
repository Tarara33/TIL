# f.につけるもの

## `text_field`    
一行タイプ（string型）    
***

## `text_area`    
複数行タイプ(text型)    
***

## `password_field`    
パスワード入力
    
[![Image from Gyazo](https://i.gyazo.com/cc8462addcc50750b214bd51ffae4864.png)](https://gyazo.com/cc8462addcc50750b214bd51ffae4864)
***
    
## `file_field`    
ファイルのアップロード用のフィールドを作成    
***

## `hidden_field`    
ユーザーには表示されないが、サーバーサイドで処理する必要があるデータをフォームに含める場合に使用。
***

## `select`
セレクトボックス作成        
詳しくは[コチラ](https://github.com/Tarara33/TIL/edit/main/Rails/%E6%A9%9F%E8%83%BD/%E3%82%BB%E3%83%AC%E3%82%AF%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9.md)    
        
[![Image from Gyazo](https://i.gyazo.com/47e3ec877cdb917303245b560a0d4769.png)](https://gyazo.com/47e3ec877cdb917303245b560a0d4769)
***

# `hidden_field` と `hidden_field_tag`
hiddenなので両方とも隠しフィールドだが、保存の仕方が少し違う。  

## hidden_fieldの特徴
オブジェクト指向のフォームヘルパーで、特定のモデルに関連する値を扱う時に使う。    
例えば、f.hidden_field :idって書くと、そのフォームのモデルの idを隠しフィールドとして生成する。    
保存のされ方は params[:user(モデル名)][:id]
***

## hidden_field_tag
汎用的なフォームヘルパーで、どんな値でも扱える。
例えば、hidden_field_tag :email, @user.emailって書くと @user.emailの値を持つ、名前が'email'の隠しフィールドを生成する。
保存のされ方は、params[:email]
***

## 例
例えば以下のような        
パスワードリセットのリンクを押して、再設定用のページ(editアクション)のコード。    
        
メールアドレスをキーとしてユーザーを検索するために、editアクションと updateアクションの両方でメールアドレスが必要になる。    
パスワード再設定用のメールのリンクに userの emailも入れてるおかげで、    
editアクションでメールアドレスを取り出すことについては問題ない。    
しかしフォームを一度送信してしまうと、この情報は消えてしまう。この値はどこに保持しておくのが適切か。

=> hidden_field_tagで　emailと言う隠しフィールドに @user.emailを入れておく。
~~~
[form画面]

<%= form_with(model: @user, url: password_reset_path(params[:id])) do |f| %>
  <%= render 'shared/error_messages' %>

  <%= hidden_field_tag :email, @user.email %>

  <%= f.label :password %>
  <%= f.password_field :password, class: 'form-control' %>

  <%= f.label :password_confirmation, "Confirmation" %>
  <%= f.password_field :password_confirmation, class: 'form-control' %>

  <%= f.submit "Update password", class: "btn btn-primary" %>
 <% end %>
~~~
***
