# 条件分岐で Viewが崩れる場合
このように、条件によって表示が変わり、見た目がズレる場合。  
  
[![Image from Gyazo](https://i.gyazo.com/82f5363a882580732caa953a75ca8d1e.png)](https://gyazo.com/82f5363a882580732caa953a75ca8d1e)
~~~
[ビューファイル](改善前)

<div class="card-body">
  <% if event.only_woman == true %>
      <h4 class="badge rounded-pill bg-danger">女性限定</h4>
  <% end %>
  <h4 class="card-title fw-bold">
    <%= link_to event.title, event_path(event)  %>
  </h4>

--------------------------------------------------------------
[ビューファイル](改善後)

<div class="card-body">
  <% if event.only_woman == true %>
    <h4 class="badge rounded-pill bg-danger">女性限定</h4>
  <% else %>
    <h4 style="height: 1.5em;"></h4>
  <% end %>
  <h4 class="card-title fw-bold">
    <%= link_to event.title, event_path(event)  %>
  </h4>
~~~
ifの条件で `<h4>`を使ってるので、 else側でも `style="height: 1.5em;"`というインラインスタイルで`<h4>`タグの高さを設定。

[![Image from Gyazo](https://i.gyazo.com/898ba08aca055cc744890e3b133c8e0b.png)](https://gyazo.com/898ba08aca055cc744890e3b133c8e0b)
***
