# RuntimeErrorが出た
いつものように、コントローラーに @qを作り、 検索フォームを作ったところ、エラーが出た。
~~~
[app/controllers/items_controller.rb]

class ItemsController < ApplicationController
 
  def index
    @q = Item.ransack(params[:q])
    @items = @q.result(distinct: true).includes(:tags).order(created_at: :desc)
  end


[app/views/items/_search_form.html.erb]

<%= search_form_for @q, url: items_path do |f| %>
<%= f.search_field :item_name, class: 'form-control', placeholder: '商品名' %>
   <%= f.submit '検索', class: 'btn btn-primary' %>
<% end %>
~~~
***

# 💡 ransack4.0.0から、モデルに設定が必要
[![Image from Gyazo](https://i.gyazo.com/9ea7a70d972deea55e8302c97c27c23b.png)](https://gyazo.com/9ea7a70d972deea55e8302c97c27c23b)

エラー画面で表示されてるメソッドを、対象モデルに貼り付けた。
~~~
[app/models/item.rb]

class Item < ApplicationRecord
  def self.ransackable_attributes(auth_object = nil)
    ["created_at", "id", "item_image", "item_name", "item_url", "memo", "price", "price_range", "target_gender", "updated_at", "user_id"]
  end
end
~~~
ここの配列に入ってるものを検索可能にするようだ。  
これでエラーが出なくなった。
***
