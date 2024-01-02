# 入れ子時に ＆を使う
SASS（インデント構文）では &を使わずにネストできるけど、  
SCSS（CSS拡張構文）では、ネストされたセレクタが親セレクタと結合する場合には&を使う必要がある。
~~~
[HTML]
  <div class="col-xl-3 mb-3">
    <button class="situation-btn xmas">ボタン3</button>
  </div>


[sassの場合]

.situation-btn {
  width: 300px;
  height: 170px;

  ⭐️.xmas {
    background-image: url('situation-xmas.jpg');
  }
}


[scssの場合]

.situation-btn {
  width: 300px;
  height: 170px;

  ⭐️&.xmas {
    background-image: url('situation-xmas.jpg');
  }
}
~~~
***
