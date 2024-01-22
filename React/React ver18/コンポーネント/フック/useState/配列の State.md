# Stateに配列を渡すとき
~~~
const arry = [1,2,3,4,5];
const [nums, setNums] = useState(arry);
~~~
このように渡すことができ、JSX内では`{nums}`とすると、  
[![Image from Gyazo](https://i.gyazo.com/fa1397e8729010e964a1f1de226ee153.png)](https://gyazo.com/fa1397e8729010e964a1f1de226ee153)  
**配列は展開されて表示される**
***

# 更新するときは新しい配列を作る。
[オブジェクト](https://github.com/Tarara33/TIL/blob/main/React/React%20ver18/%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88/State/%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%AE%20State.md)同様に、  
Stateの配列を更新するときは、新しい配列を作る。
~~~
import { useState } from "react";


const Example = () => {
  const arry = [1,2,3,4,5];
  const [nums, setNums] = useState(arry);

  const shuffle = () => {
    const newNums = [ ...nums ];  => 💡スプレッド構文で新しい配列を作った。
    const value = newNums.pop();
    newNums.unshift(lastVal);
    setNums(newNums);
  }
  return (
    <>
      <h1>{nums}</h1>
      <button onClick={shuffle}>shuffle</button>
    </>
  );
};
~~~
***
