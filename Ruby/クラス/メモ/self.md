# メソッド内での self
メソッド内で[ゲッター・セッター](https://github.com/Tarara33/TIL/blob/main/Ruby/%E3%82%AF%E3%83%A9%E3%82%B9/%E3%82%B2%E3%83%83%E3%82%BF%E3%83%BC%E3%81%A8%E3%82%BB%E3%83%83%E3%82%BF%E3%83%BC.md)を使う時、  
ゲッターの場合は self省略できるが、セッターはできない。
~~~
class User
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def hello
    # ゲッターメソッドをセット + selfつけない
    puts "Hello I am #{name}"
  end

  def hi
    # ゲッターメソッドをセット + selfつける
    puts "Hi I am #{self.name}"
  end

  def rename_Bob
    # セッターメソッドをセット + selfつけない
    name = 'Bob'
  end

  def rename_Jun
     # セッターメソッドをセット + selfつける
     self.name = 'Jun'
  end
end
------------------------------------------------------------

user = User.new('Tarara')

⭕️user.hello
=> Hello I am Tarara

⭕️user.hi
=> Hi I am Tarara

❌user.rename_Bob
user.name
=> "Tarara"

⭕️user.rename_Jun
user.name
=> "Jun"
~~~
結果、「セッターメソッドセット + selfつけない」 rename_Bobメソッドが使えてない。  

理由としては、 name = 'Bob'とただの変数に代入扱いになっているから。  
💡 なので セッターメソッド使うなら self必要!
***
