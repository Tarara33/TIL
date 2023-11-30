# on/offボタンの実装
onの時に顔文字出す / offのときは出さない という実装。
~~~
[App.jsx]

import { useState } from "react";
import { ColorfulMessage } from "./conpornents/ColorfulMessage";

export const App = () => {
  const [isShowFace, setisShowFace] = useState(false);

  const onClickToggle = () => {
    🩵setisShowFace(!isShowFace);
  };

  return (
    <>
      <button onClick={onClickToggle}>on/off</button>
      ⭐️{isShowFace && <p>Σ('◉⌓◉’)</p>}
    </>
  );
};
~~~
🩵 setisShowFace関数では、true/falseの変更を行なっている。  

⭐️ [|| と &&](https://github.com/Tarara33/TIL/blob/main/JavaScript/%E3%83%A1%E3%83%A2/%7C%7C%20%E3%81%A8%20%26%26.md)  
&&なので、isShowFaceが trueなら右を代入、 falseなら処理を終了して代入。  
trueの場合は、pタグが buttonの下に入るので、顔文字が現れる。
***
