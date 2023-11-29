# CDNから直接
HEADタグにこれを書く。  
@７や、@17はversionを示すので、使いたいやつを入れる。
~~~
<script src="https://unpkg.com/react@17/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone@7/babel.min.js"></script>
~~~
⚠️ これは開発用のバージョンなので、本番環境では minifiedバージョンを使う。
***

