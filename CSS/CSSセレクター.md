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

# 子結合　と　子孫結合
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
