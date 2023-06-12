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

