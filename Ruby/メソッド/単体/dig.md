# dig
dig メソッドは、多重の連想配列やハッシュ内の値を安全に取得するためのメソッド。

通常のハッシュアクセスの場合、存在しないキーにアクセスすると NoMethodErrorが発生する。
~~~
data = { 'key1' => { 'key2' => { 'key3' => 'value' } } }
value = data['key1']['key2']['key3']  # これが成功する
value = data['key1']['key2']['key4']  # これが NoMethodError を引き起こす
~~~

しかし、digメソッドを使用すると、途中のキーが存在しなくてもエラーを回避して値を取得できる。
~~~
value = data.dig('key1', 'key2', 'key3')  # これが成功する
value = data.dig('key1', 'key2', 'key4')  # これも成功し、nilを返す
~~~
***
