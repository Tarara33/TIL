# Reactとは？
ruby on rails のようなフレームワークではなく、    
Webサイト上のUIパーツを構築するためのJavaScriptライブラリ。    
コンポーネントベースの構造を提供する。    

⭐️UI    
ユーザーインターフェースの略。    
ユーザーとコンピュータの間のコミュニケーションを可能にする部分のこと。

⭐️コンポーネント    
部品やパーツといった意味を持つ。    
[![Image from Gyazo](https://i.gyazo.com/56bebd4f9919a94491a28b674b59c849.png)](https://gyazo.com/56bebd4f9919a94491a28b674b59c849)
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

### ⭐️extends/⭐️export/⭐️import
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

# ブラウザに表示される仕組み
App.js => index.js => index.html の順番にファイルが呼ばれていく。
~~~
[App.js]
import React from 'react;
class App extends React.Component {
  ...省略

export default App;
~~~
💡index.jsで App.jsを読み込むために exportしてる！
***
    
~~~
[index.js]
import App from './component/App';
ReactDOM.render(<App />, document.getElementById('root'))
~~~
***

### ⭐️ReactDOM.renderメソッド
<〇〇 />で大元のルートコンポネート指定して、    
document.getElementById('〇〇')で index.html内にある同じid名の部分に    
ルートコンポネートの情報を挿入する。
***

~~~
[index.html]
<body>
  <div id = 'root'></div>
</bodu>
~~~
***



