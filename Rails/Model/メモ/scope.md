# scope
モデル側であらかじめ特定の条件式に対して名前をつけて定義し、その名前でメソッドの様に条件式を呼び出すことが出来る仕組みのこと。
***

# 定義の仕方
## 基本
scopeメソッドの第一引数に条件式を呼び出すための名前、第二引数に条件式を実装する lambda(関数)を渡す。
~~~
[モデルファイル]

class モデル名 < ApplicationRecord
  scope :スコープの名前, -> { 条件式 }
end
~~~
~~~
[例: 現在時刻よりも過去の記事を取得したい場合]

scope :past_published, -> { where('published_at <= ?', Time.current) }
~~~
***

## 引数を使う方法
~~~
[モデルファイル]

class モデル名 < ApplicationRecord
  scope :スコープの名前, -> (引数){ 条件式 } 
end
~~~
~~~
[例: 公開記事を取得する上限をつけたい場合]

class Blog < ApplicationRecord 
  scope :published, -> (count(引数)){ where(published: true).limit(count) }
end
~~~
***

## 条件分を使う方法
条件分岐もできる。
~~~
[モデルファイル]

class Blog < ApplicationRecord
  scope :published, -> (count){ where(published: true).limit(count) if count < 5 }
end
~~~
***

# nilの場合
scopeメソッドでは、条件式が nilを返す場合、allメソッドを実行する。    
なので、例えば先ほどの条件式のこのコードは、もし記事が５件以上あったら allメソッドで全件取得される。
~~~
scope :published, -> (count){ where(published: true).limit(count) if count < 5 }
~~~
***

# クラスメソッドとの違い
scopeじゃなくても、クラスメソッドとしてdef ~ endで定義してもいい。  
相違点は、scopeメソッドとクラスメソッドは、nilを返す時の挙動が違う。  
  
- scopeメソッド  
nilの場合にallメソッドが実行されるので、メソッドチェーンを使う事ができる。  
    
- クラスメソッド  
nilの場合はnilが返るので、メソッドチェーンを使う事が出来ない。  
  
⭐️ scopeメソッドの引数は、クラスメソッドの機能を複製させたものなので、引数を使う場合はクラスメソッドを使う方が推奨される。
***
