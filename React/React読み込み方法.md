# CDNから直接
自分で 1から index.htmlファイルなど作って Reactするときなど。  

HEADタグにこれを書く。  
@7や、@17は versionを示すので、使いたいやつを入れる。
~~~
<script src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone@7/babel.min.js"></script>
~~~
⚠️ これは開発用のバージョンなので、本番環境では minifiedバージョンを使う。
***

# npm/yarnでインストール
~~~
$ npx create-react-app アプリ名

$ cd my-app

$ npm start
=> サーバー起動
~~~
このコマンドで、Create React Appがシステムにインストールされ、任意のディレクトリで使用できるようになる。

indexファイルなども作られる。
***

## localでの見方
サーバーを立ち上げると、このように localの URLがあるのでクリックすると

[![Image from Gyazo](https://i.gyazo.com/5f6f38c5903d571f4f185714886b52c9.png)](https://gyazo.com/5f6f38c5903d571f4f185714886b52c9)

Reactの画面でる。

[![Image from Gyazo](https://i.gyazo.com/2eaaeea621dc85bc99c3dd3af47e7f5e.png)](https://gyazo.com/2eaaeea621dc85bc99c3dd3af47e7f5e)
***

# [vite](https://github.com/Tarara33/TIL/blob/main/%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89%E7%B3%BB/Node.js/%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB/webpack%E3%83%BBvite.md)を使う場合
フレームワークに React選ぶ。
***
