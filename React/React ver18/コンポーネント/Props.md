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

