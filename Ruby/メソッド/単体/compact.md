# compact
compactは自身から nilを取り除いた配列を生成して返す。
~~~
ary = [1, nil, 2, nil, 3, nil]

p ary.compact
=> [1, 2, 3]

p ary
=> [1, nil, 2, nil, 3, nil]

ary.compact!
p ary
=> [1, 2, 3]
~~~
***
