# count 
レシーバの要素数を返す。  

- 引数を指定しない場合は、配列の要素数を返す。  
- 引数を一つ指定した場合は、レシーバの要素のうち引数に一致するものの個数をカウントして返す(一致は == で判定)  
- ブロックを指定した場合は、ブロックを評価して真になった要素の個数をカウントして返す。
~~~
[1, 2, 3, 2, 5, 6, 2, 1].count
=> 8

[1, 2, 3, 2, 5, 6, 2, 1].count(2)
=> 3

[1, 2, 3, 2, 5, 6, 2, 1].count { |num| num % 2 == 0 }
=> 4
~~~
***
