# 最初に...
package.jsonのあるディレクトリに cdで移動する。
***

## `$ npm install`
package.jsonにある「dependencies」の中に書いてあるものをインストールする。

[![Image from Gyazo](https://i.gyazo.com/0306e9d3defc3ba0f20047da45e0dc67.png)](https://gyazo.com/0306e9d3defc3ba0f20047da45e0dc67)

この場合だと、typescriptをインストールする。    
ディレクトリ配下に「node_modules」ができて、中に、typescriptが入ってる。
***

## `$ npm install 〇〇`
例えば、「jquery」を追加で欲しいときは、`npm install jquery`。  
すると、「dependencies」の中と node_modulesに jqueryが追加される。

[![Image from Gyazo](https://i.gyazo.com/9225b635bc7958f934e7cdb5b1a58993.png)](https://gyazo.com/9225b635bc7958f934e7cdb5b1a58993)
***

## `$ npm rm 〇〇`
インストールの逆。  
例えば、「jquery」を削除したいときは、`npm rm jquery`。  
これで「dependencies」の中と node_modulesにあった jqueryが削除される。
***
