# LESS変数
Bootstrapが提供している変数。[LESS変数](https://getbootstrap.com/docs/3.4/customize/#less-variables)        
gem 'bootstrap-sass'を入れると使えるようになる。
***


# Saccの変数と LESSの変数
通常、Sassではこのように「$」をつけて変数定義する。
~~~
$gray-light: #777;
~~~
    
しかし、Bootstrapが用意している　を使うと
~~~
@gray-light: #777;
~~~
このように「@」で定義して使える。
