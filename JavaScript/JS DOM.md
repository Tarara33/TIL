# document
DOM は document という特殊なオブジェクトで扱うことができる

# querySelector()
文書内から特定の要素を取得する  
querySelector("セレクター名") h1や#id名など
⚠️このセレクターで見つかった要素のうち最初のものだけしか取得できない
***

# querySelectorAll()[]
文書内から特定の要素を取得する  
querySelector("セレクター名")[何番目の要素か] h1や#id名など
~~~
〜2番目のpを変えたい〜
[html]
<p>こんにちは</p> //[0]
<p>こんにちは</p> //[1]
<p>こんにちは</p> //[2]

[js]
document.querySelectorAll("p")[1].textContent = "Changed"; 
//<p>こんにちは</p>
  <p>Changed</p>
  <p>こんにちは</p>
~~~
全ての要素を変えたい場合
forEach使う
~~~
 [js]
document.querySelectorAll("p").forEach((p,index) => {
  p.textContent = `${index}番目のpです！`;
});
~~~
***

# .textContent
中身のテキストを表現する    
`document.querySelector("h1").textContent = "変更内容"`   
でh1のテキスト内容が変わる
***

# getElementById() 
idを指定して要素を探す
getElementById(id名) 　⚠️#いらない
***

# getElementsByClassName()[]
class = "class名"のクラス名を指定して要素を探す
getElementsByClassName("class名")[そのクラス名の何番目の要素か] 　⚠️.いらない
***

# getElementsByTagName()[]
htmlタグ名で探す    
getElementsByTagName("タグ名")[そのタグ名の何番目の要素か] 
***
