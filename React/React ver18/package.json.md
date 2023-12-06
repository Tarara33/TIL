# package.jsonにある React関連
package.jsonには npm/yarnでダウンロードしたものが書かれていたりするが、([詳しくはこちら](https://github.com/Tarara33/TIL/blob/main/%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89%E7%B3%BB/Node.js/%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89/npm.md))  
reactで使えるコマンドも載っている。
***

# コマンド
scriptの中にあるものがコマンド

[![Image from Gyazo](https://i.gyazo.com/2ad28ab0cdf91c5e9b60c6973833c4ad.png)](https://gyazo.com/2ad28ab0cdf91c5e9b60c6973833c4ad)

- start
サーバーを立ち上げるコマンド。
~~~
$ npm or yarn run start
~~~
***

- build
本番環境ように buildするコマンド。
~~~
$ npm or yarn run build
~~~
***

- test
test script用のコマンド。
~~~
$ npm or yarn run test
~~~
***

- eject
create-react-appコマンドで作った reactアプリの隠しファイルを表示する。
ejectは実行すると元には戻せない。
~~~
$ npm or yarn run eject
~~~

実行してみると configファイルが出たり、package.jsonの reactコマンドの中身が変わったりしている。    
実際はこんな感じのコマンド流してたよ的なのがわかる。  
[![Image from Gyazo](https://i.gyazo.com/9ffe1fe2441f832699f8746ffe841d48.png)](https://gyazo.com/9ffe1fe2441f832699f8746ffe841d48)
***
