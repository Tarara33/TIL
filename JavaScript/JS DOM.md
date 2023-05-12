# document
DOM は document という特殊なオブジェクトで扱うことができる

# querySelector()
文書内から特定の要素を取得する  
querySelector("セレクター名") h1や#id名など
***

# .textContent
中身のテキストを表現する    
`document.querySelector("h1").textContent = "変更内容"`   
でh1のテキスト内容が変わる
***

# getElementById() 
idを指定して要素を探す
getElementById(id名) 　⚠️#いらない
