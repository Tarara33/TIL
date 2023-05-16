# マイグレーションのバージョンの動かし方
- `bin/rails or rails db:migrate`...最新までマイグレーションを適応する
- `bin/rails or rails db:migrate　VERSION=ファイル名の数字`...特定バージョンまでマイグレーションされた状態にする    
例：「20220505120641_create_tasks.rb」だったら「20220505120641」をVERSION=に入れる
- `bin/rails or rails db:rollback`...バージョンを１つ戻す
- `bin/rails or rails db:rollback STEP=数字`...バージョンを指定したステップ数だけ戻す
- `bin/rails or rails db:migrate:tedo`...バージョンを１つ戻してから上げる   
（バージョンは最終的に変化しないが、バージョンを戻す処理が想定通り動くか簡単に確認できる）
