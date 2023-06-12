# エラーメッセージを出す
form画面のviewにかいたりするのが一般的
~~~
[app/view/users/_form.html.erb]
<% if.@user.errors.any? %>
  <div class = "alert-danger"> => bootstrapでCSSしたい場合入れる
    <ul>
      <% @user.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>
~~~
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
