# rails db:reset　と　rails db:migrate:reset
効力は「db:reset < db:migrate:reset」という感じ。    

- rails db:reset    
DBをdrop(削除)して、現在のschemeファイルをロードしてDBを作り直す。   
このコマンドは「rails db:drop db:setup」と同じ。    
さらにシードファイルも読み込んでダミーデータも作ってくれる。    
新しくテーブルを作り直すので、既存のデータは全て消えて新たにシードファイルを元に作り直す。
***

- rails db:migrate:reset    
DBをdrop(削除)した後、通常通りのマイグレート（db:migrate）が行われる。    
つまり、db/migrate/**.rb が古い順から全て実行される。   
シードファイルは読み込まないので、新しいダミーデータは生成されない。    
（$ raild db:seedは自分で実行する必要がある）
***

## どちらを使うか
schemeファイルに変更を全て取り入れてるなら`$ rails db:reset`  
rails db:migrateしてから、rails db:rollbackしないでファイルを編集して  
それを適応したいなら`$ rails db:migrate:reset`
***
