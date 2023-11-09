# Form Objectのバリデーション
[![Image from Gyazo](https://i.gyazo.com/0feb84163b2f4b0dce9cdf3eb191cca7.png)](https://gyazo.com/0feb84163b2f4b0dce9cdf3eb191cca7)

このように nameは Profileモデルにあり、emailは Userモデルにあるので、  
登録フォームで一気に２つのモデルにそれぞれ値を送信したくて Form Objectを使用した。  

❓ そのときに Form Objectにバリデーションかけたが、いくつか Userモデルや Profileモデルにもかけているものが被った。
***

## 【モデルに書けていたバリデーション】
~~~
[app/models/user.rb]

class User < ApplicationRecord
  validates :email, presence: true, uniqueness: true


[app/models/profile.rb]

class Profile < ApplicationRecord
  validates :name, presence: true, length: {minimum: 6}
~~~
***

## 【FormObjectにかけたバリデーション】
~~~
[app/forms/sign_up_form.rb]

class SignUpForm
  include ActiveModel::Model
  include ActiveModel::Attributes

  # Userモデルの属性
  attribute :email, :string
  attribute :password, :string
  attribute :password_confirmation, :string
  # Profileモデルの属性
  attribute :name, :string

  validates :name, presence: true, length: {minimum: 6}
  validates :email, presence: true
~~~
***

# ⭐️ 役割でバリデーションを分ける！
FormObjectを使うなら、  
- フォーム系のバリデーション(presencesや lengthなど)は FormObjectに実装する。  
- DB系のバリデーション(uniquenessなど)はモデルに実装する。  

なので、モデルファイルと、フォームファイルはこのようになる。

## 【モデルに書くバリデーション】
~~~
[app/models/user.rb]

class User < ApplicationRecord
  validates :email, uniqueness: true


[app/models/profile.rb]

class Profile < ApplicationRecord
end
~~~
💡 フォーム系(presencesや length)を消して、DB系(uniqueness)を残してる。
***

## 【FormObjectに書くバリデーション】
~~~
[app/forms/sign_up_form.rb]

class SignUpForm
  include ActiveModel::Model
  include ActiveModel::Attributes

  # Userモデルの属性
  attribute :email, :string
  attribute :password, :string
  attribute :password_confirmation, :string
  # Profileモデルの属性
  attribute :name, :string

  validates :name, presence: true, length: {minimum: 6}
  validates :email, presence: true
~~~
***

# ❓ モデルのバリデーション消しても平気なの？？
元々、モデルに書くバリデーションは DBとフォームの両方で共通してつかわれている。 

フォーム系のバリデーション(presencesや lengthなど)なら、**ユーザーからの入力を受け取ったとき**に、  
「あ、ここ空欄ですね！長いですね！」とバリア発動してる。  

DB系のバリデーション(uniquenessなど)なら、データベースにアクセスして**保存時**に実際に一意性が検証され、    
「被ってないですね！」とバリア発動してる。  

そして FormObjectのバリデーションはフォーム系なのでモデルのフォーム系のバリデーションと同タイミングで発動される。  
💡 なので、FormObjectに任せても平気！！  

※ フォーム系バリアはモデルと　FormObjectどちらか、または両方に書いても問題はない。(冗長性が多くなる)
***

## ⚠️ DB系バリデーションの場合
フォーム系のバリデーションはモデルと　FormObjectどちらか、または両方に書いても問題はないが、  
DB系のバリデーションは 　FormObjectに書かない方がいい！  

なぜなら元々、DB系のバリデーションは発動のタイミングは、  
データベースにアクセスして保存する時なので、フォームでは発動されない。  
しかし、 FormObjectに書いてしまうと、データベースにアクセスする前にバリデーションが実行され、  
その後にデータベースによる一意性の確認が行われるため、競合状態を引き起こす可能性がある。
***
