# 例：コメント機能のAjax化
コメントの作成・削除をAjax化する。
***

## 元のコード
作成成功したらリダイレクトされる機能。    
（削除機能実装してない）
~~~
[app/controllers/comments_controller.rb]
class CommentsController < ApplicationController
  def create
    comment = current_user.comments.build(comment_params)
    if comment.save
      redirect_to board_path(comment.board), success: t('defaults.message.created', item: Comment.model_name.human)
    else
      redirect_to board_path(comment.board), danger: t('defaults.message.not_created', item: Comment.model_name.human)
    end

  def destroy
    comment = current_user.comments.find(params[:id])
    comment.destroy!
    redirect_to board_path(comment.board), success: t('defaults.message.deleted', item: Comment.model_name.human)
  end
~~~
***
~~~
[app/views/comments/_form.html.erb]
<%= form_with model: comment, url: [board, comment], local: true do |f| %>
   <%= render 'shared/error_messages', object: f.object %>
   <%= f.label :body %>
   <%= f.text_area :body, class: 'form-control mb-3', id: 'js-new-comment-body', row: 4, placeholder: Comment.human_attribute_name(:body) %>
　　　　　　<%= f.submit t('defaults.post'), class: 'btn btn-primary' %>
<% end %>
~~~
***
~~~
[app/views/comments/_comment.html.erb]
<a href="#" class='js-delete-comment-button' data-comment-id="<%= comment.id %>">
  <%= icon 'fas', 'trash' %>
</a>
~~~
***


## コントローラー編集
①viewファイルを作るのでインスタンス変数にする。     
②if文での分岐をJSに託すので分岐消す。    
③リダイレクトしないので消す。
~~~
[app/controllers/comments_controller.rb]
class CommentsController < ApplicationController
  def create
    @comment = current_user.comments.build(comment_params)
    @comment.save

  def destroy
    @comment = current_user.comments.find(params[:id])
    @comment.destroy!
  end
~~~
***

## フォーム編集
①デフォルトでAjaxなので、「local: true」を削除する。    
②エラーメッセージの出しわけもAjaxになるので消す。    
③create処理時にエラーメッセージを埋め込む。    
その位置を特定するため、form_withには「id:new_comment」を追加しておく。
~~~
[app/views/comments/_form.html.erb]
<%= form_with model: comment, url: [board, comment], id: 'new_comment' do |f| %>
   <%= f.label :body %>
   <%= f.text_area :body, class: 'form-control mb-3', id: 'js-new-comment-body', row: 4, placeholder: Comment.human_attribute_name(:body) %>
　　　　　　<%= f.submit t('defaults.post'), class: 'btn btn-primary' %>
<% end %>
~~~
***

## リンク編集
①link_toでAjax化するので「remote: true」つける。    
②メソッドをdeleteにする。
~~~
[app/views/comments/_comment.html.erb]
<%= link_to comment_path(comment),
  class:'js-delete-comment-button',
  data: {confirm: t('defaults.message.delete_confirm') },
  method: :delete,
  remote: true do %>
  <%= icon 'fas', 'trash' %>
<% end %>
~~~
***

## JSのViewファイル作る
~~~
[app/views/comments/create.js.erb]

# 既に表示されているエラーメッセージがあった場合は削除する
$('#error_messages').remove();
=> 「app/views/shared/_errors.html.erb」でid設定したエラーメッセージ表示するdivブロック

# コメント作成処理の結果によって処理を分岐させる
<% if @comment.errors.present? %>
# エラーがある、処理失敗時にはエラーメッセージのパーシャルを表示させる
  $('#new-comment').prepend("<%= j(render('shared/errors', object: @comment)) %>");

<% else %>

# エラーがない、処理成功時には作成されたコメント内容をHTML要素として追加する
  $('#js-table-comment').prepend("<%= j(render('comments/comment', comment: @comment)) %>");

# コメント入力フォームのテキストは表示する必要がないので、空文字に置き換えて内容をクリアする
  $('#js-new-comment-body').val('');
<% end %>
~~~
***

~~~
[app/views/comments/destroy.js.erb]

$("tr#comment-<%= @comment.id %>").remove();
~~~
***

### ⭐️prepend()　と　remove()
- prepend...対象のノードの子ノードの先頭にノードを追加する。
- remove...ノードを削除する。    
[詳しくは一番下の方にある](https://github.com/Tarara33/TIL/blob/main/JavaScript/DOM/%E3%83%8E%E3%83%BC%E3%83%89%E3%81%AE%E5%8F%96%E5%BE%97%E3%80%81%E4%BD%9C%E6%88%90%E3%80%81%E5%A4%89%E6%9B%B4.md)
***



