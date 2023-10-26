# rails g model　の内容
- string型...文字列
- text型... 長い文字列
- integer...整数
- boolean...true or false(0 or 1)
- references...新しく作成するテーブルのカラムに、作成済みのテーブルを指定する場合に使う   
PRIMARY KEYを持つカラムを参照する（だいたいIDにつけてる）
~~~
$ rails g model post title:string body:text user:references
=> userモデルがPRIMARY KEYをつけている「id」カラムを参照する。
~~~
⚠️「references」使うカラム名は〇〇_idとしない、〇〇とテーブル名だけで、     
そのテーブルのPRIMARY KEY(主キー)がforeign_keyとして追加される。
***

## rails g 実行後
postモデルにて
~~~
[app/model/post.rb]
class Post < ApplicationRecord
  belongs_to :user
end
~~~
アソシエーション（モデルの関連づけ）のための「belongs_to」が自動的に入る。    
⚠️親側の方に「has_many」はつかないので自分で入れる。
***

マイグレーションファイルにて
~~~
class CreatePosts < ActiveRecord::Migration[5.2]
  def change
    create_table :posts do |t|
      t.string :title, null: false
      t.text :body, null: false
      t.references :user, foreign_key: true, null: false

      t.timestamps
    end
  end
end
~~~
「foreign_key（外部キー）」とは指定したカラムの自由な記述を許可せず、指定したカラムの値のみしか使えないようにする制限のこと。    
これが自動でつく。   
「null:false」は自動ではない。
***

