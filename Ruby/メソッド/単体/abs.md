# abs
絶対値を返すメソッド。  
数値オブジェクトに対して使えて、その数値の絶対値を得られる。

### ❓ 絶対値??
絶対値とは、数直線上で原点からの距離を表す値で、必ず正の値になる。  
数学では基準となる点が「0」であることから、「基準となる0からどれだけ離れているか」を表す値のことを言う。
~~~
a = 10
b = 5

puts (b - a)
=> -5

しかし、 absメソッドを使うと絶対値になるので

puts (b - a).abs
=> 5
~~~
***
