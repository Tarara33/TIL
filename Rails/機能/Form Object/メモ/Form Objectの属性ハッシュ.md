# Form Objectファイル内に書いてあったハッシュ
Form Objectファイル内にこのように属性値をまとめたハッシュがあった。
~~~
[app/forms/user_form.rb]

class UserForm
  include ActiveModel::Model
  include ActiveModel::Attributes

  attribute :user_id, :integer
  attribute :name,    :string
  attribute :email    :string

  validates :name, :email, presences: true

  def save
    return false if invalid?

    ActiveRecord::Base.transaction do
      user = User.new(user_params)
      user.save!
    end
  rescue StandardError
    false
  end

  private

  def user_params
    {
      user_id: user_id,
      name: name,
      email: email
    }
  end
end
~~~
***

## ❓ 二つのコードは同じ？？
private内で設定しているメソッドはストロングパラメーターではなく、  
属性値を渡すメソッドなので、
~~~
class UserForm
  include ActiveModel::Model
  include ActiveModel::Attributes

  attribute :user_id, :integer
  attribute :name,    :string
  attribute :email    :string

  validates :name, :email, presences: true

  def save
    return false if invalid?

    ActiveRecord::Base.transaction do
      user = User.new(user_id: user_id, name: name, email: email)
      user.save!
    end
  rescue StandardError
    false
  end
end
~~~
このように書いても同じ意味となる。

ただ、最初の書き方の方が、メソッド化してまとめてるのでいろんなとこで使いやすかったり、修正が楽！
***

## ❓ コントローラー側のストロングパラメーターは必要？
**必要！**
***
