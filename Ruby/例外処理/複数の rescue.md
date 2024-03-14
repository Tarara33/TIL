# rescueを複数書くこともできる

## それぞれのエラーオブジェクトで例外処理を分けたい場合
例えば、 ZeroDivisionErrorが発生したらこの例外処理、	  
NoMethodErrorの場合はこの例外処理、など分けることもできる。
~~~
begin
  1 / 0 or "abc".foo
rescue ZeroDivisionError => e
  puts "0で割り算されたよ"
  puts e.class
  puts e.message
rescue NoMethodError => e
  puts "そのメソッドは存在しないよ"
  puts e.class
  puts e.message
end


【beginが 1 / 0の場合】
0で割り算されたよ
ZeroDivisionError
divided by 0

【beginが "abc".fooの場合】
そのメソッドは存在しないよ
NoMethodError
undefined method `foo' for "abc":String
~~~
***

## いくつかのエラーオブジェクトで共通の例外処理をしたい場合
例えば、 ZeroDivisionErrorと NoMethodErrorのエラーオブジェクトで共通の例外処理をさせる場合
~~~
begin
  1 / 0 or "abc".foo
rescue ZeroDivisionError, NoMethodError => e
  puts "0で割り算されたか、そのメソッドは存在しない場合にこのメッセージが出ます"
  puts e.class
  puts e.message
end

【beginが 1 / 0の場合】
--------------------------------
0で割り算されたか、そのメソッドは存在しない場合にこのメッセージが出ます
ZeroDivisionError
divided by 0

【beginが "abc".fooの場合】
0で割り算されたか、そのメソッドは存在しない場合にこのメッセージが出ます
NoMethodError
undefined method `foo' for "abc":String
~~~
***
