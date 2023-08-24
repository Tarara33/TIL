[参考](https://qiita.com/tyamagu2/items/8abd93bb7ab0424cf084)

# attributesとは
オブジェクトが抱えるデータのことで、属性（attribute）と呼ぶことがある。
例えば Userクラスに name, age, emailなどがあったらそれらのことを属性（attribute）と呼ぶ。
***

# attributesのメソッド
~~~
[User]

create_table "users", force: :cascade do |t|
  t.string "name"
  t.ineger "age"
  t.string "email"
end
~~~
***

## attributes
インスタンス.attrtibutesで「属性名 => 値」のハッシュが取り出せる。
~~~
[rails c ]

user = User.first
user.attributes
=>
{"id"=>1,
 "name"=>"Rails Tutorial",
　　"age"=> 20,
 "email"=>"example@railstutorial.org"}
~~~
***

## attribute_names
インスタンス.属性一覧が取り出せる。
~~~
[rails c ]

user = User.first
user.attribute_names
=>
[id, name, age, email]
~~~
***

# attributes の登録・更新メソッド

## attributes=
特定の attributeを変更する。  
オブジェクトの変更をしただけで、**DBには保存されていない**。  
⭐️ DBに保存する場合は saveを実行する必要がある。  
***

##  assign_attributes
特定の attributeを変更する。  
属性のハッシュを渡すことですべての属性を設定。  
attributes=と同じく **DBには保存されない**。  
⭐️ DBに保存する場合は saveを実行する必要がある。  
[参考](https://railsdoc.com/page/assign_attributes)
***

## update_attibutes
複数のattributeをまとめて変更してDBに**保存する**。   
⭐️ 単数系にするとバリデーションがかからない。  
    
[![Image from Gyazo](https://i.gyazo.com/970730e6a778164129296bba28e65be2.png)](https://gyazo.com/970730e6a778164129296bba28e65be2)    
[参考](https://railsdoc.com/page/model_update_attributes)
***

## attr_accessor  
attr_accessorは、型に縛られず値を入れることができる。[詳しくはココ](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%82%AF%E3%83%A9%E3%82%B9/%E3%82%B2%E3%83%83%E3%82%BF%E3%83%BC%E3%81%A8%E3%82%BB%E3%83%83%E3%82%BF%E3%83%BC.md)   
    
例えば、name, descriptionという属性を持つ Userオブジェクトを定義しようとすると
~~~
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
      t.string :description

      t.timestamps null: false
    end
  end
end
~~~
Railsでは上記の様な migrationファイルを作成するが、
DBを扱わない純粋な Rubyのコードでは下記のように記入ができる。  
~~~
class User
  attr_accessor :name, :description
end
~~~
つまり、型に縛られず値を入れることができる。
***
