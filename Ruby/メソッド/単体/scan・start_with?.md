# scan
引数で指定した　文字や正規表現がマッチしているかみる。  
マッチしていた場合は、配列で返す。
~~~
"foobar".scan("o")
=> ["o", "o"]

"Tarara".scan("Ta")
=> ["Ta"]

"123 456".scan(/\d+/)
=> [123, 456]
~~~
***

# +a いくつ指定要素があったか知りたい
例えば"KANAZAWA"の中に"A"がいくつあるのか知りたい場合、  
scanメソッドは指定要素を配列で返すので、sizeメソッドなどで長さを調べれば"A"の個数がわかる。
~~~
"KANAZAWA".scan("A").size

=> 4
~~~
***

# start_with?
先頭が引数のいずれかであるとき trueを返す。  
引数に正規表現も使える。
~~~
"string".start_with?("str")
=> true

"string".start_with?("ing")
=> false

"string".start_with?("ing", "str")
=> true

"string".start_with?(/\w/)
=> true

"string".start_with?(/\d/)
=> false
~~~
***
