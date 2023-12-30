# boolean型にはデフォルト値をつける
[参考](https://qiita.com/jnchito/items/a342b64cd998e5c4ef3d)

デフォルト値をつけないと、nullが入る可能性が出てくるため、  
true or falseで分けたいカラムが  
true or false or nillの3択になってしまう。
~~~
add_column :events, :only_woman, :boolean, default: false, null: false
~~~
***
