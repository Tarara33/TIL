# pluck
引数に指定したカラムの値を配列で返してくれるメソッド    
全てのデータではなく特定のカラムの値だけ取得したい場合に使うと便利で　`モデル名.pluck(:カラム名)` と使う    
| id | name | age|
----|----|----
| 1 | 一郎 | 30 |
| 2 | 二郎 | 20 |
| 3 | 三郎 | 10 |
という「Taro」と言うテーブルがあった場合、
~~~
Taro.pluck(:name)
SELECT "Taro"."name" FROM "Taro"

=> ["一郎", "二郎", "三郎"] 
~~~

## 引数を複数指定
~~~
Taro.pluck(:name, :age)
SELECT "Taro"."name" FROM "Taro"

=> [["一郎",30], ["二郎",20], ["三郎",10]] 
~~~
***

## 引数なしの場合
~~~
Taro.pluck
SELECT "Taro"."name" FROM "Taro"

=> [
[1,"一郎",30], 
[2,"二郎",20], 
[3,"三郎",10]
] 
~~~
全カラムのデータを配列に入れて返す
***

# exists?　と　present?
- exists?...データベースからデータを取得する際(前)にデータが存在するかどうかを知りたい時 
- present?...一旦全てのデータを取ってきて、その後present?メソッドによってデータが存在するか確認してる
[詳しくはこちら]（https://qiita.com/takuyanin/items/aa8c1d82ab14f1827a6a）


