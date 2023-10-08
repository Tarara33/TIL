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
