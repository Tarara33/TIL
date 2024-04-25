# alternative
二つ以上のパターンを指定して、どれか一つにマッチしたらマッチとみなす。  
`|`を使って or を表現する。
~~~
case 2
in 0 | 1 | 2
  puts "マッチしました"
end

=> マッチしました
~~~
***

# 他のパターンと組み合わせる
## [arrayパターン](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81%E3%81%AE%E7%A8%AE%E9%A1%9E/array.md)
例)　配列要素が三つ かつ 二番目の要素が4 or 5で数値がマッチするか (それを asパターンを使って変数 mに入れる)
~~~
case [2021, 4, 1]
in [y, 4 | 5 => m, d]
  puts "#{y}年、#{m}月、#{d}日"
end

=> 2021年、4月、1日
~~~
***

## [　hashパターン](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81%E3%81%AE%E7%A8%AE%E9%A1%9E/hash.md)
例)　ハッシュの nameキーの値が Alice or Bob (それを asパターンを使って変数 nameに入れる) かつ ageキーを持つ 
~~~
case {name: "Bob", age: 25}
in {name: "Alice" | "Bob" => name, age:}
  puts "#{name}さん、#{age}歳"
end

=> Bobさん、25歳
~~~
***

# ⚠️ [valiableパターン](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81/%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%83%83%E3%83%81%E3%81%AE%E7%A8%AE%E9%A1%9E/variable.md)とは組み合わせる場合は注意
valiableパターンがマッチした要素を変数に入れるパターンだが、以下のような組み合わせをすると構文エラーになる。
~~~
case [2021, 4, 1]
in [y, m, d] | [y, d]
  puts "マッチ！"
end

=> エラー
~~~
これは異なるパターンを組み合わせることはできないためエラーになる。

ただし、例外的に使わない変数(`_`つく変数)の場合は使える。
~~~
case [2021, 4, 1]
in [_, _, _] | [_, _]
  puts "マッチ！"
end

=> マッチ！
~~~
***

