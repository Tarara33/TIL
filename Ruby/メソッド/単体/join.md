# join
文字列を結合する。
~~~
str = ['a', 'b', 'c']
str.join
=> "abc"
~~~
***

## 数値も結合できる
`to_s`メソッドが使われるので、文字列・数値の混ざった配列もOK
~~~
array = ['a', 1, 'Z', 2]
array.join
=> "a1Z2"
~~~
***

## 引数繋げたい文字列を指定
~~~
str = ['a', 'b', 'c']

str.join('')
=> "abc"

str.join(' ')
=> "a b c"

str.join('-')
=> "a-b-c"

str.join('Z')
=> "aZbZc"
~~~
⚠️ 引数に数値はエラー出る。
***
