# find (エイリアス: detect)
ブロックの戻り値が 真になった最初の要素を返す。

[select](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89/%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF/select%20(find_all)%E3%83%BBreject.md)と比較
~~~
[select]
numbers = [1,2,3,4,5,6]
new_number = numbers.select{ |num| num % 3 == 0}
new_number
=> [3, 6]


[find]
numbers = [1,2,3,4,5,6]
new_number = numbers.find{ |num| num % 3 == 0}
new_number
=> 3
~~~
***

## &:使える
~~~
numbers = [1,2,3,4,5,6]
even_number = numbers.find(&:even?)
=> 2
~~~
***
