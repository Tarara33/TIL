# webpack
モジュールバンドラーの一つで複数の JSファイルを束ねる。

基本的に JSファイルは細かく分けて開発する。  
なぜなら、再利用性が上がるから。  
しかし、本番環境では、一気にまとめてもいいよね？  
と、そのまとめ役を担ってくれるのがコレ！
***

# vite
同じようなモジュールバンドラー。  
もう少ししたら主流になるかも！？
***

# vite使い方
## ① 作成コマンド実行、プロジェクト名を入力
~~~
$ npm create vite@latest
~~~
このコマンド実行後に、プロジェクト名を入力する。
***

## ② フレームワークの選択
以下の中から選んで Enterキー押す。

[![Image from Gyazo](https://i.gyazo.com/1154852bc0921444301b9382b8147303.png)](https://gyazo.com/1154852bc0921444301b9382b8147303)
***

## ③ バリアントの選択
Viteにおける「バリアント」は、主に使用する言語やコンパイラの組み合わせを指す。

[![Image from Gyazo](https://i.gyazo.com/9aaf3e77b0f1f172c34b97193bbcd6bd.png)](https://gyazo.com/9aaf3e77b0f1f172c34b97193bbcd6bd)

SWCというのはより高速にビルドを行うためのツール。  
ただ、ファイル容量が重いので、特段必要ない場合には、通常の間 JavaScriptか TypeScriptを選べば OK。
***

## ④ 最終準備
アプリができたら以下のような画面が出るので、順番に実行していく。

[![Image from Gyazo](https://i.gyazo.com/17fd3127db8a8a7771fa7caa6b7d8230.png)](https://gyazo.com/17fd3127db8a8a7771fa7caa6b7d8230)
***

# viteコマンド
package.json内に viteで使えるコマンドが置いてある。

[![Image from Gyazo](https://i.gyazo.com/d6d68c476f3c3375b333e0a36b496223.png)](https://gyazo.com/d6d68c476f3c3375b333e0a36b496223)

## dev
サーバーを起動するコマンド
~~~
$ npm or yarn run dev
~~~
[![Image from Gyazo](https://i.gyazo.com/15a3a3abd697266ae25849842194bd7f.png)](https://gyazo.com/15a3a3abd697266ae25849842194bd7f)
***

## build
本番環境ように buildするコマンド
~~~
$ npm or yarn run build
~~~
***

## lint
ESLintを使用してコードの静的解析（linting）を行うコマンド
~~~
$ npm or yarn run lint
~~~
***

## preview
buildされたプロダクションコードをローカルでプレビューするためのサーバーを起動するコマンド。
~~~
$ npm or yarn run preview
~~~
***
