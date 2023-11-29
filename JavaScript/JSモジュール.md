# モジュール(module)
ファイルごとに機能を管理させること。

例えば、「食事ファイル」に和食・洋食・中華全て書くのではなく、  
それぞれ「和食モジュール」・「洋食モジュール」・「中華モジュール」として、  
「食事ファイル」に読み込ませた方が管理しやすいし、他のファイル(例えば フードコートファイル)などでも使い回せる。
***

# モジュールの書き方
`export`を使う

## 🧡 関数に書く
~~~
export const hello = () => {
  console.log("hello!");
};
~~~
***

## クラスに書く
~~~
class User {
  constructor(name) {
    this.name = name;
  }
}

export { User }
~~~
***

## ⚠️ export default
***

# モジュールの呼び出し方
`import`を使う。

## 準備
HTMLの scriptタグに `type="module"`を入れる
~~~
[HTML]
<script type="module" src="main.js"></script>
~~~
⚠️ 直接 HTMLで JS module使う場合なので Reactなどでは書かなくていい。
***

## 🧡 関数を呼ぶ
`import {関数名} from "moduleファイルへのPATH"`

先ほどの exportで書いた関数を呼ぶ。
~~~
import { hello } from 'module.js
~~~
***

### ❓ 呼び出すときにファイル名に拡張子は必要？
モジュールバンドラーなどの webパック使ってる時はいらない。  
HTMLなどで直接呼ぶときはいる。
***
