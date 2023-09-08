# inspect
要求されたオブジェクトを表現する文字列を返す。    
`:name`は、putsすると `name`で出力されるが、    
inspectメソッドつけると`:name`で出力される。    
⭐️ p　と同じ動きをする。
~~~
[rails c]

puts :name
=> name
puts :name.inspect
=> name
p :name
=> name

puts "SSS"
=> SSS
puts "SSS".inspect
=> "SSS"
p "SSS"
=> "SSS"
~~~
***
