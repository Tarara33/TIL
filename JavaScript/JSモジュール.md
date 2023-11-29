# モジュール(module)
ファイルごとに機能を管理させること。

例えば、「食事ファイル」に和食・洋食・中華全て書くのではなく、  
それぞれ「和食モジュール」・「洋食モジュール」・「中華モジュール」として、  
「食事ファイル」に読み込ませた方が管理しやすいし、他のファイル(例えば フードコートファイル)などでも使い回せる。
***

# モジュールの書き方
`export`を使う

## 🧡 exportのみ使う
関数などの前に exportをつける。
~~~
export const hello = () => {
  console.log("hello!");
};
~~~
***

## 🩵 名前つき export
`export { クラス名や関数名 }`
~~~
class User {
  constructor(name) {
    this.name = name;
  }
}

export { User }
~~~
***

## 🩷 デフォルト export
`export default 関数名やクラス名`  
⚠️ デフォルト exportは１ファイルで１つのみ
~~~
const funcB = () => {
  console.log("funcB output");
};

export default funcB;
~~~
***

# モジュールの呼び出し方
`import`を使う。  
呼び出すファイルの一番上に書く。  

## 準備
HTMLの scriptタグに `type="module"`を入れる
~~~
[HTML]
<script type="module" src="main.js"></script>
~~~
⚠️ 直接 HTMLで JS module使う場合なので Reactなどでは書かなくていい。
***

## 🧡🩵 export、　名前つき exportを読み込む
`import { 関数名・クラス名 } from "moduleファイルへのPATH";`

先ほどの exportで書いた関数を呼ぶ。
~~~
import { hello } from 'module.js';
=> 関数 helloが読み込まれる。

import { User } from 'module.js';
=> クラス Userが読み込まれる。

💡 同じファイルなら、
import { hello, User } from 'module.js';
この書き方でも OK
~~~
***

## 🩷 デフォルト exportを読み込む
`import 関数名・クラス名 from "moduleファイルへのPATH";`  
⚠️ {}で囲まない！
~~~
import funcB from "./module.js";
~~~

### 💡 デフォルトは一つなので import時に名前変えても OK
名前が変わっても探せるので、変えても OK。
~~~
import functionB from "./module.js";
~~~

### export、　名前つき exportとまとめて書く場合(同ファイルにある)
~~~
import funcB, { hello, User } from 'module.js';
~~~
***

### ❓ 呼び出すときにファイル名に拡張子は必要？
モジュールバンドラーなどの webパック使ってる時はいらない。  
HTMLなどで直接呼ぶときはいる。
***
