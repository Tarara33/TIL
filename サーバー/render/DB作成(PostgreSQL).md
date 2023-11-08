# DB作成 (PostgreSQL)
マイページから　Newの部分を押して「PostgreSQL」を選ぶ。

[![Image from Gyazo](https://i.gyazo.com/68784126bda451e40ea105f32bfa5a16.png)](https://gyazo.com/68784126bda451e40ea105f32bfa5a16)

Nameと Database名をする。  
Nameはダッシュボードに表示される名前で、後からも変更可能。    
Database名は後から変更ができないので入力間違いに注意が必要だが、もちろん削除して作り直すこともできる。    
なお、Database名はすでに作成された名前は使えないので、同じものは作成できない。    
もし良い名前が思い付かない場合は、Nameをもとに自動でランダム作成されるので空欄でも大丈夫。  
同じく User名も空欄の場合、Database名をもとに自動で生成される。  

プランはフリープランで OK。  
⚠️ FREEプランは有効期限90日となる。
***

# Webサービスに紐づける
作った DBの infoの下の方にこのような欄がある。

[![Image from Gyazo](https://i.gyazo.com/f9dd0acf5edb186476d16969a46373ab.png)](https://gyazo.com/f9dd0acf5edb186476d16969a46373ab)

これの「Internal Database URL」をコピーして、  
Webサービスの方の左メニュー Environmentの Environment Variablesに   
Key「DATABASE_URL」に先ほどのコピーしたのを貼る。

[![Image from Gyazo](https://i.gyazo.com/894e4cf2f8131e6427b1d8bb2f220ced.png)](https://gyazo.com/894e4cf2f8131e6427b1d8bb2f220ced)
***
