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

## 
