# googleフォント設定の方法
### 1. [サイト](https://fonts.google.com/?subset=japanese&noto.script=Hira)にいく。
***

### 2. フォントを探す。
[![Image from Gyazo](https://i.gyazo.com/7b48ebb5bc14c2744ba7b0f14da2bc53.png)](https://gyazo.com/7b48ebb5bc14c2744ba7b0f14da2bc53)

すごく種類が多いので、Filtersで検索した方が見つけやすい。

【検索内容】
- Previewで実際の文字を試せる。(漢字対応していないものあるので試すとわかりやすい)
- LangLanguageで言語設定して探す。(英語しか対応してないものある)

など
***

### 3. 読み込むフォントを決める
[![Image from Gyazo](https://i.gyazo.com/3320ed34132ee7f3c5c16a57b806e138.png)](https://gyazo.com/3320ed34132ee7f3c5c16a57b806e138)

フォントの詳細ページに行くと、 100とか 300とかす数字の横に +-ボタンがある。  
これは +にするとダウンロードするフォントに含まれるようになる。
***

### 4. CSSにかく
[![Image from Gyazo](https://i.gyazo.com/1d24c51e079e127eead4a828d7f0971f.png)](https://gyazo.com/1d24c51e079e127eead4a828d7f0971f)

右にあるカバンボタンを押すと、Reviewのところに先ほど +にしたフォントたちが含まれる。  
そして、下の linkか importを選ぶ。    
bodyタグ内に その下の font-family...を書くと、全体に適応される。  

今回は試しに app/assets/stylesheet/application.scssに書く例
~~~
[app/assets/stylesheet/application.scss]

@import url('https://fonts.googleapis.com/css2?family=M+PLUS+1p:wght@300;500&display=swap');

body {
  font-family: 'M PLUS 1p', sans-serif;
}
~~~
***
