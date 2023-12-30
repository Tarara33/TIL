# selectと collection_select
## select
[こちら](https://github.com/Tarara33/TIL/blob/main/Rails/View/%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E7%B3%BB/%E3%82%BB%E3%83%AC%E3%82%AF%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9.md#select%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89)でもまとめたが、選択肢を自分で配列として定義するときに使うメソッド。  
固定の値をドロップダウンで選べるようにするときに便利。
~~~
<%= f.select :genre_id, Genre.pluck(:name, :id), { include_blank: '選択してください' } %>
~~~
***

## collection_select[(参考)](https://zenn.dev/airiin/articles/913da8c5a99f2f)
ActiveRecordのコレクションから選択肢を自動で作成するメソッド。  
他のモデルのデータを選択肢にしたい場合は便利。
~~~
<%= f.collection_select 🧡:genre_id, 💚Genre.all, 🩵:id, 💙:name, { include_blank: '選択してください' } %>
~~~
- 🧡第一引数: 選択された項目が設定されるフォームの属性名
- 💚第二引数: 選択肢として表示するオブジェクトのコレクション
- 🩵第三引数: コレクションの各オブジェクトから選択肢の valueに設定される属性名
- 💙第四引数: コレクションの各オブジェクトから選択肢の表示テキストに設定される属性名
***
