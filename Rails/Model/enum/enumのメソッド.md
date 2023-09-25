[参考](https://pikawaka.com/rails/enum#enum%E3%81%AE%E4%BE%BF%E5%88%A9%E3%81%AA%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89)    

# 例題用テーブル
下記のカラムを持つ Userテーブルを用意。  
enumは　genderカラムに設定している。
~~~
[db/schema.rb]

create_table "users", force: :cascade do |t|
    t.string "name", null: false
    t.integer "gender", default: 0, null: false


[app/models/user.rb]

enum gender: {other: 0, man: 1, woman:2}
~~~
***

# enumリスト取得メソッド
`モデル名.enumのカラム名(複数形)`で取得できる。
~~~
[rails c]

$ User.genders
=>{"other"=>0, "man"=>1, "woman"=>2}
~~~
***

# enumの整数値の取得メソッド
`user.gender?`とすると、文字列(manとか)が返ってくるが、１などの整数値が欲しい時に使う。  
`カラム名_before_type_cast`とする。  
~~~  
[rails c]

$ user1 = User.new(name: 'Tarara', gender: 1)
$ user.save

$ user1.gender?
=> "man"
$ user1.gender_before_type_cast
=> 1
~~~
***

# 確認メソッド
今 enumを設定しているカラムに入っている値が何なのか確認するメソッドのこと。  
`インスタンス変数.enumのカラムの値`で確認できる。  
true or falseで返ってくる。  
~~~
[rails c]

$ user1 = User.new(name: 'Tarara', gender: 1)
$ user.save

$ user1.man?
=> true
$ user1.woman?
=> false
$ user1.other?
=> false
~~~
***

# 更新メソッド
今 enumを設定しているカラムに入っている値を別の値に変更するメソッドのこと。  
`インスタンス変数.enumのカラムの値！`で更新できる。 
~~~
[rails c]

$ user1 = User.new(name: 'Tarara', gender: 1)
$ user.save

$ user1.man
=> true

$ user.woman!
$ user.woman?
=> true
~~~
***

# 検索メソッド
enumカラム（今回であれば gender）のデータを検索するメソッドのこと。  
`モデル名.enumのカラム名の値`とする。    
検索かけた enumの値のレコード返ってくる。  
~~~
[rails c]

$ user1 = User.new(name: 'Tarara', gender: 1)
$ user1 = User.new(name: 'Karara', gender: 2)
$ user1 = User.new(name: 'Narara', gender: 0)
$ user1 = User.new(name: 'Parara', gender: 1)
$ user.save

$ User.man
=>
id:1
name: "Tarara"
gender: "man"
id: 4
name: "Parara"
gender: "man"

$ User.woman
=>
id: 2
name: "Karara"
gender: "woman"
~~~
***
