# Reactとは？
ruby on rails のようなフレームワークではなく、    
Webサイト上のUIパーツを構築するためのJavaScriptライブラリ。    

⭐️UI    
ユーザーインターフェースの略。    
ユーザーとコンピュータの間のコミュニケーションを可能にする部分のこと。
***

# 使用言語
JavaScript    
JSX
***

# 基本構文
~~~
①import React from 'react;

②class App extends React.Component {
  ③render() {
    return(
      JSXの記述
    );
  }
}
④export default App;
~~~
①Reactをインポート    
②React.Componentを継承するクラス定義    
③JSXを戻り値とするrenderメソッドの設置    
④クラスをexport
***

### extends/export/import
- extends
あるクラスを継承すること

- export
とあるファイルで定義されてる関数や、クラスを他のファイルでも読み込めるようにすること。

- import
他のファイルで定義されてる関数や、クラスを作業ファイルでも読み込めるようにすること。
***

# 言語のエリア
[![Image from Gyazo](https://i.gyazo.com/b3b3a0439da43eb468fbc05da32ae84d.png)](https://gyazo.com/b3b3a0439da43eb468fbc05da32ae84d)
***

# コメントアウト
JSの範囲なら`//`    
JSXの範囲なら`{/**/}`
***
