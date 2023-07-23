# 入り方　と　出方
入る：`$ rails c`    
出る：`$ exit`
***

# reload!
いちいち「exit」で出なくても`$ reload!`で再読み込みされる。   
しかし @user = User.last など変数定義してた情報もリセットされる。
***

# DBの情報を見る
例えばUserテーブルの情報をコンソール内で作ったり、見たりする。
~~~
[db/schem.rb]
create_table "users", force: :cascade do |t|
    t.string "email", null: false
    t.string "crypted_password"
    t.string "salt"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.string "first_name", null: false
    t.string "last_name", null: false
    t.index ["email"], name: "index_users_on_email", unique: true
  end
~~~
***

## 作る　（new　と　create）
例えば新しくUserモデルにデータを登録したいとき
~~~
$ user = User.new(name: "Taro")
$ user.save

or

$ User.create(name: "Taro")
~~~
- new...saveをしないとデータベースに登録されない
- create... save含んでる
***

## 見る
- 全てのUserインスタンスを見る
~~~
$ User.all
~~~
[![Image from Gyazo](https://i.gyazo.com/ae7ef9f8020825faa91ebb640755fbc4.png)](https://gyazo.com/ae7ef9f8020825faa91ebb640755fbc4)
***

- idが１のUserインスタンスを見る
~~~
$ User.find(1)
~~~
[![Image from Gyazo](https://i.gyazo.com/1eda51117d11420cb1901330ae379c43.png)](https://gyazo.com/1eda51117d11420cb1901330ae379c43)
***

- first_nameに「s」を含むUserインスタンスを見る
~~~
$ User.find_by(first_name:"s")
~~~
[![Image from Gyazo](https://i.gyazo.com/48b3616a7d3d5b6274ea155c4109f97b.png)](https://gyazo.com/48b3616a7d3d5b6274ea155c4109f97b)
***

- テーブルに登録されてる最初と最後のUserインスタンスを見る
~~~
$ User.first
$ User.last
~~~
[![Image from Gyazo](https://i.gyazo.com/9be54afe0407e27a35cbaefdd8735cce.png)](https://gyazo.com/9be54afe0407e27a35cbaefdd8735cce)
***
 
## find_by と find と where
### find
各モデルのidを検索キーとしてデータを取得するメソッド    
⚠️id以外の条件で検索不可、取得したいデータのidの値が具体的に分かっている場合に使用する。
~~~
[rails c]
$ Post.find(1)
=> id 1の返ってくる
~~~
***

### find_by
各モデルをid以外の条件で検索するメソッド(idでも検索可能)   
複数の検索条件を指定可能 (⚠️返ってくる結果は最初にヒットした1件のみ)   
id及びid以外の条件が分かっている場合、その条件に該当する最初のデータを取得したい場合に使用する。
~~~
[rails c]
$ Post.find_by(id: 1)
=> id 1の返ってくる
~~~
***

### where
各モデルをid以外の条件で検索する場合、該当するデータ全てが返ってくる。
~~~
[rails c]
$ Post.where(id: 1)
=> id 1の返ってくる
~~~
***

### 返ってくるクラス
~~~
[rails c]
$ Post.find(1).class
=> Postクラスのオブジェクト

$ Post.find_by(id: 1).class
=> Postクラスのオブジェクト

$ Post.where(id: 1).class
=> ActiveRecord Relationのオブジェクト

$ Post.where(id: 1).last.class
=> Postクラスのオブジェクト
~~~
⚠️whereで探してそのクラスのオブジェクトとして返して欲しいなら「.last, .first」つける
***

## 見る（アソシエーション先の情報）
例えばBoardモデルとアソシエーションしていたらBoardテーブルの情報も見れる。
~~~
reate_table "boards", force: :cascade do |t|
    t.string "title", null: false
    t.text "body", null: false
    t.integer "user_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.string "board_image"
    t.index ["user_id"], name: "index_boards_on_user_id"
  end
~~~
***

- 特定のUserインスタンスのboardを見る
~~~
$ user = User.first
$ user.boards
~~~
[![Image from Gyazo](https://i.gyazo.com/e5e932e961f2837522f06cb5d31052f2.png)](https://gyazo.com/e5e932e961f2837522f06cb5d31052f2)
***

- そのboardたちの中のidが４のboardを見る
~~~
$ user.boards.find(4)
~~~
[![Image from Gyazo](https://i.gyazo.com/43b89f18ec20497288a8338c8f653fdc.png)](https://gyazo.com/43b89f18ec20497288a8338c8f653fdc)
***

- idが４のboardのtitleを見る
~~~
$ user.board.find(4).title
~~~
[![Image from Gyazo](https://i.gyazo.com/603459bd4228726782b4348e88324ee1.png)](https://gyazo.com/603459bd4228726782b4348e88324ee1)
***

# コンソールでエラーメッセージ確認
~~~
[model.rb]
class Post < ApplicationRecord
  validates :title,:body, presence: true
  validates :body, length: {in: 5..140}
end

[rails c]
post = Post.new
post.valid?
=> false
post.errors
=> #<ActiveModel::Errors:0x0000000126e3d3a8 @base=#<Post id: nil, title: nil, body: nil, created_at: nil, updated_at: nil>, @messages={:title=>["can't be blank"], :body=>["can't be blank", "is too short (minimum is 5 characters)"]}, @details={:title=>[{:error=>:blank}], :body=>[{:error=>:blank}, {:error=>:too_short, :count=>5}]}>
入ってるものとエラーの理由両方出る

post.errors.full_messages
=> ["Title can't be blank", "Body can't be blank", "Body is too short (minimum is 5 characters)"]
~~~

# pluck
カラムの中の情報たちを配列にして返してくれる。        
`モデル名.plick(:カラム名)`で定義する。        
~~~
[rails c]
irb(main):002:0> User.pluck(:first_name)
   (1.2ms)  SELECT "users"."first_name" FROM "users"
=> ["s", "拓海", "美咲", "優花", "大輔", "優", "優", "舞", "悠人", "太一", "優菜", "愛美", "優菜", "優花", "輝", "健", "太郎", "優斗", "翔", "美優", "m"]
~~~
***

カラムの中の情報たちを配列にして返してくれる
