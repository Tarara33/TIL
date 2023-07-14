# meta
Webページの情報を Webブラウザや検索エンジンに伝えるため HTMLに記述されるタグのこと。    
⚠️ 閉じタグいらない。
***

# metaの後につく属性
## charset=""
文書の文字エンコーディングの設定。

### 【値】
`UTF-8` がよく使われる。 
~~~
<meta charset = "UTF-8">
~~~
***

## lang=""
文章の言語設定。 

### 【値】
- ja...日本語    
- en...英語    
- zn...中国語 などなど
~~~
<meta lang = "ja">
~~~
***

## name="name" と content="value"
~~~
<meta name="format-detection" content="telephone=no">
=> 主にスマートフォンなどが電話番号を勝手にリンク設定することを避けるための記述。    
      

<meta name="robots" content="noindex, nofollow">
=> 検索エンジンのクローラに対して、このページをインデックス化せず、リンクのたどりもしないように指示する。
  　　つまり、このページは検索結果に表示されないように設定される。


<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0">
=> ページの表示領域を指定するための記述。
  　　この設定をしていないと、PCとスマートフォン、タブレットなど各デバイスごとに画面幅が違うブラウザで見た時に、
  　　ページが小さく表示されてしまったり、はみ出してしまったりする場合がある。


<meta name="description" content="ここにページの概要が入ります。">
=> ページの説明・概要を指定する記述。
   検索結果にタイトルと一緒に表示される文章でもある。
~~~
などなど。
***

# ファビコンの設定
ブラウザタブの名の横につくアイコンのこと  
~~~
<link rel = "icon" href = "画像ファイル名" >
~~~
***

