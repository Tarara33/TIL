# 入り方　と　出方
入る：`$ rails c`    
出る：`$ exit`
***

# reload!
いちいち「exit」で出なくても`$ reload!`で再読み込みされる。   
しかし @user = User.last など変数定義してた情報もリセットされる。
***

# new　と　create
例えば新しくUserモデルにデータを登録したいとき
~~~
user = User.new(name: "Taro")
user.save

or

User.create(name: "Taro")
~~~
- new...saveをしないとデータベースに登録されない
- create... save含んでる
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

