# simple_format
simple_format(文字列)で使用   
動きとしては
- 文字列を`<p>`タグで括る
- 改行は`<br />`を付与
- 連続した改行は`</p><p>`を付与
~~~
[ruby]
test = "こんにちは。
  　　　　今日は晴れです
  
  　　　　明日は雨です"
  
[rails]
simple_format(test)
→実行  
  <p>こんにちは。<br>
  　　　　今日は晴れです</p>
  <p>明日は雨です</p>
  
と変換される
~~~

## オプション
- sanitize:...true or falseで選択    
trueにするとテキストに含まれる一部のセキュリティ面で危険なHTMLタグを取り除いてくれる
- wrapper_tag:...通常pタグだが、例えばdivタグで囲いたいときなどに使う
- hメソッド...タグもそのまま出してくれる。 改行はタグでは出ないで改行される。
~~~
[ruby]
test = "こんにちは。
  　　　　今日は晴れです<h2>
  
  　　　　明日は雨です"
  
[rails オプション無し]
simple_format(test)
→実行  
  <p>こんにちは。<br>
  　　　　今日は晴れです</p>
  <p>明日は雨です</p>

→出力
  こんにちは。
  今日は晴れです。
  明日は雨です。
  
[rails オプション有り]
simple_format(h(test), sanitize:false, wrapper_tag: "div")
→実行  
  <div>こんにちは。<br>
  　　　　今日は晴れです<h2></div>
  <div>明日は雨です</div>

→出力
  こんにちは。
  今日は晴れです<h2>
  明日は雨です
~~~
***
