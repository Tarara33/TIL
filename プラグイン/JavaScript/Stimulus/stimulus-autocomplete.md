[GitHub](https://github.com/afcapel/stimulus-autocomplete)  
[参考](https://qiita.com/Yamamoto-Masaya1122/items/879d6eb540ce4e05cfe5#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)

# stimulus-autocomplete
JavaScriptのプラグインで、Stimulusフレームワークを使っているプロジェクトに  
「オートコンプリート機能」を簡単に追加するためのもの。

例えば、「蟹」と検索したら、「蟹」が含まれるものが自動的に探される。

[![Image from Gyazo](https://i.gyazo.com/6198c22e80020597b6a0e8febc3fb25a.png)](https://gyazo.com/6198c22e80020597b6a0e8febc3fb25a)
***

# 準備
## ダウンロード
**Rails7で使用する場合**
- importmap使用の場合
  ~~~
  $ ./bin/importmap pin stimulus-autocomplete
  ~~~
- esbuild使用の場合
  ~~~
  $ yarn (or npm)  add stimulus-autocomplete
  ~~~
***

## JSコントローラー編集
~~~
[app/javascript/controllers/application.js]

import { Application } from "@hotwired/stimulus"
⭐️import { Autocomplete } from 'stimulus-autocomplete'

const application = Application.start()

//コンポーネントにあるAutocompleteコントローラを使えるようにするための記述
⭐️application.register('autocomplete', Autocomplete)

// Configure Stimulus development experience
application.debug = false
window.Stimulus   = application

export { application }
~~~
⭐️マークの部分を追加
***

### 💡 なんか reactっぽい
importする感じが reactっぽい。  
=> コンポーネントをインポートするような感じでコントローラーをインポートしている！
~~~
stimulus-autocompleteを使うときの application.jsの設定は、Reactでコンポーネントを使うときのプロセスに似ている点があるダ。
Stimulusではコントローラを登録することで、特定の機能を HTML要素と結びつけることができる。
Reactではコンポーネントをインポートして使うのと同じように、
Stimulusではコントローラをインポートして applicationに登録することで、それを HTML内で使えるようにしているダナ。
両方とも再利用可能な UI部品を管理しやすくするための仕組みカ。
~~~
***

# 使い方
## 自動補完部分のビューを作る
[![Image from Gyazo](https://i.gyazo.com/ccddd3b01d46205a049f05b7ee8cc977.png)](https://gyazo.com/ccddd3b01d46205a049f05b7ee8cc977)

app/views/items/search_name.html.erbファイルを追加。
~~~
[app/views/items/search_name.html.erb]

<% @items.each do |item| %>
  <li class="ms-2 my-2 small" 💚role="option" 💙data-autocomplete-value="<%= item.item_name %>" 🧡data-autocomplete-label="<%= item.item_name %>">
    <%= item.item_name %>
  </li>
<% end %>
~~~
💚💙🧡の属性たちを追加する。 

⚠️ フォームに入力があった時に、ルーティングがこのファイルを表示する(items#search_nameアクションをする)ので、  
gem ransackなどで searchアクションなど作ってる場合は、オートコンプリート用のファイル(アクション)と判別つく名前つける。
***

#### 💚
role属性は、要素がどのような役割を果たすかを示すもの。  
ここでは`option`と指定されており、  
この`<li>`要素がオートコンプリート機能において選択肢（オプション）を表していることを示す。
***

#### 💙 
リクエストに送られるデーターの内容を指定している。  
[![Image from Gyazo](https://i.gyazo.com/638b9225aa417512bef1a25d318dcc02.png)](https://gyazo.com/638b9225aa417512bef1a25d318dcc02)  
***

#### 🧡
表示用のラベル（表示テキスト）を指定している。
*** 

## 検索フォームとオートコンプリート部分のビューを紐付ける
1. 自動補完ビューの対象フォームを  
`<div data-controller="autocomplete" data-autocomplete-url-value="/cats/search" role="combobox"></div>`で囲む。
2. フォームタグ内に`data: { autocomplete_target: 'input' }`属性追加する。
~~~
[app/views/items/_search_form.html.erb]

<%= search_form_for q, url: items_path, html: {class: 'd-flex mt-3'} do |f| %>
  <div class="col me-2">
    <div 🩷data-controller="autocomplete" 💜data-autocomplete-url-value="/items/search_name" 🩵role="combobox" class="position-relative">
      <%= f.search_field :item_name_cont, 🤍data: { autocomplete_target: 'input' }, class: 'form-control', placeholder: Item.human_attribute_name('item_name') %>
      💡<ul class="autocomplete-item position-absolute d-flex flex-wrap list-unstyled bg-white w-100" style="z-index: 1000;" data-autocomplete-target="results"></ul>
    </div>
  </div>
~~~
💡 ここの ulタグはこのオートコンプリート表示場所のため。

[![Image from Gyazo](https://i.gyazo.com/e63ca5b27c7ad563e60038d808790d75.png)](https://gyazo.com/e63ca5b27c7ad563e60038d808790d75)
***

#### 🩷
data-controller属性は、Stimulus.jsという JavaScriptフレームワークのためのもので、  
この属性により、この要素に autocompleteという名前の Stimulus.jsのコントローラーが関連付けられる。
***

#### 💜
オートコンプリート機能において補完候補を取得するための URLを指定している。  
ここに、先ほど作った自動補完ビューのURL入れる。
***

#### 🩵
role属性は、この要素がどのような役割を果たすかを示すもの。  
ここでは`combobox`と指定されており、この要素がコンボボックスとして機能することを示す。    
コンボボックスは通常、ユーザーに対して選択肢を提供し、選択させるための UIパターン。
****

#### 🤍
autocomplete_targetが `input` に設定されている場合、  
通常はオートコンプリート機能を実装する JavaScriptコードがこの入力フィールドを対象として補完候補を制御する。
***

## アクション・ルーティング設定
`search_name`アクションをコントローラーに追加する。
~~~
[app/controllers/item_controller.rb]

class ItemsController < ApplicationController

  def search_name
    @items = Item.where("item_name like ?", "%#{params[:q]}%")
    respond_to do |format|
      format.js
    end
  end
~~~
***

routingも設定する。
~~~
[config/routes.rb]

resources :items do
  collection do
    get :search_name
  end
end
~~~
***

# 💡 複数箇所でオートコンプリートしたい場合
[![Image from Gyazo](https://i.gyazo.com/b4aea4006f06fe4365b6ef89a1fa14cd.png)](https://gyazo.com/b4aea4006f06fe4365b6ef89a1fa14cd)

## 自動補完ビューをそれぞれ作る。
~~~
[app/views/items/search_name.html.erb]

<% @items.each do |item| %>
  <li class="ms-2 my-2 small" 💚role="option" 💙data-autocomplete-value="<%= item.item_name %>" 🧡data-autocomplete-label="<%= item.item_name %>">
    <%= item.item_name %>
  </li>
<% end %>
~~~
~~~
[app/views/items/search_tag.html.erb]

<% @tags.each do |tag| %>
  <li class="ms-2 my-2 small" 💚role="option" 💙data-autocomplete-value="<%= tag.tag_name %>" 🧡data-autocomplete-label="<%= tag.tag_name %>">
    <%= tag.tag_name %>
  </li>
<% end %>
~~~
***

## フォームの編集
~~~
[app/views/items/_search_form.html.erb]

<%= search_form_for q, url: items_path, html: {class: 'd-flex mt-3'} do |f| %>
  <div class="col me-2">
    <div data-controller="autocomplete" ⭐️data-autocomplete-url-value="/items/search_name" role="combobox" class="position-relative">
      <%= f.search_field :item_name_cont, data: { autocomplete_target: 'input' }, class: 'form-control', placeholder: Item.human_attribute_name('item_name') %>
      💡<ul class="autocomplete-item position-absolute d-flex flex-wrap list-unstyled bg-white w-100" style="z-index: 1000;" data-autocomplete-target="results"></ul>
    </div>
  </div>

  <div class="col me-2">
    <div data-controller="autocomplete" ⭐️data-autocomplete-url-value="/items/search_tag" role="combobox" class="position-relative">
      <%= f.search_field :tags_tag_name_cont, data: { autocomplete_target: 'input' }, class: 'form-control', placeholder: Tag.human_attribute_name('tag_name') %>
      <ul class="autocomplete-item position-absolute d-flex flex-wrap list-unstyled bg-white w-100" style="z-index: 1000;" data-autocomplete-target="results"></ul>
    </div>
  </div>
~~~
⭐️ 補完候補を取得するための URLはそれぞれのビューへの URLにする。
***

## コントローラー・ルーティングの編集
~~~
[app/controllers/item_controller.rb]

class ItemsController < ApplicationController

  def search_name
    @items = Item.where("item_name like ?", "%#{params[:q]}%")
    respond_to do |format|
      format.js
    end
  end

  def search_tag
    @tags = Tag.where("tag_name like ?", "%#{params[:q]}%")
    respond_to do |format|
      format.js
    end
  end
~~~
~~~
[config/routes.rb]

resources :items do
  collection do
    get :search_name
    get :search_tag
  end
end
~~~
***
