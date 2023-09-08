# send
レシーバーにメソッドを渡すメソッド
~~~
[rails c]

$ a = [1, 2, 3]
$ a.length
=> 3
$ a.send(:length)
=> 3
$ a.send("length")
=> 3
~~~
シンボル: lengthや文字列 "length"を sendで渡してみると、いずれも lengthメソッドの呼び出しと同じ結果になった。      
つまり、どちらもオブジェクトに lengthメソッドを渡しているため、等価になる。
***
