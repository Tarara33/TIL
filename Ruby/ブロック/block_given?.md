# block_given?
ブロックが渡されたかどうかを true or false で返すメソッド。
~~~
def foo
  p block_given?
end
----------------

foo
=> false

foo do
end
=> true
~~~

実際のコード    

[![Image from Gyazo](https://i.gyazo.com/eb951a8b232b350a4bac0084d1baa689.png)](https://gyazo.com/eb951a8b232b350a4bac0084d1baa689)
***

# if文とかでも活躍
もしブロック渡されていたら、この処理、、、などと使える。  
💡 [ブロックを引数として渡す]()
~~~
def greet(&block)
  puts "おはよう"

  if block_given?
    puts "好きだよ"
  end

  puts "バイバイ"
end
-----------------------

greet
=> おはよう
=> バイバイ

greet do
end
=> おはよう
=> 好きだよ
=> バイバイ
~~~
***
