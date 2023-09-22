# Viewでの条件分岐
たとえば、性別カラムに enumを作り、ブラウザで女性の場合はフォームにボタンを出現させるというような実装。
~~~
[app/models/user.rb]

enum gender: {others: 0, man: 1, woman: 2}
~~~
~~~
[viewファイル]

<%= form_with model: event do |f| %>
  <div class="mb-3">
    <%= f.label :title %>
    <%= f.text_field :title, class: 'form-control' %>
  </div>
  🩵<% if current_user.gender == "woman" %>
    <div class="mb-3">
      <%= f.label :only_woman %>
      <%= f.check_box :only_woman %>
    </div>
  <% end %>
  <%= f.submit '登録', class: 'btn btn-primary' %>
<% end %>
~~~
※ gem sorcery使ってるため、 current_user使っている。
***

## ❓ `current_user.gender == 2`ではダメだった
~~~
[らんてくん]

enumを使って設定したとき、その値は内部的には数値で保持されているけど、コード上ではシンボルや文字列として扱うことができるからだよ。  
だから、current_user.gender == "woman"と書くと、enumが自動的に "woman"を 2に変換して比較するんだ。  
でも、current_user.gender == 2と直接数値で比較しようとすると、enumはその数値を "woman"に変換することはできないから、期待通りの結果にならないんだな。
~~~
***
