# 現在の使用バージョン
~~~
$ ruby -v
=> ruby 2.5.1p57 (2018-03-29 revision 63029) [-darwin22]
~~~
***

# インストールしたバージョン一覧
~~~
$ rbenv versions
system
* 2.5.1 (set by /Users/sarina/.rbenv/version)
  2.6.3
  2.6.4
  2.6.5
  2.6.6
  2.7.4
  2.7.8
  3.0.2
  3.1.4
~~~
***

# インストール
## インストール可能なリスト一覧
~~~
$ rbenv install -l
~~~
***

## インストール方法
~~~
$ rbenv install バージョン指定
~~~
***

# 設定
~~~
[グローバル設定]
$ rbenv global バージョン指定  

[ローカル設定]
$ rbenv local バージョン指定
~~~
***

# M1マック
上のインストール方法だとエラー出ることある。    
その場合、コレでやってみて。
~~~
$ OPENSSL_CFLAGS=-Wno-error=implicit-function-declaration RUBY_CFLAGS=-DUSE_FFI_CLOSURE_ALLOC rbenv install バージョン
~~~
***
