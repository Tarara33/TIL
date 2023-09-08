# find_by と find と where
## find
各モデルのidを検索キーとしてデータを取得するメソッド    
⚠️id以外の条件で検索不可、取得したいデータのidの値が具体的に分かっている場合に使用する。
~~~
[rails c]
Post.find(1)
=> id 1の返ってくる
~~~
***

## find_by
各モデルをid以外の条件で検索するメソッド(idでも検索可能)   
複数の検索条件を指定可能 (⚠️返ってくる結果は最初にヒットした1件のみ)   
id及びid以外の条件が分かっている場合、その条件に該当する最初のデータを取得したい場合に使用する。
~~~
[rails c]
Post.find_by(id: 1)
=> id 1の返ってくる
~~~
***

## where
各モデルをid以外の条件で検索する場合、該当するデータ全てが返ってくる。
~~~
[rails c]
Post.where(id: 1)
=> id 1の返ってくる
~~~
***

## 返ってくるクラス
~~~
[rails c]
Post.find(1)。class
=> Postクラスのオブジェクト

Post.find_by(id: 1).class
=> Postクラスのオブジェクト

Post.where(id: 1).class
=> ActiveRecord Relationのオブジェクト

Post.where(id: 1).last.class
=> Postクラスのオブジェクト
~~~
⚠️whereで探してそのクラスのオブジェクトとして返して欲しいなら「.last, .first」つける
***
