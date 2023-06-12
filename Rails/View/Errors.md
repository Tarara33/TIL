# エラーメッセージを出す
form画面のviewやパーシャルファイルにかいたりするのが一般的   
例：　パーシャルファイルに書く場合
~~~
[app/view/shared/_errors.html.erb]
<% if object.errors.any? %>
  <div class = "alert-danger"> => bootstrapでCSSしたい場合入れる
    <ul>
      <% object.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>

[app/view/users/new.html.erb]
<% form_with (model: @user, local: true) do |f|
  <%= render 'shared/errors', obect: f.object %>
=> パーシャルファイルの「object」の部分が「@user.object」になって適応される。
~~~
<%= render 'shared/errors', obect: ＠user %>でもいいが、   
postモデルのフォームに使いたい時は<%= render 'shared/errors', obect: ＠post %>    
など変えなきゃいけないので「f.object」にすると同じ文章で使いまわせる
***

## errors.any?　と　errors.present?
- errors.any?   
エラーコレクションにエラーオブジェクトが少なくとも1つ含まれている場合に true を返す。    
つまり、エラーコレクションが空でないかどうかを確認する。

- errors.present?    
エラーコレクションが空でない場合に true を返すが、    
エラーコレクション内に具体的なエラーメッセージが含まれているかどうかまでは確認しない。   
つまり、エラーコレクションが存在しているかどうかだけを確認する。
***
