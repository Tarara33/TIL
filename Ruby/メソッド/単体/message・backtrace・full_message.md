[例外オブジェクトに対して使えるメソッドたち](https://docs.ruby-lang.org/ja/latest/class/Exception.html#I_MESSAGE)

# message
エラーメッセージをあらわす文字列を返す。
~~~
begin
  1 + nil
rescue => e
  p e.message
  #=>  "nil can't be coerced into Fixnum"
end
~~~
***

# backtrace
バックトレース情報を返す。
~~~
def methd
  # わざと例外を発生させる
  raise
end

begin
  methd
rescue => e
  p e.backtrace
end

#=> ["filename.rb:2:in `methd'", "filename.rb:6"]
~~~
***

### ❓ バックトレースとは??
バックトレースってのは、プログラムがエラーに遭遇した時点でのメソッド呼び出しの履歴をすリストのこと。  
プログラムがどのような経路をたどってエラーが発生したかを示してくれるから、デバッグするときにめちゃくちゃ役に立つ！
***

# full_message
messageメソッドよりも詳細な情報を含むエラーメッセージを返す。  
具体的には、エラーメッセージに加えて、発生した場所のバックトレース情報も含まれる。  

⚠️ バックトレースも含むが、messageメソッド + backtraceメソッドを使った結果とは違う。
***
