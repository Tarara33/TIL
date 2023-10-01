# DSファイル
.DS_Storeは MacOSがディレクトリのカスタム属性を保存するために作るファイルで、消しても問題ないそう。  
設定で作らないようにもできるが、そのせいで Finderの振る舞いがおかしくなることもある。  
なので下記コードをたまに実行して消すぐらいが良さそう。
~~~
$ find . -name '.DS_Store' -type f -ls -delete
~~~
***

# 絶対パス / 相対パス 
例えばこのような階層の場合

[![Image from Gyazo](https://i.gyazo.com/f8757e7a327a1426d025a13fd3611371.png)](https://gyazo.com/f8757e7a327a1426d025a13fd3611371)
  
## 絶対パス
ルートディレクトリからのパスを指す。  
  
bye.rbファイルへのパス　=> /chery/foo/hello.rb  
「/」でルートディレクトリを表す。
***
  
## 相対パス
現在のディレクトリから見た対象ファイルへのパスを指す。  
    
yes.rbから見た no.rbへのパス => no.rb  
bye.rbから見た hello.rbへのパス => ../foo/hello.rb  
yes.rbから見た hello.rbへのパス => ../../foo/hello.rb  
***

### ❓ ルートディレクトリ
rails newや git cloneを実行したディレクトリがルートディレクトリになる。  
つまり、そのコマンドを実行した場所がプロジェクトのルートディレクトリとなる!
***
