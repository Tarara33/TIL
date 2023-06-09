[サブミットボタンについて](https://railsdoc.com/page/submit_tag)
# 確認ダイアログを出す
[![Image from Gyazo](https://i.gyazo.com/2ef5559b601317fefd50aa4f8214d29b.png)](https://gyazo.com/2ef5559b601317fefd50aa4f8214d29b)
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
