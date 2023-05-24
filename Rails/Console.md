# $rails c で入る

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
