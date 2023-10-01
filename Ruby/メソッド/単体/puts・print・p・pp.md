# puts
- 末尾に改行が入る。
- 配列の場合、要素ごとに改行する。
- 戻り値は nil
~~~
[irb]

>> puts 'abc'
abc
=> nil

>> puts 123
123
=> nil

>> puts a = [1, 2, 3]
1
2
3
=> nil
~~~
***

# print
- 末尾に改行が入らない。
- 配列の場合、要素ごとに改行しない。
- 戻り値は nil
~~~
[irb]

>> print 'abc'
abc=> nil

>> print 123
123=> nil

>> print a = [1, 2, 3]
[1, 2, 3]=> nil
~~~
***

# p
- 末尾に改行が入る。
- 文字列の場合ダブルクオートで返す。
- 配列の場合、要素ごとに改行しない。
- 戻り値は引数のオブジェクト
~~~
[irb]

>> p 'abc'
abc
=> "abc"

>> p 123
123
=> 123

>> p a = [1, 2, 3]
[1, 2, 3]
=> [1, 2, 3]
~~~
***

# pp
pとほぼ同じだが、複雑な配列などを見やすくしてくれる。
~~~
p Encoding.aliases.take(5)
[["BINARY", "ASCII-8BIT"], ["CP437", "IBM437"], ["CP720", "IBM720"], ["CP737", "IBM737"], ["CP775", "IBM775"]]
=> [["BINARY", "ASCII-8BIT"], ["CP437", "IBM437"], ["CP720", "IBM720"], ["CP737", "IBM737"], ["CP775", "IBM775"]]

pp Encoding.aliases.take(5)
[["BINARY", "ASCII-8BIT"],
 ["CP437", "IBM437"],
 ["CP720", "IBM720"],
 ["CP737", "IBM737"],
 ["CP775", "IBM775"]]
=> [["BINARY", "ASCII-8BIT"], ["CP437", "IBM437"], ["CP720", "IBM720"], ["CP737", "IBM737"], ["CP775", "IBM775"]]
~~~
⚠️ 自分で作った多次元配列などには反応しない。
***

# その他
- printf...書式付きで文字列を表示する
- sprintf...文字列を返す
***
