[公式](https://github.com/faker-ruby/faker)

# faker
seedファイルでダミーデーターを作ってくれる。

# 使い方
## Gem導入
~~~
[Gemfile]

gem 'faker'

$ bundle
~~~
***

# seedファイルに書く
`require`で fakerを読み込む。
~~~
[db/seeds.rb] 例：Userクラスのインスタンス作りたい

⭐️require 'faker'

10.times do |x|
  User.create!(
    first_name: Faker::Name.first_name,
    last_name: Faker::Name.last_name,
  )
end
~~~
$ rails db:seedを実行すると「Sara, Draco」など本物っぽいデーターが10個できる。  
⭐️ 部分はなくても動く時はあるが、動かないときは入れてみる。  
***
