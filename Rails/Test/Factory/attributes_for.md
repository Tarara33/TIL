# attributes_for
specファイルにてモデルの属性のハッシュを生成するために使用される。

例えばファクトリーファイルに
~~~
[spec/factories/users.rb]

FactoryBot.define do
  factory :user do
    name { "John Doe" }
    email { "john@example.com" }
    password { "password" }
  end
end
~~~
と書いていた場合、specファイルなどで
~~~
[specファイル]

let(:user_attributes) { ⭐️attributes_for(:user) }
~~~
とかくと、user_attributesには `name、email、password` などの値を持つハッシュが代入される。
***
