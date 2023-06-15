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
[![Image from Gyazo](https://i.gyazo.com/9bea15335f81be1044fe4122a7dc3094.png)](https://gyazo.com/9bea15335f81be1044fe4122a7dc3094)
⚠️最上位のノードは「Doctype」ではなく「Document」ノード。   
このような階層のことを「Nodeツリー」「DOMツリー」とも呼ぶ。  
***

# ノード
DOMではドキュメントを構成するオブジェクトのことを「ノード」と呼ぶ。   
ノードはいくつか種類がある。
***

##  要素ノード（Elementノード）
HTMLの要素を表す
~~~
例：　　<html>, <head>, <body>, <p>, <h1> など
~~~
***
  
## テキストノード（Textノード）
テキストや空白、改行を表す。
[![Image from Gyazo](https://i.gyazo.com/a290dea0c38bdc3d1b1a307707f104c2.png)](https://gyazo.com/a290dea0c38bdc3d1b1a307707f104c2)
⚠️文章内の空白や改行はノードになるが「html」要素の先頭と末尾の空白と改行はノードにならない！
***

## 属性ノード
class, id, nameなどの属性を表す。
***

## コメントノード
コメントを表す。
***


