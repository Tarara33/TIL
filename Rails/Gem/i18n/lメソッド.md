# lメソッドとは？
日付や時刻を表す際に用いる。  
国や地域によって時刻は異なりますが、lメソッドを使えば現地時間に対応できる。
***

# 使い方
例えば以下のような国際化フォルダがあり、
~~~
[config/locales/ja.yml]

ja:
  time:
    current: "%Y年%m月%d日"


[config/locales/en.yml]
en:
  time:
    current: "%m %d, %Y"
~~~
Viewファイルで
~~~
<%= l(Time.current) %>
~~~
このように書くと、それぞれの現地時間を、現地の言葉や表記方法で表示できる。
***
