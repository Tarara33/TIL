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

# 💡 Procオブジェクトの作り方は4種類
1. Proc.new
2. procメソッド
3. ->
4. lambda

`Proc.new`と `proc`は上でまとめたので、　`->`と `lambda`についてまとめる。
***

## ->構文
~~~
【引数がない場合】
-> {puts "HELLO"}

【引数がある場合】
->(a, b(引数リスト)) { a + b(引数の実行)}
~~~

呼び出しは `call`を使う。
~~~
hello_proc = -> {puts "HELLOOOO!"}
hello_proc.call
=> HELLOOOO!

sum_proc = -> (a, b) { puts a + b }
sum_proc.call(5, 19)
=> 24
~~~
***

## lambda
~~~
【引数がない場合】
lambda {puts "HELLO"}

【引数がある場合】
lambda {| a, b | puts a + b }
~~~

呼び出しは `call`を使う。
~~~
hello_proc = lambda {puts "HELLOOOO!"}
hello_proc.call
=> HELLOOOO!

sum_proc = lambda {|a, b| puts a + b }
sum_proc.call(5, 19)
=> 24
~~~
***

### 💡 Proc.new と lambda
lambdaは引数の過不足に厳しい
~~~
【Proc.new】
sum_proc = Proc.new {|a, b| puts a.to_i + b.to_i }
sum_proc.call
=> 0

sum_proc.call(10)
=> 10
--------------------------------------------------------

【lamdda】
sum_proc = lambda {|a, b| puts a.to_i + b.to_i }
sum_proc.call
=> 引数が足りないエラー
~~~
***
