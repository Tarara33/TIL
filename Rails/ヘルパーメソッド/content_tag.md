# content_tag
HTMLを作ってくれるヘルパーメソッド    
第一引数にHTMLタグ名、    
第二引数にタグで挟むテキスト    
あとは classや idも指定できる。    
~~~
<%= content_tag(:p, 'テスト') %>
=> <p>テスト<_p>

<%= content_tag(:p, 'テスト', id: 'test') %>
=> <p id='test'>テスト<_p>

<%= content_tag(:div, id: 'test') do %>
  <%= content_tag(:p, 'テスト' %>
<% end %>
=> <div id="test">
    <p>テスト</p>
   </div>
~~~
***

