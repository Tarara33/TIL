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
