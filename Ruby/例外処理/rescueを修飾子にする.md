# rescueを修飾子にする
**例外が発生しそうな処理 rescue 例外が発生した場合の戻り値**  
という書き方をすると簡潔にかける。
~~~
【例: 例外が発生しない】
1 / 1 rescue 0
=> 1

【例: 例外が発生する】
1 / 0 rescue 0
=> 0
~~~
⚠️ エラークラスの指定はできないので StandardErrorとその配下が拾われる。
***
