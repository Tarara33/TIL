[参考](https://qiita.com/megane42/items/bf85746b9cefaf38f473)

# rails new
何もつけずに rails new行うと、importmapを使う。
***

# ❓ importmap
JavaScriptのライブラリを管理するようなやつ。  
Rails6系は [Node.js](https://github.com/Tarara33/TIL/blob/main/%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89%E7%B3%BB/Node.js/Node_js.md)のモジュールの一つである Webpackerに依存していた。 
~~~
❓ Webpacker
CSS、JavaScript、画像などを１つのファイルとしてまとめるためのモジュールバンドラーで、node.jsのモジュールの1つ。
はサーバーサイドで動く JavaScript環境。
~~~

importmapになってから、Node.jsに依存せずに JavaScriptのモジュールをブラウザが直接インポートできるようになる。   
簡単な JSの動きならこれでOK  
ただ、現在は対応ブラウザが Chromeと Edgeなどなので、対応していないブラウザでアプリ開くと動かない場合がある。
***

#### ブラウザが直接インポートがわかりにくかったので聞いた。
~~~
今までは全ての本（JSファイル）を1つの巨大な本（バンドルファイル）にまとめて読むようにしていたんだ。
でも、importmapを使えば、必要な本だけを直接読みに行くことができるようになるんだ。
だから、全ての本をまとめる手間が不要になるんだよ。
~~~
***

## ❓ 具体的に何が変わる??
今まで、「app/assets/application.js」に JavaScriptファイルをまとめていた。  
👎 ファイルを読み込む順番も気にしながら書いていた。
~~~
[app/assets/application.js]

//= require jquery3
//= require popper
//= require bootstrap-sprockets
~~~

これが 「app/javascript/packs/application.js」に書くようになる。  
👍 モジュールなので、順番気にしなくていい。  
⚠️ それぞれのパッケージがESモジュールとして提供されている必要があるのは注意。
~~~
[app/javascript/packs/application.js]

import $ from 'jquery';
import 'bootstrap';
~~~
***

## ❓ 開発者にとってのメリット
現時点の私では、どうせ「application.js」ファイルには書くし、いまいちすごく便利な感じはない。  
強いて言えば
- JSファイルの書く順番気にしなくていい
- Node.js installしなくていい

ぐらい。  
どちらかというと PCが JSファイルまとめたりしない分楽になるみたい。
***
