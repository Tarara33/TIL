# JSXの中に書く JS
`{}`内に書く。

## 定数を使える
~~~
const Sample = () => {
  const title = "HELLO Title"

  return(
    <h3>{title}</h3>
  );
};
~~~
***

## 属性名に使える + メソッド使える
~~~
const Sample = () => {
  const title = "HELLO Title"
  const name = "Sample"

  return(
    <div className={name.toLowerCase()}>
      <h3>{title}</h3>
    </div>
  );
};
~~~
[![Image from Gyazo](https://i.gyazo.com/c9f994b3e018f47b7faf551cae0ab8b7.png)](https://gyazo.com/c9f994b3e018f47b7faf551cae0ab8b7)

`toLowerCase()`メソッドで小文字に変えられた、定数 nameがクラス名にはいってる。
***

## 関数を入れる
~~~
const Sample = () => {
  const hello = (name) =>`${name}さんこんにちは！`

  return(
    <h3>{hello('Tarara')}</h3>
  );
};
~~~
***

# 画面上に出ないもの
## コメントアウト
これは空タグとなり、画面には出ない。
~~~
const Sample = () => {
  const title = "HELLO Title"

  return(
    <h3>{/* コメントアウト */}</h3>
  );
};
~~~
***

## bool値
trueや falseのこと
~~~
const Sample = () => {
  const bool = true

  return(
    <h3>{bool}</h3>
  );
};
~~~
***
