# importの注意点
例えば　同じ階層の CSSファイルなど使うときこのように書く。
~~~
import "./sample.css";
~~~

`./`がなく
~~~
import "sample.css";
~~~
と書いてしまうと、node_modulesの中のファイルを探しに行ってしまう。
***
