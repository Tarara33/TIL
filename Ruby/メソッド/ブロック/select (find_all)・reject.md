# select (エイリアス: find_all)
各要素に対してブロックを評価してその戻り値が **真**の要素を集めた配列を返す。
~~~
numbers = [1,2,3,4,5]
even_numbers = numbers.select{ |num| num.even? }
even_numbers
=> [2, 4]

[&:の場合]

even_numbers = numbers.select(&:even?)
even_numbers
=> [2, 4]
~~~
***

# reject
各要素に対してブロックを評価してその戻り値が **偽**の要素を集めた配列を返す。
~~~
numbers = [1,2,3,4,5]
even_numbers = numbers.reject{ |num| num.even? }
even_numbers
=> [1, 3, 5]

[&:の場合]

even_numbers = numbers.reject(&:even?)
even_numbers
=> [1, 3, 5]
~~~
***
