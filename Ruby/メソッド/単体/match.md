# match
正規表現で使うメソッド。  
文字列がマッチすると MatchDataオブジェクトが返る。  
また、キャプチャ化していると、それぞれを抜き出すこともできる。
~~~
text = "私の誕生日は1234年56月78日です。"

数字を抜き出す。+ グループ化させる正規表現
/(\d+)年(\d+)月(\d+)日/

/(\d+)年(\d+)月(\d+)日/.match(text)
または
text.match(/(\d+)年(\d+)月(\d+)日/)
=> #<MatchData "1234年56月78日" 1:"1234" 2:"56" 3:"78">

p[0]
=> "1234年56月78日"
p m[1]
=> "1234"
p m[2]
=> "56"
p[3]
=> "78"
p[4]
=> nil
~~~
***

## キャプチャに名前ついてる場合
数字ではなく、キャプチャ名でも抜き出せる。  
その際は、**シンボル型**にする。
~~~
text = "私の誕生日は1234年56月78日です。"
text.match(/(?<year>\d+)年(?<month>\d+)月(?<day>\d+)日/)
=> #<MatchData "1234年56月78日" year:"1234" month:"56" day:"78">

p[:year] ❗️シンボル型
=> "1234"
~~~
***
