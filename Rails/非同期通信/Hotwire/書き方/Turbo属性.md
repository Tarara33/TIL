# Turbo属性をつける
基本的に `<turbo-frame>`内からのリンクやフォームのリクエストが Turbo Frameリクエストになるが、  
他の場所でタグを使っていたりで使えない場合。

そんな場合には`data-turbo-frame`属性を指定してあげればいい。
~~~
[例]

<%= search_form_for @search do |f| %>

# data-turbo-frame属性を指定する
<%= search_form_for @search, html: { data: { turbo_frame: "cats-list" } } do |f| %>
~~~
これでレスポンスされた HTMLから  
`<turbo-frame id="cats-list">...</turbo-frame`>に一致する箇所だけを置換してくれるようになる。
***
