[公式](https://railsguides.jp/association_basics.html#belongs-to%E3%81%AE%E3%82%AA%E3%83%97%E3%82%B7%E3%83%A7%E3%83%B3-counter-cache)

# counter_cacheとは
従属しているオブジェクトの個数の検索効率を向上させる。  
***

# 例
例えば、アイテムにタグづけしているが、タグ「A」が他に何件のアイテムにタグづけられてるのかを出したい時に便利！

【テーブル関係はこちら】

[![Image from Gyazo](https://i.gyazo.com/6fdf5c7510a109a412eca7c26a0d1479.png)](https://gyazo.com/6fdf5c7510a109a412eca7c26a0d1479)

~~~
[app/models/item.rb]

class Item < ApplicationRecord
  has_many :item_tag_relations, dependent: :destroy
  has_many :tags, through: :item_tag_relations
end


[app/models/tag.rb]

class Tag < ApplicationRecord
  has_many :item_tag_relations, dependent: :destroy
  has_many :items, through: :item_tag_relations
end


[app/models/item_tag_relation.rb]

class ItemTagRelation < ApplicationRecord
  belongs_to :item
  belongs_to :tag

  validates :item_id, uniqueness: {scope: :tag_id}
end
~~~
***

# 実装
## 〇〇_countというカラムを追加
Tagモデルに 「items_count」というカラムを追加する。
~~~
$ rails g migration AddItemsCountToTags items_count:integer default: 0 null: false

↓

[db/migrate/日付_add_items_count_to_tags.rb]

class AddItemsCountToTags < ActiveRecord::Migration[7.0]
  def change
    add_column :tags, :items_count, :integer, default: 0, null: false
  end
end

↓

$ rails db:migrate
~~~
💡 カラム名に決まりはないけど、一般的には`関連モデル名の複数形_count`という命名が使われることが多い。
***

### ❓ どのモデルに countカラムつければいい？？
カウンターキャッシュで countカラムをどのモデルにつけるかは、関連するオブジェクトの数を数えたいモデルを基準に判断する。 

たとえば、Tagがいくつの Itemを持っているかを知りたい場合は、  
Tagモデルに関連する Itemモデルの数を数えたいということで、Tagモデルにカウンターキャッシュ用のカラムを追加する。
***

## belongs_toにカウンターキャッシュオプションつける
元々、カウンターキャッシュオプションは `belongs_to`につける。  
なので、多対多の関係性の場合は中間テーブルの `belongs_to :countカラムがついてるモデル`につける。
~~~
[app/models/item_tag_relation.rb]

class ItemTagRelation < ApplicationRecord
  belongs_to :item
  belongs_to :tag, 🩵counter_cache: :items_count

  validates :item_id, uniqueness: {scope: :tag_id}
end
~~~
***

## 取得
~~~
[rails c]

$ item = Item.last
$ tag = item.tags.last
$ tag.items_count
=> 1
~~~
~~~
[Viewファイル]

<% @item.tags.each do |tag| %>
  <p><%= "#{tag.name}(#{tag.items_count})" %></p>
<% end %>
~~~
***

# 💡 クエリの差
実は、カウンターキャッシュ使わなくても、タグにつくアイテムの件数は取得できる。  
その場合は、Viewファイルで `size`メソッドを使って以下のようにかく。
~~~
[Viewファイル]

<% @item.tags.each do |tag| %>
  <p><%= "#{tag.name}(#{tag.items.size})" %></p>
<% end %>
~~~

そして、この画面を出すのに

[![Image from Gyazo](https://i.gyazo.com/ed8d1fc1684eb2fd4e157015adb5dc72.png)](https://gyazo.com/ed8d1fc1684eb2fd4e157015adb5dc72)

- sizeを使った場合
[![Image from Gyazo](https://i.gyazo.com/bc66b03f11fd41615f3e8881dad04e23.png)](https://gyazo.com/bc66b03f11fd41615f3e8881dad04e23)

- カウンターキャッシュを使った場合
[![Image from Gyazo](https://i.gyazo.com/64c0341741f5a336a67c74d8a8f50a74.png)](https://gyazo.com/64c0341741f5a336a67c74d8a8f50a74)

このように、sizeの場合はCOUNTするためのクエリが発生するが、カウンターキャッシュなら発生しない。
***
