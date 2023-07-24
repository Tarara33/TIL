# Factory_bot
テスト用のダミーデーター作れる。

# 導入
~~~
[gemfile]
gem 'factory_bot'
$bundle
~~~
***

# ファイルの置き場所
「spec/factories/」配下にデーターを作りたいモデル名のファイルをおく。    
例： spec/factories/user.rb    
    
💡 rails g rspec:model User    
などで叩くと、自動的に factoryファイルもできる。
***

# 書き方
## ベーシックな書き方
~~~
[spec/factories/user.rb]
FactoryBot.define do
  factory :user do
    name { "testuser1" }
    email {"a@exsample.com"}
  end
end
~~~
***

## モデル名以外の書き方
~~~
FactoryBot.define do
  factory :testuser, class: User do
    name { "testuser2" }
  end
end
~~~
「:user」とモデル名入れずに違う名前をつけている。    
その場合は classでモデルを指定する。
***

# sequence
複数のテストデータを生成する際に活用できると便利
~~~
[factories/users.ub]

FactoryBot.define do
  factory :user do
    sequence(:name) { |n| "ごりら#{n}" }
  end
end
~~~
rails cなどで`FactoryBot.create(:user)`されるたびに    
~~~
irb(main):001:0> FactoryBot.create(:user)
=> #<User id: 11, name: "ごりら1", created_at: "2021-08-22 23:32:54", ....
irb(main):002:0> FactoryBot.create(:user)
=> #<User id: 12, name: "ごりら2", created_at: "2021-08-22 23:33:04", ....
~~~
とつくられていく
***

# Factoryデーターを紐付ける
例えば、UserのテストデーターとTaskのテストデーターを作る時、    
~~~
[spec/factories/user.rb]
FactoryBot.define do
  factory :user do
    name { "testuser1" }
    email {"a@exsample.com"}
  end
end


[spec・factories/task.rb]
FactoryBot.define do
  factory :task do
    title { "テスト" }
    body {"テストだよ"}
  ⭐️user
  end
end
~~~
⭐️ このuserは先ほど定義したUserモデルのファクトリー「：user」という名前から、      
自動的に作成されたユーザーオブジェクトが関連付けられるようになっている。
***

## :user(モデル名)以外の書き方をしたデーターとの紐付け
~~~
[spec・factories/task.rb]
FactoryBot.define do
  factory :task do
    title { "テスト" }
    body {"テストだよ"}
  ⭐️association :user, factory: :admin_user
  end
end
~~~
⭐️ こう書くことでUserモデルのファクトリー:admin_userから、    
自動的に作成されたユーザーオブジェクトを関連付ける。
***
