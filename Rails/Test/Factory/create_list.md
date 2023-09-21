# create_list(作る数)
FactoryBotの便利なメソッドの1つで、指定されたファクトリから複数のオブジェクトを生成してデータベースに保存するために使用される。
~~~
FactoryBot.define do
  factory :user do
    name { "Tarara" }
    sequence(:email) { |n| "tarara#{n}@example.com"}
  end
end

$ FactoryBot.create_list(:user, 5)
=>
1. name: "Tarara", email: "tarara1@example.com"
2. name: "Tarara", email: "tarara2@example.com"
3. name: "Tarara", email: "tarara3@example.com"
4. name: "Tarara", email: "tarara4@example.com"
5. name: "Tarara", email: "tarara5@example.com"
~~~
***

## ❓ 二つの違い
上の例のような FactoryBotの :userの定義なら 
  
- 5.times do FactoryBot.create(:user) end      
- FactoryBot.create_list(:user, 5)
  
の結果は同じ？?  
    
=> 同じ！
***
