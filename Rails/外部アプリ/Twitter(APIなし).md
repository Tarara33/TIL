[参考](https://www.nishishi.com/blog/2020/07/twitter_no_api.html)

# tweetの埋め込み
【APIを使った場合】  
Twitter APIにツイートの URLを送ると、ツイートの情報や埋め込み用の HTMLなどが JSON形式で返ってくる。  
その中から「埋め込み用の HTML」を抜き出して表示に使えば、ツイートが埋め込める。  

    
【デメリット】  
リクエスト頻度には制限があると、タイミングによっては反応が返ってこない場合もありそう。（アクセス頻度が高い場合とか）
***

# API使わない方法

### 手順① 
blockquote要素を1つ以下のコードのように書く。（中身にテキストはなくて良い）
~~~
<blockquote class="twitter-tweet">～</blockquote>
~~~
***

### 手順②
先ほどの中に a要素を含めて、ツイート単独ページへのリンクを作る。
~~~
<blockquote class="twitter-tweet">
  <a href="（埋め込みたいツイートのURL）"></a>
</blockquote>
~~~
***

### ❓　URLどこから持ってくるの？？
twitterの　共有ボタン押した時に出る　Copy linkを押すと、URL取れる。
***

### 手順③
Twitter埋め込み用ウィジェットの JavaScriptを読み込む。
~~~
<blockquote class="twitter-tweet">
  <a href="（埋め込みたいツイートのURL）"></a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
~~~
💡 最後の Twitter側の JavaScript（widgets.js）は1度だけ読めば良いので、毎回出力する必要はない。
***

## 読み込まれるまで...
tweetが読み込まれるまで待ち時間中には何も表示されないので、(ツイート埋め込み処理中)と表示すると親切。  
あと、 tweetが何らかの理由で表示されなかったときに、     
tweetへのリンクだと分かりやすいように aタグにテキストで tweetと入れた。  
~~~
<blockquote class="twitter-tweet">
   (ツイート埋め込み処理中)
   <a href="https://twitter.com/nishishi/status/1277915695032893440">tweet</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
~~~
***

# 公式の埋め込みツール
[埋め込みツール](https://publish.twitter.com/#)で、URL貼ると HTML出してくれる。
~~~
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">高校の友達たちと旅行だから今日までに味覚戻ってきて欲しかったけど結局まだ戻ってきてない🦠せっかくの旅行メシ...🍚</p>&mdash; Tarara💻RUNTEQ44期 (@tarara_run44) <a href="https://twitter.com/sary_run44/status/1693797796133122115?ref_src=twsrc%5Etfw">August 22, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
~~~
ただ、このままコード貼るとこの tweetにしか使えないので汎用性はない。  
でも、参考にするといい。   
***
