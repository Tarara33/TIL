# form_with
webサービスで新規登録やユーザログインのときに使われるフォーム画面のヘルパーメソッド      
`form_with(モデル or スコープ or URL（など指定） [, オプション])`の形
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

# url:
url:　で指定するURLは、入力フォームの送信先URL。       
なので大体は 「crateアクション」のURLパス or 「updateアクション」のURLパス。                   
***

## url:は書かなくてもいいの？？
modelのみ書いて　URL書かないこともあるが、それでも createや　updateアクションに飛べるのはなぜ？      
      
urlオプションを指定しない場合、form_withヘルパーはデフォルトの送信先 URLを生成する。       
デフォルトでは、modelオプションに基づいて以下の優先順位で URLが生成される。            
            
オブジェクトが新規作成 （new_record?が true）の場合は、createアクションに対する URLが生成される。      
オブジェクトが既存のレコード　（new_record?が　false）の場合は、updateアクションに対する　URLが生成される。      
***

### どうやって判別してるの？？
persisted? というメソッドを使い「 true なら patch、false なら post 」と判断している。[(ソースコード)](https://github.com/rails/rails/blob/8bec77cc0f1fd47677a331a64f68c5918efd2ca9/activerecord/lib/active_record/persistence.rb#L242-L247)    
ちなみに persisted?メソッドでは、 @new_recordと @destroyedの            
2つのインスタンス変数にアクセスしていて、どちらもfalseならtrueを返す。
      
【確認方法】
~~~
[rails c]

# DBに保存済みのインスタンスと未保存のインスタンスを用意します。
>> persisted_user = User.first
>> new_user = User.new

# @new_recordと@destroyedというインスタンス変数を持っているのか確認します。
>> persisted_user.instance_variables
# => [:@attributes, :@aggregation_cache, :@association_cache, :@readonly, :@destroyed, :@marked_for_destruction, :@destroyed_by_association, :@new_record, :@_start_transaction_state, :@transaction_state]
>> new_user.instance_variables
# => [:@attributes, :@aggregation_cache, :@association_cache, :@readonly, :@destroyed, :@marked_for_destruction, :@destroyed_by_association, :@new_record, :@_start_transaction_state, :@transaction_state]

# それぞれのインスタンス変数の値を確認します
>> persisted_user.instance_variable_get('@new_record')
# => false
>> persisted_user.instance_variable_get('@destroyed')
# => false
>> persisted_user.persisted?
# => true

>> new_user.instance_variable_get('@new_record')
# => true
>> new_user.instance_variable_get('@destroyed')
# => false
>> new_user.persisted?
# => false

# ちなみに削除済みも確認してみる
>> destroyed_user = persisted_user.destroy
>> destroyed_user.instance_variable_get('@new_record')
# => false
>> destroyed_user.instance_variable_get('@destroyed')
# => true
> destroyed_user.persisted?
# => false
~~~
***

# フォーム入力欄のオプション
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
