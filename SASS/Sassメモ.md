# 変数定義
`$変数名`で設定できる
~~~
[sacc]
$color: fffff;

h1 {
  bsckground-color: $color;
  => fffffが適応される
~~~
⚠️定義した場所によっては反映されない   
- 変数を使う場所より下に定義（ファイルは上から処理されるのでそんな変数ないよとエラーになる）   
- 変数を{}内で定義（スコープが届かないので{}外では使えない）
***

# @mixinと　@include
`@mixin mixin名 {} `で定義して、   
`{@include　mixin名} `　でSassファイル内で使える
~~~
[sacc]
@mixin example {
  color: fffff;
  font-size:16px;
  font-weight: bold;
}

h1 {
@include example
background-color: red;
}
=> @mixinの内容が反映される
~~~
***

# 引数
@mixinに引数を渡せる
~~~
[sacc]
@mixin example($color) {
  color: $color;
  font-size:16px;
  font-weight: bold;
}

h1 {
@include example(red)
background-color: red;
}
=> 文字色がredになる

h2 {
@include example(blue)
background-color: red;
}
=> 文字色がblueになる
~~~
***

# 入れ子構造
~~~
[html]
<div class = "content">
  <h1></h1>
  <p></p>
</div>

[sass]

.content {
  margin: 0 auto;
  padding: 16px;
  
  h1 {
    color: black;
  }
  
  p {
    color: white;
  }
}
=> CSSでいう.content h1 {...と同じ意味
~~~
***

# 動きをつける
- :active...クリックしたときの動き
- :hover...ホバーした時
***

# &
~~~
[html]
<ul>
  <li></li>
  <li class="second"></li>
  <li></li>
</ul>

[sass]
li {
  color: black;
  
  &.second {
  color: red;
  }
}
=> li.secondと同じ意味
~~~
***

# @extend
CSSのスタイルを再利用するために使用される機能。
~~~
[SASS]

// ベースのスタイルを定義
.button {
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
}

// .buttonを拡張して別のクラスを作成
.submit-button {
  @extend .button;
  font-weight: bold;
}

// .buttonを拡張して別のクラスを作成
.cancel-button {
  @extend .button;
  background-color: #dc3545;
}
~~~
***
