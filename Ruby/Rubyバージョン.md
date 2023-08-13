# バージョン一覧
`$ rbenv versions`
***

# 設定
` $ rbenv global バージョン指定`    
全部の領域で指定バージョンが使われる    
`$ rbenv local バージョン指定`   
実行したディレクトリで使われる
***

# インストール
`$ rbenv install バージョン指定`   
または   
`$ rbenv install -l`    
インストールできるリスト見れる
***

# M1マック
上のインストール方法だとエラー出ることある。    
その場合、コレでやってみて。
~~~
$ OPENSSL_CFLAGS=-Wno-error=implicit-function-declaration RUBY_CFLAGS=-DUSE_FFI_CLOSURE_ALLOC rbenv install バージョン
~~~
***
