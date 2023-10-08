# for文
指定した要素分繰り返し処理を行ったり、配列の要素を順番に取得したい場合に使用する。  
~~~
[配列の場合]

array = [1,2,3]
for x in array
  puts x
end

[実行結果]
1
2
3


[ハッシュの場合]
hash = {sam: 28, yusuke: 29, misa: 20}
for key, value in hash
  puts "#{key}は#{value}歳です"
end

[実行結果]
samは28歳です
yusukeは29歳です
misaは20歳です
~~~
***
