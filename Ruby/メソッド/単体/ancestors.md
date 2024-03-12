# ancestors
クラス、モジュールのスーパークラスとインクルードしているモジュールを優先順位順に配列に格納して返す。
~~~
module Foo
end

class Bar
  include Foo
end

class Baz < Bar
end

Baz.ancestors
# => [Baz, Bar, Foo, Object, Kernel, BasicObject]
~~~
***
