# つまずいたこと
「商品名・価格」を登録できる itemテーブルがあった。  
その後 ジャンル別にアイテムを検索できるように、Genreテーブルを作成して、  
アイテムは一つのジャンルを持つ、ジャンルは複数のアイテムを持つという関係性で一対多の関係を作った。
~~~
[item.rb]

class Item < ApplicationRecord
  belongs_to :genre, optional: true


[genre.rb]
class Genre < ApplicationRecord
  has_many :items
~~~

元々登録されてるアイテムがあったので、  
Itemテーブルに外部キーを持たせるマイグレーションファイル作成時は`null: true`とした。
~~~
class AddForeignKeysToItem < ActiveRecord::Migration[7.0]
  def change
    add_reference :items, :genre, null: true, foreign_key: true
  end
end
~~~

しかし、いざアイテムを新しく作って保存しようとしたら「Genreがありません」とエラーが出た。
***

# 原因は belongs_to
Rails 5以降では、belongs_to関連付けにおいてデフォルトで   
required: true(HTMLや JavaScriptなどで使用され、特定のフィールドがフォームで必須であることを示すためのもの)  
が適用されるようになったから、  
belongs_toの関連付けを持つモデルは、関連するオブジェクトが存在することが必須になる。  
***

# 解決策は...
belongs_toに `optional: true`をつける。
~~~
[item.rb]

class Item < ApplicationRecord
  belongs_to :genre, optional: true
~~~
このようにすると、外部キーが nilであっても DBに保存できるようになる。
***
