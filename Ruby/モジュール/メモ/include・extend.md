# include / extend
どちらもモジュールを読み込むメソッド。  
読み込んだモジュールのメソッドを、インスタンスメソッドかクラスメソッドのどちらで読み込むかの違い。
  
もし、インスタンスメソッドかクラスメソッドのどちらでも使いたいなら、両方使う。
***

## include
インスタンスメソッドとして読み込む。
~~~
[モジュールファイル]

module Greet
  def hello(name)
    p "こんにちは、#{name}さん"
  end
end


[クラスファイル]

class Human
  include Greet
end


# メソッドの実行
Human.new.hello("山田")
=>  "こんにちは、山田さん"

# クラスメソッドとしての使用はNG
Human.hello
=> エラー
~~~
***

## extend
クラスメソッドとして読み込む。
~~~
[モジュールファイル]

module Greet
  def hello(name)
    p "こんにちは、#{name}さん"
  end
end


[クラスファイル]

class Human
  extend Greet
end


#メソッドの実行
Human.hello("山田")
=>  "こんにちは、山田さん"

# インスタンスメソッドとしての使用はNG
Human.new.hello("山田")
=> エラー
~~~
*** 
