# shift
配列の先頭の要素を取り除いてそれを返す。
引数を指定した場合はその個数だけ取り除き、それを配列で返す。
空配列の場合、n が指定されていない場合は nil を、指定されている場合は空配列を返す。
また、n が自身の要素数より少ない場合はその要素数の配列を返す。
どちらの場合も自身は空配列となる。

~~~
array = ["A", "B", "C", "D", "E", "F", "G", "H", "I", "J"]

array.shift
=> "A"

array.shift(2)
=> ["B", "C"]

array.shift(100)
=> ["D", "E", "F", "G", "H", "I", "J"]
arrayにある残りの要素を入れて 93個分はなし

array.shift
=> nil
空配列なので nil
~~~
***
