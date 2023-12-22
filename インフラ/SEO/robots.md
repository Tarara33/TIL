# robots.txt
特定のウェブクローラに対して、クロールしても良いページやクロールしてはいけないページなどの指示をするファイル。 

例えば、Webアプリにおいて、  
「アプリのページは Google検索の結果に出していいよ、でも管理人用ページは検索結果に出さないで」のような指示ができる。
***

# 作り方
## robots.txtファイルを作る
アプリの publicディレクトリ配下に robots.txtという名前のファイルを作る。
***

## robots.txt編集
以下の場合、すべてのページをクロールして OKという意味になる。
~~~
[public/robots.txt]

User-agent: *
Disallow:
~~~
##### User-agent: *
全てのウェブクローラを表す。
***

##### Disallow:
この配下に表示したくないページのディレクトリなどを指定する。
~~~
[public/robots.txt]

User-agent: *
Disallow: /admin/
~~~
この書き方だと、adminフォルダ配下のページはクロールしないように指示している。
***

# ⭐️ サイトマップを登録
[サイトマップ](https://github.com/Tarara33/TIL/blob/main/%E3%82%A4%E3%83%B3%E3%83%95%E3%83%A9/SEO/%E3%82%B5%E3%82%A4%E3%83%88%E3%83%9E%E3%83%83%E3%83%97.md)を作っていたら robots.txtに記載する。
~~~
[public/robots.txt]

User-agent: *
Disallow: /admin/

Sitemap: https://gift-compass.com/sitemap.xml.gz
~~~
`$ rails sitemap:refresh`コマンドで出てきた URLを書いている。
***

# ❓ ログインが必要なページも Disallowに書いたほうがいい？？
結論、書かなくてOK!  
なぜなら、クローラーはログインが必要なページにはアクセスできないので、  
そもそもインデックス(検索エンジンがウェブページを収集してデータベースに登録すること)されることはないから。

ただし、クローラーがログイン不要のページからログイン必要なページに誤ってたどり着けるようなリンクがある場合は、    
そのページを Disallowで指定してクローラーがアクセスしないようにするのがいいかもしれない。
***

# ❓ Google Search Consoleに登録する？？
サイトマップのように登録する？？
=> 登録はしないでOK  
robots.txtはサーバーに置いておくだけで、検索エンジンがアクセスしたときに読み込まれる。
***



