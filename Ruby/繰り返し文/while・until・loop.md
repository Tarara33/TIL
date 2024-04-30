#### ⚠️ while, until, loop共に、無限ループになりうるのでちゃんと止まりどころかく。(PCに負担かかる)

# while文
条件式が falseになるまで処理を繰り返し行う。
~~~
num = 15
while num < 20
  p num 
  num += 1
end

[実行結果]
15
16
17
18
19
~~~
***

### 条件を trueにすることもできる(⚠️ break必須)
~~~
num = 0
while true
  puts num
  num += 1
  break if num >= 5
end
~~~
***

# until文
条件式が trueになるまで処理を繰り返し行う。
~~~
num = 15
until num < 10
  p num
  num -= 1
end

[実行結果]
15
14
13
12
11
10
~~~
***

# loop文
無限ループの処理。  
[ループ抜け](https://github.com/Tarara33/TIL/blob/main/Ruby/%E7%B9%B0%E3%82%8A%E8%BF%94%E3%81%97%E6%96%87/%E3%83%AB%E3%83%BC%E3%83%97%E3%82%92%E6%8A%9C%E3%81%91%E3%82%8B.md)ちゃんと書く。
~~~
loop do
  無限ループさせる処理
end
~~~
***
