# useEffect
use系フックの一つで、コンポーネントの[レンダリング](https://github.com/Tarara33/TIL/blob/main/React/React%20ver18/%E3%83%A1%E3%83%A2/%E3%83%AC%E3%83%B3%E3%83%80%E3%83%AA%E3%83%B3%E3%82%B0.md#%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E3%81%AE%E3%83%AC%E3%83%B3%E3%83%80%E3%83%AA%E3%83%B3%E3%82%B0)がされるとき、  
第二引数で設定した要素が変更の場合は、 userEffect内に書いている処理を実行する。  
変更されない場合は、useEffectは処理は実行しない。  
***

# 例題
~~~
[APP.jsx]

import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  const [num, setNum] = useState(0);
  const [isShowFace, setisShowFace] = useState(false);

  const onClickCountUp = () => {
    setNum((num) => num + 1);
  };

  const onClickToggle = () => {
    setisShowFace(!isShowFace);
  };
  
  🌞if (num > 0) {
      if (num % 3 === 0) {
        isShowFace || setisShowFace(true);
      } else {
        isShowFace && setisShowFace(false);
      }
    }

  return (
    <>
      <button onClick={onClickCountUp}>カウントアップ</button>
      <p>{num}</p>
      <button onClick={onClickToggle}>on/off</button>
      {isShowFace && <p>Σ('◉⌓◉’)</p>}
    </>
  );
};
~~~
このコードには問題がある。  
カウントアップするボタンと、顔文字を表示・非表示するボタンの二つのボタンがあるが、  
🌞部分の if文で「カウントが3の倍数なら、顔文字を表示する」と  
二つのボタンを連動させてしまったせいで、on/offボタンがうまく動かなくなった。  

num = 0の場合は on/off動く。  

しかし、 num = 1の時は on/offボタン押すと、onClickToggle関数内で isShowFaceは trueになるが、  
その後 if文で numが 1なので else文にわたり、 trueなので右辺の `setisShowFace(false)`になり、顔文字が消える。  
つまり、コンマ何秒だけ表示され、その後すぐに falseになり消されるので実際はボタン押しても出てこない。  

num = 3の場合は逆に顔文字が消えない。  

このように不具合が出た。
***

# useEffect使う
## ① importする
~~~
import { useEffect, useState } from "react";
=> useStateと同じ reactから　importしているので一緒に importしてる。
~~~
***

## ② useEffectの設定
~~~
[App.jsx]

import { useEffect, useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  console.log("----APP----");
  const [num, setNum] = useState(0);
  const [isShowFace, setisShowFace] = useState(false);

  const onClickCountUp = () => {
    setNum((num) => num + 1);
  };

  const onClickToggle = () => {
    setisShowFace(!isShowFace);
  };

  ⭐️useEffect(() => {
    console.log("----userEffect----");
    if (num > 0) {
      if (num % 3 === 0) {
        isShowFace || setisShowFace(true);
      } else {
        isShowFace && setisShowFace(false);
      }
    }
  }, ⭐️[num]);

  return (
    <>
      <button onClick={onClickCountUp}>カウントアップ</button>
      <p>{num}</p>
      <button onClick={onClickToggle}>on/off</button>
      {isShowFace && <p>Σ('◉⌓◉’)</p>}
    </>
  );
};
~~~
第二引数は配列なので、　num以外にも isShowFaceなども入れられるが、  
今回は if文で numの数値を気にしているので numだけ入れる。

すると、numが更新された時はレンダリングされた時に useEffect内の処理、if文も実行されるが、  
on/offボタンの場合は、 isShowFaceが変わってレンダリングされた時、useEffect内の処理、if文は実行されないので、  
シンプルに on/offボタンどうりに 顔文字が動く！
***

## ❓ 本当に実行されないの？？
console.logでコンポーネント実行時に出力させてみる。

まず、リロード時は実行される。
[![Image from Gyazo](https://i.gyazo.com/5a81c7b5d7a35d1f676f4767df86ac88.png)](https://gyazo.com/5a81c7b5d7a35d1f676f4767df86ac88)
***

次に num変更時...useEffect内も実行される。

[![Image from Gyazo](https://i.gyazo.com/76bdefeb2c570984f66a7e07575b8103.png)](https://gyazo.com/76bdefeb2c570984f66a7e07575b8103)
***

次に on/offボタンで isShowFace変更時...useEffect内も実行されない。

[![Image from Gyazo](https://i.gyazo.com/c50ab5cf3418b0b2920fc9fbb61607da.png)](https://gyazo.com/c50ab5cf3418b0b2920fc9fbb61607da)
***
