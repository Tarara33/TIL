# include と require と require_relative
クラス内でかくこの表記は  
- include...モジュールをクラス内に取り込むために使用される。    
- require...外部のファイルや標準ライブラリをプログラム内で使用できるようにするために使用される。
- require_relative...他ファイルに定義したクラスなどを読み込む。(相対パス)
~~~
[ruby/foo/hello.rb]

class SignUpForm
  include ActiveModel::Model
=> ActiveModel::Modelと言うモジュールの取り込み

  require 'net/http'
=> Net::HTTPResponseと言うクラスの取り込み

  require_relative '../bar/bye'
=> foo/hello.rbから見た相対パスで bar/bye.rbに書いてあるクラスを取り込む。
end
~~~
[Net::HTTPResponse](https://docs.ruby-lang.org/ja/latest/class/Net=3a=3aHTTPResponse.html#I_HEADER)



[![Image from Gyazo](https://i.gyazo.com/f328fefdc66d690aa57c429a10d1ea57.png)](https://gyazo.com/f328fefdc66d690aa57c429a10d1ea57)
***
