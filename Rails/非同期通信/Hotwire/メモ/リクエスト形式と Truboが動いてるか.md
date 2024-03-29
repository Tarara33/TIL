# それぞれのリクエスト形式
HTML形式のリクエストは、ページをフルページを更新する。  
Turbo Stream形式のリクエストは、ページの一部分だけが更新される。
***

# 確認方法
このページは、HTML形式なのかTurbo Stream形式どちらかわからない時は、ログを見るといい。

[![Image from Gyazo](https://i.gyazo.com/164edc0f2290db283828bf679dd9e394.png)](https://gyazo.com/164edc0f2290db283828bf679dd9e394)
***

# ❓ じゃあリクエスト形式が HTMLだと truboじゃないの？？
答えは　NO。  
例えばサーバーが HTML形式でレスポンスを処理していることを意味しても、  
フロントエンドで Turboが機能しているかどうかは、サーバー側のログだけで判断することはできない。
***

# 💡 Truboが動いているか見分ける方法 
## ① ブラウザのデベロッパーツールで確認を開いて、ネットワークタブを見る。  
Turboを使ったリクエストは通常、XHR（XMLHttpRequest）や Fetchとして表示される。
***

## 例
例えばこの画面の「ねこボタン」、「いぬボタン」を押すときに、Truboが効いてるか確認する。

[![Image from Gyazo](https://i.gyazo.com/6cc664e3e5a54319a1804605066dc213.png)](https://gyazo.com/6cc664e3e5a54319a1804605066dc213)
***

まず、Turboを効かせてないもの、効かせてるもの、どちらもリクエストは HTML形式である。  
💡 Turboは GETリクエストではフォーマットが Turbo Streamにならない。  
GET以外の PUT/PATCH/DELETEでなる。

[![Image from Gyazo](https://i.gyazo.com/919a9ed9af0b3b28c001524fba972de9.png)](https://gyazo.com/919a9ed9af0b3b28c001524fba972de9)
***

そしてこれが Turboを効かせてないもの、効かせてるものの検証でのネットワーク。

[![Image from Gyazo](https://i.gyazo.com/0c24071450a1c825c3aca6afa771e347.png)](https://gyazo.com/0c24071450a1c825c3aca6afa771e347)

### Turboを効かせてない
まず「タイプ」を見てみると、Turboを効かせてないものは XHR（XMLHttpRequest）や Fetchではない。  
また、左の「名前」をみると、ボタンを押すたびにリロードされるので、一番上に押したボタン名が出るが、  
違うボタンを押すと、前に押したボタン名は消える。

### Turboを効かせてる
まず「タイプ」を見てみると、Fetchの表示がある。  
また、左の「名前」をみると、ボタンを押すたびに一部分が変わるので、  
違うボタンを押しても、前に押したボタン名は消えない。

#### ⚠️ タイプが fetchだからといって、必ずしもTurboを使っているわけではない。    
fetchでも画面が上までスクロールされたりしたら Turbo機能してない。
***

## ② ページが部分的に更新される
ページ遷移やフォームの送信を行った時に、ページ全体がリフレッシュされずに部分的に更新される場合は Turboが機能してる。
***

## ③ `turbo_frame_tag（<turbo-frame>）`がコード内にある。
`turbo_frame_tag（<turbo-frame>）`があると、リクエストヘッダーに Turbo-Frame:が含まれる。

[![Image from Gyazo](https://i.gyazo.com/98f5f61490cb540bbe28bea58ff56248.png)](https://gyazo.com/98f5f61490cb540bbe28bea58ff56248)

💡 `turbo_frame_tag（<turbo-frame>）`内にあるリンクやフォームは Turbo Frameリクエストになる
***
