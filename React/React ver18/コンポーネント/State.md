# State
各コンポーネントごとに持つ、コンポーネントの状態を管理する仕組み。
***

# 使い方
## ① useStateの import
stateを使うコンポーネントで行う。   
コンポーネントごとに importする必要があるので、複数のコンポーネントで state使うなら、各々 importする。
~~~
[App.jsx]

⭐️ import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  return (
    <>
    </>
  );
};
~~~
***

## ② useStateの設定
`useState`は関数。  
そして戻り値は 「state」と「stateを更新するための関数」となっているので、それぞれを分割代入で受け取る。

`const [state名, setstate名] = useState(stateの初期値);`と設定する。  
~~~
[App.jsx]

import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  ⭐️const [num, setNum] = useState(0);
  return (
    <>
    </>
  );
};
~~~
#### ⚠️ useStateは関数コンポーネントの一番上でしか呼べない。([こちら](https://github.com/Tarara33/TIL/blob/main/React/React%E3%83%A1%E3%83%A2/use%E7%B3%BB%E3%83%95%E3%83%83%E3%82%AF.md))
***

## ③ setState関数を使ってみよう！
例として、カウントアップボタンを作る
~~~
[App.jsx]

import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  const [num, setNum] = useState(0);
  const onClickCountUp = () => {
    🩵setNum((num) => num + 1);
  };
  return (
    <>
      <button onClick={onClickCountUp}>カウントアップ</button>
      <p>{num}</p>
    </>
  );
};
~~~
🩵 onClickCountUp関数のなかで、setNum関数の発動。  
カウントアップボタン押したら、 setNum関数が発動して num + 1される。
***

# ⚠️ setState関数呼んだ瞬間に stateが更新されるわけではない
例えば、以下のコードだとどんな動きをするか？？
~~~
[App.jsx]

import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  const [num, setNum] = useState(0);
  const onClickCountUp = () => {
    setNum((num) => num + 1);
    setNum((num) => num + 1);
  };
  return (
    <>
      <button onClick={onClickCountUp}>カウントアップ</button>
      <p>{num}</p>
    </>
  );
};
~~~
setNumを2回呼んでいるが、カウントアップボタンを押すと 2ずつ増えるか？？  
=> 増えない、1ずつ増える。
***

## 💡 Reactはバッチ処理している
バッチ処理とは、一定量の(あるいは一定期間の)データを集め、一括処理するための処理方法。  
そのため、２回分の setNumもページが読み込まれないでそのまま処理されるので numの更新がなく、同じ0 + 1をするだけ。

[![Image from Gyazo](https://i.gyazo.com/f37ec2d1e5dbbac692cbd23e0539f19e.png)](https://gyazo.com/f37ec2d1e5dbbac692cbd23e0539f19e)
***

### ❓ なぜバッチ処理している？？
一括で処理したほうが挙動が早いから。  
処理ごとにページ更新すると重くなる。
***

## ❓ 理想通りの挙動を実行できないのか？？
押したら 2ずつ増やすことはできないのか？？  
=> できる！

setStateに関数を渡すことができるので関数内で処理する。  
💡 引数には必ず numが入る。
~~~
[App.jsx]

import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  const [num, setNum] = useState(0);
  const onClickCountUp = () => {
    setNum((num) => num + 1);
    setNum((num) => num + 1);
  };
  return (
    <>
      <button onClick={onClickCountUp}>カウントアップ</button>
      <p>{num}</p>
    </>
  );
};
~~~
***
