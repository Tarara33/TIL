# GET メソッド
~~~
$ curl http://localhost:3000/articles(取得したいページのURL)
=>
<!DOCTYPE html>
<html>
  <head>
    <title>CurlExample</title>
    <meta name="csrf-param" content="authenticity_token" />
    <meta name="csrf...省略
~~~
***

# POST メソッド
~~~
$ curl -X POST -F 'article[title]=hoge' -F 'article[content]=fuga' http://localhost:3000/articles(NewアクションのページのURL)
                  =>モデル名[キー名]　= 値
  
=><html><body>You are being <a href="http://localhost:3000/arti...省略
~~~
***

# PATCH メソッド
~~~
$ curl -X PATCH -F 'article[title]=hogeeee' -F 'article[content]=fugaaaa' http://localhost:3000/articles/1(ShowアクションのページのURL)
=>⚠️URL最後の:idあるURLにする
=><html><body>You are being <a href="http://localhost:3000/arti...省略
~~~
***

# DELETE　メソッド
~~~
$ curl -X DELETE http://localhost:3000/articles/1(ShowアクションのページのURL)
=><html><body>You are being <a href="http://localhost:3000/arti...省略
~~~
***
