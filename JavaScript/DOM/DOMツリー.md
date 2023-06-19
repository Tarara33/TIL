# DOM (Document Object Model)
HTMLなどのドキュメントに含まれる要素や要素に含まれるテキストのデータをオブジェクトとして扱う。   
プログラムからHTMLやXMLを自由に操作するための仕組みだ。
***

# DOMの階層構造
DOMでは対象となるドキュメントをノートの階層構造として扱う。   
例えばこんなコードがあった場合
~~~
<!DOCTYPE HTML>
<html>
  <head>
    <title>散歩</title>
  </head>
  <body>
    <p id="xxx">
      今日は<strong>「イラスト入門」</strong>を購入
   </p>
  </body>
</html>
~~~
DOMでは次のような階層構造のドキュメントツリーとして認識する。
[![Image from Gyazo](https://i.gyazo.com/4be5f9b6bed38b9e445f1f1a6f507521.png)](https://gyazo.com/4be5f9b6bed38b9e445f1f1a6f507521)
⚠️最上位のノードは「Doctype」ではなく「Document」ノード。   
このような階層のことを「Nodeツリー」「DOMツリー」とも呼ぶ。  
***

# ノード
DOMではドキュメントを構成するオブジェクトのことを「ノード」と呼ぶ。   
ノードはいくつか種類がある。
***

###  要素ノード（Elementノード）
HTMLの要素を表す
~~~
例：　　<html>, <head>, <body>, <p>, <h1> など
~~~
***
  
### テキストノード（Textノード）
テキストや空白、改行を表す。
[![Image from Gyazo](https://i.gyazo.com/e731f75550cea183c18e36c8e9a4e094.png)](https://gyazo.com/e731f75550cea183c18e36c8e9a4e094)
⚠️文章内の空白や改行はノードになるが「html」要素の先頭と末尾の空白と改行はノードにならない！
***

### 属性ノード
class, id, nameなどの属性を表す。
***

### コメントノード
コメントを表す。
***

# ノードの関係性
[![Image from Gyazo](https://i.gyazo.com/2486f26325242c9e4690230853761e7e.png)](https://gyazo.com/2486f26325242c9e4690230853761e7e)

-　親ノード（Parent Node）   
-　子ノード(Child Node)    
-　兄弟ノード（Sibling Node）  
-　子孫ノード（親の親との関係）   
と言う関係性がある
***
