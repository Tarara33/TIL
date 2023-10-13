[参考](https://qiita.com/megane42/items/bf85746b9cefaf38f473)

# ⭐️ rails new
rails7系からフロントサイドが変わった部分がある。  

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

# ⭐️ Rails new + esbuild
こちらは Node.js使う。Node.js使うので　yarnなども必要。  
importmapではなく esbuildを使う。
~~~
$ rails new app_name --css=bootstrap
~~~
Rails７系はアセットパイプラインに Webpackerをデフォルトで使用するようになったので、      
このように rails new コマンドで --css オプションを指定して、  
CSSフレームワークを選択した場合、Webpackerと esbuildがデフォルトで使用される。
***

# ❓ esbuild
Webpackerなどと同じで、javascriptのバンドラー。  
Webpackerよりまとめる速度がめちゃはやい！らしい！
***

## 💡 ちなみに...
javascriptのバンドラーは rails newの時に指定することができるので、  
現在すでに動いているアプリの環境を移行したいとかでなければ new時に指定するのがおすすめ。  

後々追加もできるが、めんどくさい。  
例えば、最初に rails newで importmapを JSバンドラーに設定した後に、  
やっぱり esbuildを JSバンドラーにするとなると、以下のようなことをしなければいけない。
~~~
* esbuildのインストール
* 依存関係の調整
* JavaScriptファイルの整理
* importmapの削除
* ビルドスクリプトの実行
* ビューの更新
~~~
***

# importmap か esbuildか
~~~
【importmap が適している場合】
* プロジェクトが小規模で、複雑な依存関係を持っていない場合。
* クライアントサイドの JavaScript モジュールの管理が主要な課題であり、ブラウザでのモジュールのロードが問題となる場合。
* シンプルなフロントエンドのアプリケーションを開発しており、高度なビルドプロセスが不要な場合。

【esbuild が適している場合】
* プロジェクトが大規模で、多くの依存関係を持ち、👍高度なビルドプロセスが必要な場合。
* フロントエンドとバックエンドの両方で JavaScript コードを管理し、効率的なバンドリングとトランスパイルを実現したい場合。
* 高速なバンドル生成が重要で、パフォーマンスが要求される場合。
~~~
**⚠️ もし、Hotwireなどを使用する予定があれば esbuildがいい！**  
リアルタイム通信には 高速ビルドが必要。  
importmapはビルドやトランスパイルのプロセスが含まれていない。
***
