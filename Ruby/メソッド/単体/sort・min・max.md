# sort (sort!)
配列の内容をソートする。  
要素同士の比較は <=> 演算子を使って行い、ソートされた配列を生成して返す。 
~~~
[3, 5, 1].sort
=> [1, 3, 5]

["d", "i", "a"].sort
=> ["a", "d", "i"]
~~~
***

## 降順にする場合
`reverse`を使う。
~~~
[3, 5, 1].sort.reverse
=> [5, 3, 1]

["d", "i", "a"].sort
=> ["i", "d", "a"]
~~~
***

## ハッシュでの sort
ハッシュで並び替える場合、キーを基準か値を基準にするか選択する。  

- `hash.sort`  
キーを基準に並び替えて、キーと値を表示する。

- `hash.keys.sort`   
キーを基準に並び替えて、キーのみ表示する。

- `hash.values.sort`  
値を基準に並び替えて、値のみ表示する。
~~~
fruit_stock = {
  "apple" => 5,
  "banana" => 2,
  "date" => 7,
  "cherry" => 10,
}

【hash.sort】
p fruit_stock.sort
=> [["apple", 5], ["banana", 2], ["cherry", 10], ["date", 7]]

【hash.keys.sort】
p fruit_stock.keys.sort
=> ["apple", "banana", "cherry", "date"]

【hash.values.sort】
p fruit_stock.values.sort
=> [2, 5, 7, 10]
~~~
***

# min
最小の要素、もしくは最小の n要素が昇順で入った配列を返す。  
全要素が互いに <=> メソッドで比較できることを仮定している。  

引数を指定しない形式では要素が存在しなければ nilを返す。  
引数を指定する形式では、空の配列を返す。
~~~
[3, 5, 1].min
=> 1

[3, 5, 1].min(2)
=> 1
=> 3

[].min
=> nil

[].min(2)
=> []
~~~
***

## 💡 こんな使い方も
単語の文字数順に並び替える。
~~~
animals = ["wolf", "sheep", "rabbit", "cat"]
p animals.min(2) { |a, b| a.length <=> b.length }
=> ["cat", "wolf"]
~~~
💡 sortメソッドだとこうなる
~~~
# a->z順に並ぶ
["cat", "rabbit", "sheep", "wolf"]
~~~
***

# minメソッドと sortメソッドの違い
minメソッドは最小値1つ (もしくは引数で指定した個数)返すが、sortメソッドは並び替えて配列ごと返す。
***

# max
minの逆。
***
