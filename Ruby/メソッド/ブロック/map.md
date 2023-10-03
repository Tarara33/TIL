# map (エイリアス collect)
各要素に対してブロックを評価した結果を新しく配列にして返す。
~~~
numbers = [1,2,3,4,5]
new_numbers = numbers.map{ |num| num * 10 }
new_numbers
=> [10, 20, 30, 40, 50]
~~~~
***

## &:使える
もしブロック内での処理にメソッドを使うなら`&:`で省略できる。
~~~
numbers = [1,2,3,4,5]
str_numbers = numbers.map{ |num| num.to_s }
str_numbers
=> ["1", "2", "3", "4", "5"]

↓

str_numbers = numbers.map(&:to_s)
str_numbers
=> ["1", "2", "3", "4", "5"]
~~~
⚠️ &:にすると{}ではなく（）になる。
***
