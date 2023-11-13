[公式](https://github.com/mbleigh/acts-as-taggable-on)

# acts-as-taggable-on
モデルにタグ機能をつけられる gem
***

# 導入
~~~
[gemfile]
gem 'acts-as-taggable-on'

$ bundle
~~~
***

# インストール
~~~
$ rails acts_as_taggable_on_engine:install:migrations
~~~
マイグレーションファイルが 7つほどできる。
***

## ⚠️ マイグレーションファイルの修正
- 「_add_missing_unique_indices.acts_as_taggable_on_engine」
- 「_add_missing_taggable_index.acts_as_taggable_on_engine」

この2つのマイグレーションファイルをコメントアウトして修正する。  
修正しないと SQLiteであれば使用できるが、MySQLへの本番環境のデプロイ時にエラーが発生する。([参考](https://qiita.com/mattan5271/items/726b2f2f1b6655516b9d))
~~~
[_add_missing_unique_indices.acts_as_taggable_on_engine]

# frozen_string_literal: true
# This migration comes from acts_as_taggable_on_engine (originally 2)

class AddMissingUniqueIndices < ActiveRecord::Migration[6.0]
  def self.up
    #add_index ActsAsTaggableOn.tags_table, :name, unique: true

    #remove_index ActsAsTaggableOn.taggings_table, :tag_id if index_exists?(ActsAsTaggableOn.taggings_table, :tag_id)
    #remove_index ActsAsTaggableOn.taggings_table, name: 'taggings_taggable_context_idx'
    #add_index ActsAsTaggableOn.taggings_table,
    #          %i[tag_id taggable_id taggable_type context tagger_id tagger_type],
    #          unique: true, name: 'taggings_idx'
  end

  def self.down
    #remove_index ActsAsTaggableOn.tags_table, :name

    #remove_index ActsAsTaggableOn.taggings_table, name: 'taggings_idx'

    #add_index ActsAsTaggableOn.taggings_table, :tag_id unless index_exists?(ActsAsTaggableOn.taggings_table, :tag_id)
    #add_index ActsAsTaggableOn.taggings_table, %i[taggable_id taggable_type context],
    #          name: 'taggings_taggable_context_idx'
  end
end
~~~
~~~
[_add_missing_taggable_index.acts_as_taggable_on_engine]

# frozen_string_literal: true
# This migration comes from acts_as_taggable_on_engine (originally 4)

class AddMissingTaggableIndex < ActiveRecord::Migration[6.0]
  def self.up
  # add_index ActsAsTaggableOn.taggings_table, %i[taggable_id taggable_type context],
  #            name: 'taggings_taggable_context_idx'
  end

  def self.down
  #  remove_index ActsAsTaggableOn.taggings_table, name: 'taggings_taggable_context_idx'
  end
end
~~~
***

# 使い方
## ① タグをつけるモデルにコードを追加
例えば Postモデルにタグづけをするなら、
~~~
[app/models/post.rb]

class Post < ApplicationRecord
  acts_as_taggable_on :tags
~~~
***

## ② コントローラーのストロングパラメーター編集
タグづけするモデルのフォームに、タグ入力欄を作るので、  
そのモデルのコントローラーのストロングパラメーターに`tag_list`を追加する。

⚠️ `tag_list`以外の名前だとエラー出る。
~~~
[app/controllers/post_controller.rb]

def post_params
  params.require(:post).permit(:title, :body, :tag_list)
end
~~~
***

## ③ フォームの設定
~~~
[app/views/posts/_form.html.erb]

<%= form_with(model: post) do |form| %>
  <div class="form-group form-group_tags">
    <%= form.label 🩵:tag_list, "Tag", class: "form-label" %>
    <%= form.text_field :tag_list, value: 💚post.tag_list.join(","), class: "form-control", 🧡data: { role: "tagsinput" } %>
  </div>
~~~
🩵 ラベル名は`tag_list`

💚 これを書くことで、「,」で区切られた複数のタグが保存される。

🧡 Bootstrap Tags Inputの導入用。
***

# 使えるメソッド
## tag_list
特定のインスタンスについてるタグが見れる。
~~~
[rails c]
$ post = Post.last
$ post.tag_list
=> ["a", "b", "c"]
~~~
***

## tag_list.add("tag_name") / tag_list.remove("tag_name")
特定のインスタンスに新たにタグを追加する。 / 削除する。
~~~
[rails c]
$ post = Post.last
$ post.tag_list
=> ["a", "b", "c"]

$ post.tag_list.add("tarara")
=> ["a", "b", "c", "tarara"]

$ post.tag_list.remove("a")
=> ["b", "c", "tarara"]
~~~
***

## tagged_with("tag_name")
特定のタグ名に関連付けられたモデルのインスタンスを取得。
~~~
[rails c]

$ post = Post.tagged_with("tarara")
=>
#<Post:0x00000001050f1eb8
  id: 4,
  title: "テスト",
  body: "テスト",
  created_at: Mon, 13 Nov 2023 18:14:50.308624000 UTC +00:00,
  updated_at: Mon, 13 Nov 2023 19:02:08.830251000 UTC +00:00,
  user_id: 1,
  tag_list: nil>]
~~~
***

## most_used / least_used
登録数が多い / 少ない 順にタグから取得。(デフォルトは20なので、変更したければ least_used(10)などとする)
~~~
[rails c]

$ most = ActsAsTaggableOn::Tag.most_used
=> 
[#<ActsAsTaggableOn::Tag:0x0000000105930980
...
irb(main):017> most_used_tags = ActsAsTaggableOn::Tag.most_used
  ActsAsTaggableOn::Tag Load (0.7ms)  SELECT "tags".* FROM "tags" ORDER BY taggings_count desc LIMIT ?  [["LIMIT", 20]]
=> 
[#<ActsAsTaggableOn::Tag:0x00000001057b0830
...
irb(main):018> most_used_tags
~~~
***

# Viewでのタグカウント
特定のタグがついてる投稿数。  
`taggings_count`を使う。
~~~
[app/views/posts/show.html.erb]

<div class="tags">
  <% if @post.tags.present? %>
    <div class="d-flex flex-wrap">
      <% @post.tags.each do |tag| %>
  <span class="badge badge-info mr-2 mb-2">
    <%= link_to "#{tag.name}(#{tag.taggings_count})", posts_path(name: tag.name) %>
  </span>
      <% end %>
    </div>
  <% else %>
    <p>登録されているタグはありません</p>
  <% end %>
</div>
~~~
***
