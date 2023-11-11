# 既存の Userモデルに sorceryを機能を追加する。
1. gem "sorcery" を入れて `$ bundle` する。
2. `$ rails g sorcery:install` をする
3. マイグレーションファイルを編集する。
~~~
[マイグレーションファイル]

class SorceryCore < ActiveRecord::Migration[7.0]
  def change
     create_table :users do |t|
       t.string :email,            :null => false
       t.string :crypted_password
       t.string :salt

       t.timestamps                :null => false
     end

     add_index :users, :email, unique: true
  end
end

↓

class SorceryCore < ActiveRecord::Migration[6.0]
  def change
    add_column :users, :email, :string, null: false
    add_column :users, :crypted_password, :string
    add_column :users, :salt, :string
    # ... その他必要なカラム ...

    add_index :users, :email, unique: true
  end
end
~~~
すでに Userモデルが存在するので、 create_tableのままだと、エラー出る。  
そのため、既存の Userモデルにカラム追加のような書き方をする。

4. `$ rails db:migrate`
5. Userモデルに `authenticates_with_sorcery!`がついてることを確認！
***
