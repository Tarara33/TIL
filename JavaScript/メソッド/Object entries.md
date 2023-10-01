# Object.entries()
オブジェクトをに使う。   
それぞれのプロパティを配列にして、さらにそれらをまとめた配列を取得する事ができる。
~~~
const scores = {
    math: 80,
    english: 30,
  };

const entries = Object.entries(scores);
console.log(entries);
//  ['math', 80], ['english', 30]
~~~
***
