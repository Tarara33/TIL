# alt
imgタグに使う画像の説明　　
何らかの理由で画像が表示されなかったときに、このテキストを表示してくれたり、　　
視覚に障害がある人が使う音声読み上げブラウザでこの説明を読み上げてくれたりする
***

# target
aタグに使うリンク先を開くときにブラウザの別のタブで開いて欲しい時に使う  
`<a href = "" target = "_blank"></a>`と値は「_blank」にする
***

# input type = ""
フォームを作る時に出てくる        
type一覧
- text...一行分のテキストBOX
- email...モバイル端末で入力フォームをタップしたときに、メールアドレスを入力しやすい特殊なキーボードが表示される
- placeholder...何を入力するのか例を薄く表示する
- checkbox...チェックBOXで複数押せる
- radio...チェックBOXみたいな感じだが一つしか押せない   
💡`<input type = "radio" name = "">`でnameの値が同じものの中から一つ選ぶ感じ
- submit...ボタン  
-  password...入力が黒丸になる
-  number...数値を入れる
-  date...日付の入力（カレンダー表示）    
numberとdateはブラウザや古いパソコンによっては動かないので注意
***

# size
selectタグでドロップリストを作った時に使う  
`<select size ="3"></select>`とするとリストが３行分表示される形になる
***

# value　と　checked　と　selected
- value...textやtextareaでの初期値の設定
- checked...checkboxやradioでの初期値の設定
- selected...ドロップリストでの初期値の設定
- ***

# diabled
inputなどに使うボタンを押せなくする  
送信ボタンなど必須項目を入力してから出ないと押せないようにしたい時に使う
***

# rel=""
href=""で指定したリンク先とどのような関係にあるのかを示すことができる
`<a href = "style.css" rel="stylesheet">`だとリンク先はスタイルシートですとなる    
- stylesheet...CSS
- next...次のページ
- prev...前のページ
- external...外部サイトへのリンク
- noopener...一種のセキュリティー対策。`target="_blank"`をつけると自動的に付与されること多い。  
動きとしては、別タブで開かれた参照先に対して参照元のリンクを渡さないようにすることができる。  
参照元リンク情報を渡さないことで、渡したくない情報（ユーザーIDなど）がURLリンクに含まれていても大丈夫になる。[参考](https://hideharublog.com/noopener-noreferrer/)  
    
など多くある
***

# action=""
フォームに入力された情報の送信先を指定する   
action属性を空にするとフォームが置かれた自身のページに送信される
***

