# 使える範囲
[![Image from Gyazo](https://i.gyazo.com/b3b3a0439da43eb468fbc05da32ae84d.png)](https://gyazo.com/b3b3a0439da43eb468fbc05da32ae84d)

return内でのみ使えて、記述したものはブラウザに表示される。
***

# JSを埋め込む
~~~
class App extends React.Component {
  render() {
    const text = 'Hello World';
    return(
      <h1>{text}</h1>
    );
  }
}
~~~
`{}`を使って埋め込むことができる。
***

# img
HTMLのimgタグは閉じタグいらないが、JSXのimgタグは必要。
~~~
<img src = "#" />
~~~
***
