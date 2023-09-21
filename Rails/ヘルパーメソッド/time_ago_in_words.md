[参考](https://railsdoc.com/page/time_ago_in_words)  
  
# time_ago_in_words
日時を表示  
Twitterなどで見かける「3分前」みたいな表示をする場合に使う。  
  
⭐️ 引数には過去の時間を入れ、その時間と現在時刻の差を出す。
~~~
[例]
time_ago_in_words(3.minutes.ago)
=>
3 minutes
~~~
***

## Twitterみたいに現在時刻から何分前みたいにしたい時
[![Image from Gyazo](https://i.gyazo.com/ee5c295dae87c7d56817235418c68d3f.png)](https://gyazo.com/ee5c295dae87c7d56817235418c68d3f)  

~~~
[viewファイル]

Posted <%= time_ago_in_words(@micropost.created_at) %> ago.
~~~
記事(@micropost)の作成時間(created_at)から現在時刻の差を出す。
***

# Rails cで確認できる
helperオブジェクト使うとコンソールでも使える。
~~~
[rails c]

$ helper.time_ago_in_words(1.year.ago)
=> "about 1 year"
~~~
***
