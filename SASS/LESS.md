# LESS
基本的には、Sassとほぼ同じ。
ただ、「Saas」に比べると少しシンプルな記述方法になる。
また、Sassファイル内でも使える(?)
***

# Saccの変数と LESSの変数
通常、Sassではこのように「$」をつけて変数定義する。
~~~
$gray-light: #777;
~~~
    
しかし、Bootstrapが用意している　[LESS変数](https://getbootstrap.com/docs/3.4/customize/#less-variables)を使うと
~~~
@gray-light: #777;
~~~
このように「@」で定義して使える。
