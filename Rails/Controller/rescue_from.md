# rescue_from
rescue_fromは例外処理をとりまとめる Railsの機能。

rescue_fromは例外の種類を指定し、そのときに実行する処理を記述しておけば、  
１つのコントローラファイルの中のすべてのアクションで発生する例外をキャッチしてくれる。  
また、withオプションで例外時に実行するメソッドを指定する。
~~~
rescue_from キャッチする例外名, with: :メソッド名

private

def メソッド名
  例外発生時に実行する処理
end
~~~
⚠️ rescue_fromは下から優先的に実行する性質があるため、複数のエラーを複合する例外クラスは上に記述する必要がある。
***
