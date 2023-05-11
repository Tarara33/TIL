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



