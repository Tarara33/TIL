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
逆にあまり触られたくない属性は外すことでセキリュティも守れる。 

これでエラーが出なくなった。
***

# 複数モデルの場合
[ココ](https://github.com/Tarara33/TIL/blob/main/Rails/Gem/ransack/%E8%A4%87%E6%95%B0%E3%81%AE%E6%A4%9C%E7%B4%A2%E6%AC%84.md)で複数モデルの設定方法を書いているが、 
version4.0.0から、アソシエーションモデルへの設定も必要となる。

例えば、以下のように Itemモデルの検索フォームで、Tagモデルも検索するとき。
~~~
[app/views/items/_search_form.html.erb]

<%= search_form_for @q, url: search_items_path do |f| %>
  <%= f.search_field :item_name_cont, class: 'form-control', placeholder: Item.human_attribute_name('item_name') %>
  🧡<%= f.search_field :tags_tag_name_cont, class: 'form-control', placeholder: Tag.human_attribute_name('tag_name') %>
  <%= f.submit '検索', class: 'btn btn-primary' %>
<% end %>
~~~
今までは、「:tags_tag_name_cont」で   
「アソシエーションモデル名_アソシエーションモデルのカラム名_サーチマッチャー」を入れれば　OKだった。 
***

## しかし、version4.0.0からは...
① 親モデルに、`self.ransackable_associations`メソッドで、アソシエーション先の検索対象モデルを追加。   
② 子モデルに、`self.ransackable_attributes`メソッドで、検索対象カラムを追加。 
~~~
[app/models/item.rb]

class Item < ApplicationRecord
  def self.ransackable_attributes(auth_object = nil)
    ["created_at", "id", "item_image", "item_name", "item_url", "memo", "price", "price_range", "target_gender", "updated_at", "user_id"]
  end

 def self.ransackable_associations(auth_object = nil)
     ["tags"]
 end
end


[app/models/tag.rb]

class Tag < ApplicationRecord
 def self.ransackable_attributes(auth_object = nil)
  ["tag_name"]
 end
end
~~~
***
