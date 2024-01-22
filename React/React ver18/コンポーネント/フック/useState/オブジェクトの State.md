# Stateにオブジェクトを渡すとき
このように、Stateにオブジェクトを渡す場合。
~~~

const personObj = {name: 'Tarara', age: 29}
const [person, setPerson] = useState⭐️(personObj)
~~~
`useState({name: 'Tarara', age: 29})`が入ってる。
***

# Stateの更新
初期画面はこんな感じ。

[![Image from Gyazo](https://i.gyazo.com/e9bd6fd6f9e55c3c41a36db512544a5e.png)](https://gyazo.com/e9bd6fd6f9e55c3c41a36db512544a5e)
~~~
import { useState } from "react";

const Example = () => {
  const personObj = {name: 'Tarara', age: 29}
  const [person, setPerson] = useState(personObj)

  const changeName = (e) => {
    setPerson( 🩵{name: e.target.value, age: person.age});
  };

  const changeAge = (e) => {
    setPerson( {name: person.name, age: e.target.value});
  };

  const reset = () => {
    setPerson(💚{ name: "", age: "" })
  }

  return (
    <>
      <h3>Name: {person.name}</h3>
      <h3>Age: {person.age}</h3>

      <input type="text" value={person.name} onChange={changeName}/>
      <input type="number" value={person.age} onChange={changeAge}/>
      <div>
        <button onClick={reset}>リセット</button>
      </div>
    </>
  )
};
~~~
🩵 名前を変える関数を作った時、nameだけではなく、ageも入れているのは、  
名前だけだと、Stateが更新された時、ageの値はないと処理されて値が消えるから。

(ageは入力触ってないのに消えてる)  
[![Image from Gyazo](https://i.gyazo.com/bded43e9d796f0f3ff0be51f1a2760ec.png)](https://gyazo.com/bded43e9d796f0f3ff0be51f1a2760ec)
***

💚 リセットするときはそれぞれ空の値入れる。
***

# ⚠️ オブジェクト Stateの注意点
- オブジェクトの更新をかけるときは、更新したいキー以外のキーも値を入力しないと、消える！
- Stateを更新するオブジェクトは新しく作る必要がある。

## Stateを更新するオブジェクトは新しく作る必要がある。
~~~
const changeName = (e) => {
    person.name = e.target.value
    setPerson(person)
  };
~~~
このコードだと名前フォーム動かない。 + 画面更新されない。

💡 なぜなら、 personのオブジェクト自体は新しいものではないので、これを setPersonに入れても画面更新されない
***

# 😮‍💨 キー多いとき書くの大変
Stateにオブジェクト渡さないといけないのはわかったが、キーが多いオブジェクトだったら全部書くの大変！
=> **スプレット構文使えばいい!!**

~~~
const changeName = (e) => {
    setPerson( {name: e.target.value, age: person.age});
  };

↓

const changeName = (e) => {
    setPerson( {...person, name: e.target.value});
  };

これでも同じ意味になる！
~~~
💡 スプレット構文で 元のオブジェクト personを展開して、カンマの後に変更する部分を入れればいい
***

