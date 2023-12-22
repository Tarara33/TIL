# Twitterでの共有 (gem不要)
[参考](https://subaru-hello.hateblo.jp/entry/2021/09/02/224859)  

文章のみなら、gem不要

[![Image from Gyazo](https://i.gyazo.com/a847d9fac255fdebf8b1742af33cefcb.png)](https://gyazo.com/a847d9fac255fdebf8b1742af33cefcb)
***

# やり方
viewファイルに Twitterへのリンクを作る。
~~~
[Viewファイル]

<%= link_to "https://twitter.com/share?url=#{ 🩵request.url }&text=【%20#{item.user.name}さんの欲しいものが投稿されました！】
      %0a%0a#{ item.item_name }、が欲しいなぁ〜🎁
      %0a%23プレゼント探し%20%23ギフトコンパス%20%23欲しいもの",
      target: '_blank' do %>
      <i class="bi bi-twitter"></i>アピールする!!
<% end %>
~~~
`"https://twitter.com/share?url=#{ request.url }`はまるパクリで OK

🩵 request.urlとすると、シェアしたページのURLが貼られる。  
もし、topページなどに飛ばしたい場合は`root_url`とすると、共有を見てページへ飛んだユーザーは topページへいく。  

【%記号の意味】
- %20...半角スペース
- %23...ハッシュタグ
- %0a...改行
***

# 画像など入れる場合 (gem必要)
このように画像をつける場合は gem必要となる。

[![Image from Gyazo](https://i.gyazo.com/e0618afc01a0192a85f6211057d26b00.png)](https://gyazo.com/e0618afc01a0192a85f6211057d26b00)
***

# やり方
app/helpers/application_helper.rbを編集
~~~
[app/helpers/application_helper.rb]
def default_meta_tags
    {
      site: 'Gift Compass',
      reverse: true,
      charset: 'utf-8',
      description: 'Gift Compassはあなたのギフト選びをサポートします。',
      keywords: 'Gift, Compass, ギフト, プレゼント, 欲しいもの',
      canonical: request.original_url,
      separator: '☆',
      icon: [
        { href: image_url('app_icon.png') }
      ],
     ⭐️ # Twitter Card
      twitter: {
        card: 'summary',
        title: 'Gift Compass',
        description: 'Gift Compassはあなたのギフト選びをサポートします。',
        image: image_url('summary_icon.png'),
      }
    }
  end
~~~
⭐️部分から下が Twitterの設定
***

## cardの種類
summary と summary_large_imageがある。  
どちらかを選択する。

[![Image from Gyazo](https://i.gyazo.com/4984bc1364ebc9b667c416ca1e271090.png)](https://gyazo.com/4984bc1364ebc9b667c416ca1e271090)
***

## title と description
これは cardの中に入れたい文面を入れる。
***

## image
app/assets/images配下に画像を置いて、パスを入れる。
***

# シェアで反映される確認する
[バリデーター]([https://gift-compass.onrender.com/](https://cards-dev.twitter.com/validator))で反映されるかチェックできる。    
左の URLに cardに表示される URLを入れる。  
元々は実際にプレビューが見れたようだが、現在は見れない。    
しかし、右下の Logが通っていれば実際に共有時に反映されるはず。
***
