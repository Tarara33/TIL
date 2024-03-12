# module_function
モジュールではミックスイン(モジュールをクラスに includeすること)としての使えて、なおかつモジュールの特異メソッドとしても使える。  
一石二鳥なメソッドの定義の仕方。
~~~
module Greet

  def hello(name)
    "Hello, #{name}"
  end

⭐module_function :hello
end


[特異メソッドとして使う]
Greet.hello(doraemon)
=> "Hello, doraemon"


[ミックスインでつかう]
class Human

  def self_introduction
    ⭕️hello ("everyone")
    "nice to meet you!"
  end
end

tarara = Human.new
tarara.self_introduction
=> hello, everyone
    nice to meet you!
~~~
***

## ⚠️ ミックスインされた時は　プライベートメソッド扱いになる
~~~
tarara = Human.new
tarara.hello
=> エラー！ プライベートメソッドなのでインスタンスが直接は使えない
~~~
***
