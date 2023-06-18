# resources :コントローラー名
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
~~~
***

