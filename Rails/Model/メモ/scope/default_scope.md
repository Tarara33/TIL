[参考](https://qiita.com/takaesu_ug/items/f0b3a80111d6bd4ec8b0)

# default_scopeとは
モデルに対する全てのクエリに適用されるデフォルトのスコープ。  
`default_scope { 内容 }`でかく。  
  
たとえば以下のように書くと、そのモデルの全てのクエリ結果が created_atの降順になるようになる。
~~~
[modelファイル]

default_scope { order(created_at: :desc) }
~~~
***

## 例
たとえばこのように Userモデルに定義する。  
「created_atの昇順で並べる」という default_scope
~~~
[app/models/user.rb]

class User < ActiveRecord::Base
  default_scope ->{ order('created_at ASC') }
end
~~~
そして rails cで挙動を確認する
~~~
[rails c]

$ User.limit(10)
# SELECT `users`.* FROM `users` ORDER BY created_at ASC LIMIT10
~~~
***

# ⚠️ default_scopeはあまり良くないとされている
## 理由① default_scopeのオーバライドができない
たとえば、先ほどの Userを updated_atの降順で取得したい。
~~~
[rails c]

$ User.order('updated_at DESC').limit(10)
SELECT  `users`.* FROM `users`  WHERE `users`. ORDER BY 💔created_at ASC, updated_at DESC LIMIT 10
~~~
必ずdefault_scopeの条件が優先的に入ってしまう。
***

## 対応: unscopedを使う
一時的に default_scopeを無効にする unscopedをはさむ必要がある。
~~~
[rails c]

$ User.unscoped.order('updated_at DESC').limit(10)
SELECT  `users`.* FROM `users` ORDER BY updated_at DESC LIMIT 10
~~~
***

## 理由② インスタンス作成時に影響する。
たとえば、先ほどの　default_scopeを、  
「権限が adminの Userを created_atで昇順にする」と変えたとする。
~~~
[app/models/user.rb]

class User < ActiveRecord::Base
  default_scope ->{where(role: "admin").order('created_at ASC')}
end
~~~

そして、新しくUserモデルのインスタンスを作ると...
~~~
[rails c]

$ user = User.new
<User id: nil, name: "", 💔role: "admin", created_at: nil, updated_at: nil>
~~~
なぜかただ、インスタンスを作っただけで権限が adminになっている！！  
たぶん `where(role: "admin")`のせい。
***

#### ということから、適当に使うと痛い目にあう！
