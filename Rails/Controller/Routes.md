# resources (複数系　リソース) :コントローラー名
resourcesメソッドに含まれる
-　index...一覧    
-　show...詳細    
-　new...新規作成    
-　create...登録    
-　edit...編集    
-　update...更新    
-　destroy...削除        
の７機能が使えるルート一式を定義する    
***

## resourcesのオプション
- only...指定したアクションのみ作成する
- except...指定したアクション以外を作成する
~~~
resources :tasks, only: [:index]
=> indexアクションルートのみ作成

resources :tasks, except: [:delete, :edit, :update]
=> destroy,edit,update以外の４アクションのルートを作成する
~~~
***

# resource (単数系　リソース)
resources と違ってコントローラの７つのアクションに対して、indexとid付きのパスが生成されない。    
show, editアクションの実行に、idが必要ない場合に有効なので、    
例えばユーザー情報の編集画面などで「profile/1/edit」と idがあると、    
何番めに作ったユーザーだとかわかってしまうし、自分以外の編集しないのでidなくていい。    
という場合などに使える！
~~~
resource :profiles, only: %i[show edit update]
~~~
**resourceで作った場合**
[![Image from Gyazo](https://i.gyazo.com/4263a8a1d2a421b8aa1dbe888aec29ca.png)](https://gyazo.com/4263a8a1d2a421b8aa1dbe888aec29ca)
**resourcesで作った場合**
[![Image from Gyazo](https://i.gyazo.com/a2d66768f63a977f4bc1e1711a3a9a37.png)](https://gyazo.com/a2d66768f63a977f4bc1e1711a3a9a37)
***

# ネスト
コントローラーの親子関係を作る。    
URLが「/boards(親)/:board_id/comments（子）」のような感じになる。
~~~
[config/routes.rb]

resources :boards do
  resources :comments
end
~~~
***

## ネストの注意点
⚠️resources do が増えることを「ネストが深くなる」と言う。      
扱いにくいのでネストは一回に留めることが推奨されてる。    
「深いネスト」を作らないようにするために、コレクション（index/new/createのような、idを持たないもの）    
のアクションだけを親のスコープの下で生成するという手法がある。    
メンバー（show/edit/update/destroyのような、idを必要とするもの）    
のアクションをネストに含めないようにするのがポイント。
~~~
[config/routes.rb]

resources :boards do
  resources :comments, only: %i[index new create]
end
~~~
⚠️この場合resources はついてるので、    
コレクションアクションは「/boards(親)/:board_id/comments（子）」のようになるが、    
メンバーアクションのルーティングも、    
「/comments/:id/edit(.:format)」のように作成はされている。    
***

## ネストのオプション
- :shallow
上で記述しているコレクションアクションの書き方をこれで定義できる
~~~
[config/routes.rb]

resources :boards do
  resources :comments, shallow: true
end
~~~
親側に書くと親子両方に反映される
~~~
[config/routes.rb]

resources :boards, shallow: true  do
  resources :comments
end
~~~
***

# member と collection
7つのresourcesアクション以外に自分で作った時に使う。（たぶん）    
簡単に言うとIDをつけるかつけないか。    
:id を使用した特定のデータに対するアクションの場合は member を使用する。    
:id の必要ない全体のデータに対するアクションの場合は collection を使用する。    
💡get,patch,put,deleteなどの後に書く　「:〇〇」はコントローラー名ではなく、    
URLパスとそれに関連付けられたアクション名。
***

## member
routingにIDがつく(users/:id/...)の形    
例：userコントローラーにlogoutというアクションを作った。    
それを(users/:id/...)の形で使いたい。
~~~
[例]
resources :user do
  member do
    get :logout　
  end
end

=> users#logoutは、「/users/:id/logout」となる
~~~
***

## collection
routingにIDがつかない(users/...)の形
例：user_sign_upsコントローラーにtellというアクションを作った。    
それを(user_sign_ups/tell)の形で使いたい。
~~~
[例]
resource :user_sign_ups do
  collection do
    get :tell 
  end
end

=> user_sign_ups#tellは、「/user_sign_ups/tell」となる
~~~
***

## on
member, collectionに指定したいアクションが一つの時は
~~~
resources :user do
  member do
    get :logout
  end
end

=>

resources :user do
  get :login, on: :member
end

でもOK
~~~
***

# 似てる奴ら
~~~
get "login", to: "user_sessions#new"
=> 単一のルーティング設定。
　　　　[/　login]というURLパスにGETリクエストが送信された場合、user_sessions#newアクションが実行される。


resources :user_sessions do
  get :login, on: :collection
end
=> [/user_sessions/login]というURLパスにGETリクエストが送信された場合、
    user_sessions#loginアクションが実行されます。
~~~


# URL設計
[![Image from Gyazo](https://i.gyazo.com/dcde886cecd718f32c6ab9dc7b2c1a0d.png)](https://gyazo.com/dcde886cecd718f32c6ab9dc7b2c1a0d)
「URI Pattern」, 「Controller#Action」を操作する方法をまとめる。    
例として上の写真のstudentコントローラーを操作する。
[参考](https://techtechmedia.com/namespace-scope-module-routing/)

***

##  namespace
「URI Pattern」と「Controller#Action」の2つを同時にカスタマイズしたい場合に使用する。
~~~
[例：URI Pattern => classroom/student/,
 Controller#Action => classroom/student#アクション]にする場合

namespace :classroom do
  resources :student
end
~~~
💡アクションを書いていくコントローラーも「app/controllers/student_controller.rb」から    
「app/controllers/classroom/student_controller.rb」に配下を変える
~~~
[app/controllers/classroom/student_controller.rb]

module Crassroom
 class StudentsController < ApplicationController
    def index
      @students = Student.all
    end
  end
end
~~~
***
[![Image from Gyazo](https://i.gyazo.com/824603bc90cebd38bf234e7c57b4ccad.png)](https://gyazo.com/824603bc90cebd38bf234e7c57b4ccad)
***


## scope
「URI Pattern」のみカスタマイズしたい場合に使用する。
~~~
[例：URI Pattern => classroom/student,
 Controller#Action => student#アクションのまま]にする場合

scope :classroom do
  resources :student
end
~~~
💡コントローラーアクションはそのままなので、    
「app/controllers/student_controller.rb」でOK
***
[![Image from Gyazo](https://i.gyazo.com/3c3cf7b315cc78f52170be677833e7d8.png)](https://gyazo.com/3c3cf7b315cc78f52170be677833e7d8)
***

## scope　module
「Controller#Action」のみカスタマイズしたい場合に使用する。
~~~
[例：URI Pattern => student/のまま,
 Controller#Action => classroom/student#アクション]にする場合

scope module :classroom do
  resources :student
end
~~~
💡アクションを変えているのでコントローラーも「app/controllers/student_controller.rb」から    
「app/controllers/classroom/student_controller.rb」に配下を変える
~~~
[app/controllers/classroom/student_controller.rb]

module Crassroom
 class StudentsController < ApplicationController
    def index
      @students = Student.all
    end
  end
end
~~~
***
[![Image from Gyazo](https://i.gyazo.com/b60e0dceeb5bd6614438028445c4a49c.png)](https://gyazo.com/b60e0dceeb5bd6614438028445c4a49c)
***
