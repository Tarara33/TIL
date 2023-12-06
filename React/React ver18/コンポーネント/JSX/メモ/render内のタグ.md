# render内のタグ
要素を divタグで囲むのが基本だが、他の囲みかたもある。

#### 💡 おさらい
JSX記法は、複数の要素を一まとめにする必要がある。  
そのため、 divタグなどで囲まれることが主流。  
***

## ① 基本的な div囲み
~~~
[index.js]

import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

const App = () => {
  return (
　　　⭐️ <div>
      <h1>やほほ</h1>
      <p>お元気？</p>
    </div>
  );
};

root.render(
  <StrictMode>
    <App />
  </StrictMode>,
);
~~~

divで囲んでいるので、生成される HTMLも divで囲まれている。

[![Image from Gyazo](https://i.gyazo.com/f8fe4f14fafcd297b6a18b5f0c513e3c.png)](https://gyazo.com/f8fe4f14fafcd297b6a18b5f0c513e3c)
***

## ② `React.Fragment`で囲む
~~~
[index.js]

⭐️import React, { StrictMode } from "react";
import { createRoot } from "react-dom/client";
const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

const App = () => {
  return (
　　　⭐️ <React.Fragment>
      <h1>やほほ</h1>
      <p>お元気？</p>
    </React.Fragment>
  );
};

root.render(
  <StrictMode>
    <App />
  </StrictMode>,
);
~~~
⚠️ React.Fragmentを使う場合は、reactを importする時、全て importする。(デフォルトインポート)か `{Figment}`をインポートする。

React.Fragmentで囲むと生成される HTMLは divタグなどはなく要素のみになる。

[![Image from Gyazo](https://i.gyazo.com/7c3e4c6800edccfba06a2affdf7d84c3.png)](https://gyazo.com/7c3e4c6800edccfba06a2affdf7d84c3)
***

## ③ 空タグ
Flagmentの短縮バージョン。
~~~
[index.js]

import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
const rootElement = document.getElementById("root");
const root = createRoot(rootElement);

const App = () => {
  return (
 ⭐️ <>
      <h1>やほほ</h1>
      <p>お元気？</p>
    </>
  );
};
root.render(
  <StrictMode>
    <App />
  </StrictMode>,
);
~~~
⚠️ こちらの場合は Figmentのインポートいらない。  

React.Fragment同様、生成される HTMLは divタグなどはなく要素のみになる。

[![Image from Gyazo](https://i.gyazo.com/7c3e4c6800edccfba06a2affdf7d84c3.png)](https://gyazo.com/7c3e4c6800edccfba06a2affdf7d84c3)
***
