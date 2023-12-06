# Props
親コンポーネントから子コンポーネントへ値を渡すための仕組み。
***

# 使い方
## 親コンポーネント
例えば、h1タグの下に任意の文字列を、指定した色にして消して欲しい。
~~~
🩵import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  const onClickButton = () => alert();

  return (
    <>
      <h1>やほほ</h1>
      🩵<ColorfulMessage 🩷color="blue" 🩷message="お元気ですか"></ColorfulMessage>
    </>
  );
};
~~~
🩵 任意の文字列を、指定した色にするコンポーネント(関数)を読み込む。  
そして、そのコンポーネントで帰ってきた要素を入れる部分を`<ColorfulMessage(コンポーネント名)>`タグをおく。  

🩷 渡す値はオブジェクト形式で渡すので、キー(名前はなんでも OK)に値を渡す。
***

## 子コンポネート
渡ってきた任意の文字列を、渡ってきた任意の色にする 子コンポーネント。
~~~
[colorfulmessage.jsx]

export const ColorfulMessage = (⭐️props) => {
  ⭐️console.log(props);

  const contentStyle = {
    color: 🧡props.color,
    fontSize: "18px",
  };

  return <p style={contentStyle}>{🧡props.message}</p>;
};
~~~
⭐️ 引数名はなんでも OK!  
オブジェクト形式で値が渡ってくる。  

[![Image from Gyazo](https://i.gyazo.com/5ecd60e34b244a1f5ebc761c02c960fd.png)](https://gyazo.com/5ecd60e34b244a1f5ebc761c02c960fd)


🧡 オブジェクト名.キー名で適応箇所に値を入れる。
***

## 図にしてみると

[![Image from Gyazo](https://i.gyazo.com/6c274bbe9cc2e4d8299b9e544b1115b5.png)](https://gyazo.com/6c274bbe9cc2e4d8299b9e544b1115b5)
***

# 子コンポーネントに渡すときの技
## タグの間の要素の取得
タグの間にある要素は `props.children`で取得できる。
~~~
[オブジェクトキーに入れる場合]

【親】 <ColorfulMessage message="元気ですか？？"></ColorfulMessage>
↓
【子】 return <p>{props.message}</p>;


[props.children]

【親】 <ColorfulMessage>お元気ですか</ColorfulMessage>
↓
【子】 return <p>{props.children}</p>;
~~~
わざわざ タグ間からキーに入れなくても OK!
***

## 分割代入しちゃう
~~~
[colorfulmessage.jsx]

export const ColorfulMessage = (props) => {
⭐️const { color, children } = props;
  const contentStyle = {
    💡color: ⭐️color,
    =>オブジェクトの省略で colorのみでも OK

    fontSize: "18px",
  };

  return <p style={contentStyle}>{⭐️children}</p>;
};
~~~
💡 ちなみに、このコード、[オブジェクトの省略](https://github.com/Tarara33/TIL/blob/main/JavaScript/JS%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88.md#%EF%B8%8F-%E7%9C%81%E7%95%A5%E8%A8%98%E6%B3%95)が使える。
***

### さらに短縮
引数で受け取った時点で分割代入する。
~~~
[colorfulmessage.jsx]

export const ColorfulMessage = ({⭐️ color, children }) => {

  const contentStyle = {
    color: ⭐️color,
    fontSize: "18px",
  };

  return <p style={contentStyle}>{⭐️children}</p>;
};
~~~
***

# デフォルト値設定できる
~~~
[colorfulmessage.jsx]

export const ColorfulMessage = ( {color, ⭐️children="Happy" }) => {

  const contentStyle = {
    color,
    fontSize: "18px",
  };

  return <p style={contentStyle}>{⭐️children}</p>;
};


--------------------------------------------------------------------
[親]

<ColorfulMessage color="green" />

=> 緑色の文字でデフォルトの「Happy」が表示される
~~~
***

# 受け取った propsのキー名を変えたい時
例えば以下のコードで親コンポーネントから受け取った props 「message」を「m」として使いたい時。  
`キー名: 使いたいキー名`とする。
~~~
[親]

<ColorfulMessage message="HAPPY" />

--------------------------------------------------------------------
[colorfulmessage.jsx]

export const ColorfulMessage = (⭐️{message: m ="Happy" }) => {

  const contentStyle = () => <p>⭐️{m}</p>;
};
~~~
***

# 送れるもの
要素や定数、関数など送れる！
***
