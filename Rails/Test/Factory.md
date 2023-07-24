# Factory_bot
テスト用のダミーデーター作れる。

# 導入
~~~
[gemfile]

group :development, :test do
gem 'factory_bot'
end

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
    sex {:man}
    deadline { 1.week.from_now }
  end
end
~~~
- 名前やメールアドレスなど入力系は{""}に文字など書く。        
- 性別など選択系は{:選択肢}で書く。        
- 期限などのカレンダー入力系はメソッド使って{}内に書く。
***

## モデル名以外の書き方
~~~
FactoryBot.define do
  ⭐️factory :testuser, class: User do
    name { "testuser2" }
  end
end
~~~
「:user」とモデル名入れずに違う名前をつけている。    
その場合は classでモデルを指定する。
***

# sequence
uniqunessかけてるカラムに使う（重複しなくなる）

## 使いかた①ブロックタイプ
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

## 使いかた②ノーブロックタイプ
変えたい場所が末尾の場合のみ使える書き方
~~~
[factories/users.ub]

FactoryBot.define do
  factory :user do
    sequence(:name, "ごりら_1")
  end
end
~~~
⚠️ 末尾が数字の場合【のみ】この形式が使える。
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
  ⭐️association user
    または
    user のみでも可
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

# FactoryBot. の省略
~~~
[spec/rails_helper.rb]

config.include FactoryBot::Syntax::Methods
~~~
        
と記載すると、テストを書くところで「FactoryBot.」を省略できる。
~~~
user = FactoryBot.create(:user)
↓
user = create(:user)
~~~
***

# ⚠️ FactoryBotは new使えない
~~~
⭕️user = FactoryBot.create(:user)
⭕️user = FactoryBot.build(:user)
❌user = FactoryBot.new(:user)
~~~
なので、作成・セーブするときは create        
作成だけしてバリデーションなど確認したいときは build使う。
***
