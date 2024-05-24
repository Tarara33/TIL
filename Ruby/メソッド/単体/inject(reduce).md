# inject (エイリアスは `reduce')
たたみ込み演算を行うメソッド。  
配列やハッシュなどに使える。  
~~~
numbers = [1,2,3,4]
sum = numbers.inject(0) { |result, num| result + n }

puts sum
=> 10
~~~
動きとしては、初回はinjectの引数(今回は 0)がresultに入り、numにはnumbersの中身が順番に入る。   
そして、２回目以降は前回の戻り値が resultに入る。  

なので、
1. result = 0, num = 1
2. result = 1, num = 2
3. result = 3, num = 3
4. result = 6, num = 6

結果 10となる
***

# injectメソッドに渡すシンボル
- (:+)...和集合する
~~~
arr = [1, 2, 3]
arr.inject(:+)
=> 6
~~~

- (:-)...差集合する
~~~
arr = [1, 2, 3]
arr.inject(:-)
=> -4
~~~

- (:*)...積集合する
~~~
arr = [1, 2, 3]
arr.inject(:*)
=> 6
~~~
***
