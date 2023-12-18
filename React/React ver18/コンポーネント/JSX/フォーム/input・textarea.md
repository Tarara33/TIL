# label / input
ラベルを設定しておくと、ラベル部分をクリックすると、フォームにフォーカス当たる。  
label内の`htmlFor`の値と input内の`id`の値を一致させる。
~~~

import { useState } from "react";

const Example = () => {
  const [val, setVal] = useState("");

  return (
    <>
      <label htmlFor=⭐️"abc">ラベル</label>
      <input type="text"
             id=⭐️"abc"
             placeholder="初期値"
             🔴value={val}
             onChange={(e) => setVal(e.target.value)} />
~~~
***

# textarea
~~~
import { useState } from "react";

const Example = () => {
  const [val, setVal] = useState("");

  return (
    <>
      <label htmlFor="def">ラベル</label>
      <textarea id="def"
          placeholder="初期値"
          🔴value={val}
          onChange={(e) => setVal(e.target.value)} />
~~~

# 💡 閉じタグがいらない
HTMLだと`<input>入力された中身</input>`のようになるが、JSXだと閉じタグがいらない。  
🔴なので、inputや textareaの入力要素は `value`に書く。
***
