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

# trait
だいたい一緒だけど少しだけステータスが違うデーターを作るときに使うと便利。    
例えば、あるテストだけ adminにしたい + 名前もadmin_userにしたいなど。
~~~
[traitなし]

FactoryBot.define do
  factory :user do
    sequence(:name) { |n| "user-#{n}" }
    password { 'password' }
    password_confirmation { 'password' }
    role { :writer }
  end
end

let(:user) {create(:user)}
let(:admin) {create(:user, name: 'admin_user, role: admin)}
~~~
traitを使ってない場合は、名前はコレ、権限はコレと()内に定義しなければいけない。
***

## 使い方
~~~
FactoryBot.define do
  factory :user do
    sequence(:name) { |n| "user-#{n}" }
    password { 'password' }
    password_confirmation { 'password' }
    role { :writer }

    trait :admin do
      sequence(:name) { |n| "admin-#{n}" }
      role { :admin }
    end

    trait :editor do
      sequence(:name) { |n| "editor-#{n}" }
      role { :editor }
    end

    trait :writer do
      sequence(:name) { |n| "writer-#{n}" }
      role { :writer }
    end
  end
end


[呼び出す時]
let(:user) {create(:user)}
=> ノーマルユーザー作られる。
let(:admin) {create(:user, :admin)}
=> traitのadminのユーザーが作られる。
~~~
trait :〇〇の、〇〇部分を(:user, :〇〇)とつける感じ
***

# transient
[参考](https://qiita.com/joker1007/items/da63b8630351c1f3fe1d)    
一時的な属性を定義するための機能。    
これを使うと、Factoryで定義したモデルの実際の属性ではないものを一時的に追加できる。    
それ自体はデータベースに保存されないけど、他の属性やコールバック内で利用できる。    

## 使い方
~~~
FactoryBot.define do
  factory :article do
    sequence(:title) { |n| "title-#{n}" }
    sequence(:slug) { |n| "slug-#{n}" }
    category
  end

  trait :with_author do
    transient do
      🧡sequence(:author_name) { |n| "test_author_name_#{n}"}
      💛sequence(:tag_slug) { |n| "test_author_slug_#{n}"}
    end

    #build直後に自由にインスタンスを修正することができる。コールバック。
    🩵after(:build) do |article, evaluator|
      article.author = 💙build(:author, name: evaluator.author_name, slug: evaluator.tag_slug)
    end
  end
~~~
articleモデルには    
🧡 :author_name    
💛 :tag_slug    
この二つの属性(カラム)はないが、transientなら一時的な属性を定義できるので使える。  
***
            
## 🩵 after(:build) / evaluator
記事オブジェクトがビルド（インスタンス化）された直後に実行されるブロック。  
- 第一引数 articleはビルドされた記事オブジェクトを示す。  
- 第二引数 evaluatorは FactoryBotが提供するオブジェクトで、  
trait内で定義された一時的な属性（author_nameと tag_slug）にアクセスするために使用される。

        
💙 build(:author...は authorモデルインスタンス作ってる。  
  アソシエーションしてるから作れる(?)
***

## ❓ コールバックで使える？？
[参考](https://qiita.com/metheglin/items/47116ccbdb26aa00e034)  
callbackを使えば、生成したインスタンスが create, buildされたイベントの直後に自由にインスタンスを修正することができる。  
例えば先ほどのように定義した FactoryBotをテストで呼び出す時に
~~~
let(:article) {create (:article, :with_author, author_name: 'Tarara') }
~~~
と author_nameを test_author_name_#{n}から　Tararaに変更できる。
  
⭐️ callback作る際は after(:create)ではなく、after(:build)にしておいたほうがよさそう。
***
