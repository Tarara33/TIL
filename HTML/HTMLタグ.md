# ul ol dl
- ul...liタグと組み合わせて「・」のリストを作る
- ol...liタグと組み合わせて「1.」のリストを作る
- dl...説明付きのリスト　
       dtタグでリストアイテムのタイトル　　
       ddタグでリストアイテムの説明をつける
~~~
<dl>
  <dt>アイテム１</dt>
  <dd>アイテム一つめです。</dd>
</dl>
~~~
***

# table
表を作るタグ
- thead...表の見出しを作る
- tbody...表の本体を作る
- tr...表の行数を作る（theadとtbody両方分のtrタグ入れる）
- th...theadで使うセルを作る
- td...tbodyで使うセルを作る

~~~
<table>
  <thead>
    <tr><th>見出し１</th><th>見出し２</th></tr>
  </thead>
  <tbody>
    <tr><td>見出し１の内容１</td><td>見出し２の内容１</td><tr>
    <tr><td>見出し１の内容２</td><td>見出し２の内容２</td><tr>
  </tbody>
</table>
~~~
***

# nav
ブラウザのナビゲーションを作る      
見た目は変わらないが検索エンジンのようなプログラムが文書を解析しやすくなる
***

# label
入力フォームで何を入力して欲しいかのタイトルみたいな感じ       
このラベルがどの入力部品に関するものかを明示しておくと、コンピュータやブラウザがこの文書を解釈しやすくなる
~~~
<label　for = "name">お名前</label>
<input type = "text" id = "name">
~~~
inputの方のid名と同じものをlabelのforに入れる     
紐付けることでブラウザでもラベルのほうをクリックすると、こちらの関連する入力部品が青くなってすぐに入力できるようになるので便利    
音声読み上げブラウザでは、こちらの入力部品にフォーカスが当たったときにこちらの関連するラベルを読み上げてくれる
***

# fieldset　と　legend
- fieldset...入力フォームのチェックBOXなどをまとめる
- legend...fieldsetでまとめたグループの説明
~~~
<fieldset>
  <legend>対象の商品</legend>
  <label><input type = "checkbox">商品A</label>
  <label><input type = "checkbox">商品A</label>
</fieldset>
~~~
***

# select　と　option
- select...ドロップリストを作る
- option...選択肢
~~~
<select>
  <option>選択肢１</option>
  <option>選択肢２</option>
  <option>選択肢３</option>
</select>
~~~
***

       
       
       
