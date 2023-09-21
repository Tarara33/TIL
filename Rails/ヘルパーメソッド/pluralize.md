# pluralize
文法的に正確な複数形を生成するのに役立つ。  
`pluralize(カウント数, "単語")`と使う。  
  
たとえば、エラーメッセージの表示で、  
エラーが一つなら「error」  
エラーが二つなら「errors」  
と表示を分けたいときに便利！  
~~~
The form contains <%= pluralize(object.errors.count, "error") %>
~~~
***
