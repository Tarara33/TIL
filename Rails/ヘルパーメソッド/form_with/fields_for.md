[参考](https://zenn.dev/goldsaya/articles/06d7e685a66431#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)

# fields_for
これを使うと親モデルに紐付いた子モデルのデータを編集できる。  

例えばこのような関係のテーブルの時に、親である Userモデルのフォームに 子である Profileモデルのフォームを入れられる。
[![Image from Gyazo](https://i.gyazo.com/702a0db398c47bb9fdd171204f571283.png)](https://gyazo.com/702a0db398c47bb9fdd171204f571283)
***

# 使い方
# ① accepts_nested_attributes_forを親モデルに書く
accepts_nested_attributes_forとはモデルメソッドであり、  
親モデルを保存するときに、Associationで関連づけた子モデルも一緒に保存することができるメソッド。  

~~~
[app/models/user.rb]

class User < ApplicationRecord
  has_one :profile, dependent: :destroy
  accepts_nested_attributes_for :profile
end
~~~
***

## accepts_nested_attributes_forのオプション [(参考)](https://railsguides.jp/form_helpers.html#%E8%A4%87%E9%9B%91%E3%81%AA%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)
###  allow_destroy: true
親モデルのフォームで子モデルの削除を許可する。  
親コントローラーで設定するストロングパラメーター内の子モデル用ハッシュにキー`_destroy`を追加する必要がある。  
***

### reject_if: 
子モデルのオブジェクト作成を自動的に拒否する。  
proc(ブロック)タイプなので、条件処理を書いて、その条件が trueの時は子モデルのオブジェクトは生成されない。  

`:all_blank`をつけることもできて、
~~~
accepts_nested_attributes_for :profile, reject_if: :all_blank
~~~
とかくと、子モデルの属性(今回は nameのみ)が全て空だったときは作成されない。
***

## ❓ dependent: :destroyついてるのに allow_destroy: trueつける必要ある？？
この二つは動きが違う。  
`dependent: :destroy`は、 あるUserオブジェクトが消された時に関連づく Profileオブジェクトも消される。  

`allow_destroy: true`は、フォームから Userを更新する時、同時に Profileオブジェクトを削除することができるようになる。  
ストロングパラメーター内の子モデル用ハッシュにキー`_destroy`を追加しているので、その値が`1`や `true`の場合消される。  
通常は、チェックボックスなどのフォーム要素を通じてユーザーが明示的に削除を指示する設計が多い。
***

# ② フォームを書く
親モデルのフォームにて`fields_for`を使って 子モデルのフォームを入れる。

`fields_for :モデル名 do...`とする。  
アソシエーションが単数系なら単数系、複数形なら複数系で書く。
~~~
[app/views/users/new.html.erb]

<%= form_with(model:@user, local: true) do |f| %>
  <%= render 'shared/errors', object: f.object %>
  
  🩵<%= f.fields_for :profile do |p| %>
    <div class="mb-3">
      <%= p.label :name %>
      <%= p.text_field :name, class: 'form-control' %>
    </div>
  <% end %>
  
  <div class="mb-3">
    <%= f.label :email %>
    <%= f.text_field :email, class: 'form-control' %>
  </div>
  
  <div class="mb-3">
    <%= f.label :password %>
    <%= f.text_field :password, class: 'form-control' %>
  </div>
  
  <div class="mb-3">
    <%= f.label :password_confirmation %>
    <%= f.text_field :password_confirmation, class: 'form-control' %>
  </div>
  
  <%= f.submit class: 'btn btn-primary d-block mx-auto' %>
<% end %>
~~~
⚠️ 通常のフォームは form_withのフォームビルダーオブジェクト(f.)使うが、  
fields_forを使うフォームはそのフォームビルダーオブジェクトを使うので(ここでは p.)注意。
***

# ③ コントローラーの編集
親モデルのコントローラーを編集する。

## newアクション
~~~
[app/controllers/user_controller.rb]

def new
  @user = User.new
  🧡@user.build_profile
end
~~~
🧡 @userに対して関連する Profileモデルの新しいインスタンスを作成するために書く。  
⚠️ これを書かないと、フォーム画面で子モデルの欄が出てこない！

💡 アソシエーションが 
- `has_one`なら`@親モデルインスタンス.build_子モデル`  
- `has_many`なら`@親モデルインスタンス.子モデルs(複数形).build`

と書く。
***

## ストロングパラメーター
検証で子モデルのフォームを見てみると、このようにネストされてるので、以下のように書く。

[![Image from Gyazo](https://i.gyazo.com/2dde5681b18cf29e073bcf6925261aa9.png)](https://gyazo.com/2dde5681b18cf29e073bcf6925261aa9)
~~~
[app/controllers/user_controller.rb]

private

  def user_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation, 💚profile_attributes: [:name])
  end
~~~

また、もし `allow_destroy: true`をつけていた場合は、このように書く。
~~~
[app/models/user.rb]

class User < ApplicationRecord
  has_one :profile, dependent: :destroy
  accepts_nested_attributes_for :profile, 💛allow_destroy: true
end


[app/controllers/user_controller.rb]

private

  def user_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation, 💛profile_attributes: [:name, :_destroy])
  end
~~~
***


