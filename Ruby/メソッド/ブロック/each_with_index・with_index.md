# each_with_index ・ with_index
通常、eachや mapなどはブロックパラメーター( |この部分| )  
にレシーバーの要素が入るが、インデックスも欲しいときはこれらを使う。

eachで使うときは each_with_indexを使う。  
each以外のメソッドでは with_index使う。
***

# 使い方
## 基本
~~~
fruits = ["apple", "banana", "peach"]
fruits.map.with_index { |fruit, i| "#{i}: #{fruit}" }
=> ["0: apple", "1: banana", "2: peach"]
~~~
***

## 条件文との併用
例: 名前に aを含む　かつ　インデックスが奇数のものは削除される。
~~~
fruits = ["apple", "banana", "peach", "melon"]
fruits.delete_if.with_index { |fruit, i| fruit.include?('a') && i.odd? }
=> ["apple", "peach", "melon"]
~~~
***

## インデックスを0以外から始める
引数に始めたい数値を入れる。
~~~
fruits = ["apple", "banana", "peach"]
fruits.map.with_index(10) { |fruit, i| "#{i}: #{fruit}" }
=> ["10: apple", "11: banana", "12: peach"]
~~~
⚠️ each_with_indexだとインデックスのスタート数値変えれないので、`each.with_index`で行う。
***

