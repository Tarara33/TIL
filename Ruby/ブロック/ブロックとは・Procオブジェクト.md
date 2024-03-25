## ブロックとは？？
Rubyにおいてブロックは、簡単にいうと「処理のかたまり」で、`do~end`や `{}`で囲まれているもののこと。    
また、ブロックは **引数としても渡せる！**
***

# Procオブジェクト
文字列が Stringクラス、数値が Integerクラスのように、ブロックも Procというクラスである。

ブロックオブジェクトを作るときは `Proc.new`か  
Karnelモジュールの `proc`というメソッドを使う。
~~~
【Proc.newを使用】
hello_proc = Proc.new do
  "HELLO!"
end

⭕️ {}でもOK
hello_proc = Proc.new { puts "HELLO!" }

---------------------------------------------
【procメソッドの使用】
hello_proc = proc do
  "HELLO!"
end

⭕️ {}でもOK
hello_proc = proc { puts "HELLO!" }
~~~
***

# Procオブジェクトの実行
メソッドと同じで作るだけでは実行されない。  
Procオブジェクトを実行するには `call`メソッドを使う
~~~
hello_proc = Proc.new { puts "HELLO!" }

hello_proc.call
=> HELLO!
~~~
***

# 引数を利用するブロックオブジェクトを作る & 実行する
`Proc.new`や`proc`の後に引数を入れる。 (デフォルト値も設定できる。)  
callメソッドでも引数を設定する。
~~~
sum_proc = Proc.new { |a, b| puts a + b }

sum_proc.call(5,8)
=> 13
-----------------------------------------
【デフォルト値の設定】

sum_proc = Proc.new { |a = 3, b = 2| puts a + b }

sum_proc.call
=> 5

sum_proc.call(10)
=> 12

sum_proc.call(1,9)
=> 10
~~~
***
