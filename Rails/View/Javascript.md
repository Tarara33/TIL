[サブミットボタンについて](https://railsdoc.com/page/submit_tag)
# 確認ダイアログを出す
[![Image from Gyazo](https://i.gyazo.com/0842a62b75b0a406bb8b14d606c276a5.png)](https://gyazo.com/0842a62b75b0a406bb8b14d606c276a5)
~~~
= link_to "アカウント削除", user_registration_path, method: :delete, data: { confirm: "【確認】アカウントを削除してもよろしいですか？" }

[文章を改行したい時]
= link_to "アカウント削除", user_registration_path, method: :delete, data: { confirm: "【確認】\nアカウントを削除してもよろしいですか？" }
=> \nで改行する
~~~
***

# 送信ボタンの無効化（連続で押させたくない時）
 ~~~
<%= f.submit "送信する", data: { disable_with: "送信中..." } %>
~~~
⚠️form_remort_tagの場合コツが必要らしい。[コツ](https://zariganitosh.hatenablog.jp/entry/20070623/1182551690)
***

## デフォルト値
data-disable-withにはデフォルトでvalue属性が使われる。    
[ココ](https://api.rubyonrails.org/v5.1.7/classes/ActionView/Helpers/FormTagHelper.html#method-i-submit_tag)に、いくつか例が載っている。    
「=>」で書かれているのは、出力されるHTML(検証で見れるやつ)      
⭐️ value属性とは異なった文字を表示したい場合は、「 data: { disable_with: '表示したい文字'} 」とする。
***



