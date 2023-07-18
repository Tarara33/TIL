# form_with
webサービスで新規登録やユーザログインのときに使われるフォーム画面のヘルパーメソッド
`form_with(モデル or スコープ or URL（３つの内どれか一つ指定） [, オプション])`の形
~~~
[rails]
<%= form_with(model: @user, local: true do |f| %>
      <%= f.label :name %>
      <%= f.text_field :name %>

      <%= f.submit "Create" %>
    <% end %>
    
    
[html]
<form accept-charset="UTF-8" action="/users" class="new_user"
      id="new_user" method="post">
  
  <label for="user_name">Name</label>
  <input id="user_name" name="user[name]" type="text" />

  <input  name="commit" type="submit"
         value="Create" />
</form>
~~~
となる
***

## オプション
- accept    
許可されるファイルのタイプを指定する。   
~~~
例：　<%= file_field :board_image , accept: "image/*" %>

「*」はMIMEタイプの部分に対して部分的な一致を表す特殊な記号。
具体的には、image/の後に続く任意の拡張子やMIMEタイプを受け入れることを示す。
今回の例では、image/jpeg、image/png、image/gifなど、image/で始まるMIMEタイプのファイルを受け入れることを意味する。
~~~
***

- onchange    
ユーザーが要素の値を変更したときに発生するイベントを処理するために使用される。   
具体的には、ユーザーがテキストフィールドにテキストを入力したり、セレクトボックスの選択肢を変更したり、   
ファイルアップロードフィールドでファイルを選択したりするなど、   
要素の値に変更があった場合にonchangeイベントがトリガーされる。
~~~
例：　<%= file_field :board_image , onchange: 'previewImage(preview)' %>

ファイルがアップロードされたときに、onchangeイベントが発生して   
JavaScriptの関数previewImageを呼び出すことを意味する。引数としてpreviewが渡されることも示されている。
~~~
***

## 検証で見てほしいこと
①action属性...このフォームに入力した情報をどこに送信するか   
②input要素...各項目の入力エリア    
③input要素のname属性...各項目の識別子。入力値に対して名前を付けてあげないとサーバは送られてきた入力値を識別できない。   
~~~
<form class="new_board" ①action="/boards" accept-charset="UTF-8" method="post"><input name="utf8" type="hidden" value="✓"><input type="hidden" name="authenticity_token" value="RmOCBP4FNs6fX8mozD9nOyFROqb4RYJuXGIkqL9DzYlrUOscj3ySsYn/dFP2TU9G0TzQ0GFM8Z2x7Fb9+qeoCQ==">
        <div class="form-group">
          <label for="board_title">タイトル</label>
          <②input class="form-control" type="text" ③name="board[title]"　=>boardモデルのtitleカラムに入るよって意味 id="board_title">
        </div>
</form>
~~~
***
