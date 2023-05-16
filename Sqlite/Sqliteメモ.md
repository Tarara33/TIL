# rollback後にもう一度migrateしたらエラー出た
1.元々のmigrateファイルに「not null制約」をつけるため、新しくmigrationしてファイル記入してから`rails db:migrate` =>成功   
2.その後「null制約」ができていることを確認して、一度rollbackして元に戻す（「null制約」適応前に戻す）　=>成功    
３.その後nullでもデータ保存がされてちゃんとrollbackされていることを確認したので再度migrateで「null制約」つける =>失敗    
原因は一度rollbackした時にnullで作ったデーターがそのままだったため。それを消したらmigrateできた
***
