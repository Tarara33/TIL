# メソッドの呼び出し制限
Rubyではクラス内のメソッドに呼び出し制限を設定することができ、以下の種類がある。

# public
制限なし（デフォルトではこの設定になっている）。
***

# private
クラスの外からは呼び出せない。  
同じインスタンス内でのみ、関数形式で呼び出せる

例: ストロングパラメーターなど
***

# protected
クラスの外からは呼び出せない。  
同じインスタンス内で呼び出せる。  
また、他のインスタンスでも同じクラスやサブクラスであれば呼び出せる。
***

# コードで見る
~~~
class User
  def hello
    p "こんにちは"
  end

  private

  def love
    p "好きです！"
  end

  protected

  def friend
    p "ズッ友だよ！"
  end
end
~~~
***

## publicを呼ぶ場合
Userクラスのインスタンスを作成して、それぞれのメソッドを呼び出してみる。
~~~
me = User.new

me.hello
=> "こんにちは"

me.love
=> エラー

me.friend
=> エラー
~~~
結果、publicな helloは呼び出せた。
***

## private/protectedを呼ぶ場合
publicなメソッド内で、private/protectedメソッドを呼ぶと使える。  
コード内にメソッド追加！
~~~
class User
  def hello
    p "こんにちは"
  end

  🆕def kokuhaku
    love
  end

  🆕def nakayoshi
    friend
  end

  private

  def love
    p "好きです！"
  end

  protected

  def friend
    p "ズッ友だよ！"
  end
end
~~~
~~~
me = User.new

me.kokuhaku
=> "好きです！"

me.nakayoshi
=> "ズッ友だよ！"
~~~
***

## ❓ protectedの特徴
protectedは他のインスタンスでも同じクラスやサブクラスであれば呼び出せるという特徴を持つ。
~~~
class User
  def initialize(name)
    @name = name
  end

  def become_friends(user)
    p "#{get_name}さんと#{user.get_name}さんが友達になりました"
  end

  protected

  def get_name
    @name
  end
end

----------------------------

class Robot < ⭐️User
  def initialize(name)
    super(name)
  end
end
~~~
~~~
me = User.new('たらら')
her = User.new('橋本環奈')
robot = Robot.new('ロボット')

me.become_friends(her)
=> "たららさんと橋本環奈さんが友達になりました"

me.become_friends(ロボット)
=> "たららさんとロボットさんが友達になりました"
~~~
Robotクラスは Userクラスを継承してるので protectedメソッド使える。
***
