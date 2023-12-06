# CDNから直接
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
***
