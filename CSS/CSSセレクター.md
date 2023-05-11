# *
全称セレクター
~~~
[html]
<body>
  <h1></h1>
  <p></p>
</body>

[css]
* {
margin: 0;
}
~~~
body,h1,pの全部にmargin:0;が適応される
***
 
# .クラス名
クラスセレクター
***

# #id名
idセレクター
***

# [属性名]
属性セレクター
~~~
[html]
<ul>
  <li>item1</li>
  <li class= "item">item2</li>
  <li>item3</li>
</ul>

[css]
[class] {
color: pink;
}
~~~
item2に反映される
***

# 子結合セレクター　と　子孫結合セレクター
- 子結合...親要素 > 子要素名 {...
- 子孫結合...親要素  子要素名 {...     
空白を入れる
~~~
[html]
<section>
  <h1>sectionの子要素</h1>
  <p>sectionの子要素</p>
  <div>
    <p>sectionの孫要素</p>
  </div>
</section>

[css]
section > p　{
color: pink;
}
section p {
color: blue;
}
~~~
sectionの子要素pがピンク、sectionの孫要素pがブルーになる
***

# 隣接兄妹セレクター
~~~
[html]
<ul>
 <li>item1</li>
 <li>item2</li>
 <li>item3</li>
</ul>

[css]
li + li {
border-top: 1px solid black;
}
~~~
2番目で選択した要素のうち、直前の兄弟要素が１番目である要素を選択する   
item1には直前にliがないのでボーダーはつかない    
item2,item3には直前にliがあるので適応されボーダーがつく
***

# 擬似クラスセレクター
:nth-child(){... で表す
~~~
[html]
<ul>
 <li>item1</li>
 <li>item2</li>
 <li>item3</li>
 <li>item4</li>
 <li>item5</li>
</ul>
<p>ulの兄弟要素2</p>
<p>ulの兄弟要素3</p>

[css]
:nth-child(3) {
color: red;
}
~~~
こうするとli兄弟の中の３番目の兄弟であるli(item3)と、    
ulと兄弟の３番目の兄弟であるp(ulの兄弟要素3)が赤くなる
~~~
li:nth-child(3) {
color: red;
}
~~~
こうするとliクラスかつ三番目の兄弟要素となりli(item3)が赤くなる
***

# :nth-childの受け渡す要素の種類
- odd...奇数に反映
- even...偶数に反映
- n...0から１づつ増えていく（3nとすると、3×1で3番目、3×2で6番目、3×3で9番目...という）
- first-child...最初の兄弟に反映
- last-child...最後の兄弟に反映
***

# :hover
カーソルが上に来た時にCSSに変化がある
***

# :focus
フォーム欄など入力する箇所を押した時変化がある
***

# :active
ボタンなどクリックした時に変化がある
***

# :not()
()内に入れたもの以外に反映させる 
~~~
[html]
<html>
  <body>
    <div class = "box">BOX1</div>
    <div>BOX2</div>
    <div class = "box">BOX3</div>
  </body>
</html>

[css]
:not(.box) {
border: 1px solid red;
}
~~~
こうするとhtmlタグやbodyタグにも反映させてしまうので    
`div:not(.box)`にするとBOX2のみに反映される
***

# 擬似要素
::つける   
`<p>b こんにちは　a</p>`    
- ::before...bのところにつける
- ::after...aのところにつける
***
