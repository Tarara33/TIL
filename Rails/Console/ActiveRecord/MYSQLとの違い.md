# 結合テーブルの書き方
このようにアソシエーションされてるテーブルで検索かけるとき。
~~~
[モデルファイル]

カラム: id, title, body
class Post < ActiveRecord::Base
 has_many :post_tags
 has_many :tags, through: :post_tags
end

カラム: id, name
class Tag < ActiveRecord::Base
 has_many :post_tags
 has_many :posts
end

カラム: id, post_id, tag_id
class PostTag < ActiveRecord::Base
 belongs_to :post
 belongs_to :tag　　
end
~~~
***

### Active Recordの場合
アソシエーションされていれば、中間テーブルは joinsしなくていい。
~~~
[rails c]

⭕️ $ Post.joins(:tags)
⭕️ $ Post.joins(post_tags: :tags)
~~~
***

### MYSQLの場合
アソシエーションされていても、中間テーブルを INNER JOIN ON で繋ぐ
~~~
[mysql]

❌ SELECT *
  　FROM post
  　INNER JOIN tag ON post.id = tag.id

⭕️ SELECT *
　　　　　FROM post
　  INNER JOIN ON post_tags ON post.id = post_tags.post_id
　  INNER JOIN tag ON post_tag.tag_id = tag.id
~~~
💡 MYSQLの場合、アソシエーションとかは関係なく、カラムにある同じ idをつなげるので、  
INNER JOIN tag ON post.id = tag.id のような繋げ方ができない。
***
