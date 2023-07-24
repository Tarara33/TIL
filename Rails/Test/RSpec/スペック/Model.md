# モデルスペック
モデルのメソッドやバリデーションなど、モデルの内部の動きをテストするためのもの。    
コンソールで実行した動きと同じようなことをテストすることが多い。
***

# 準備
⭐️ データー作るので gem 'factory_bot'必要。    
インストール後にターミナルでコレ叩くと、Factoryファイルも一緒に作られる。
~~~
$ rails g rspec:model モデル名(単数系)
~~~
***

# データー作る
[詳しくはこちら](https://github.com/Tarara33/TIL/blob/main/Rails/Test/Factory.md)
　***

# テストかく
今回は Taskモデルのバリデーションがされてるか・エラーメッセージの内容      
をテストしている。
~~~
[spec/models/task_spec.rb]

⭐️ require 'rails_helper'

RSpec.describe Task, type: :model do
  describe 'validation' do
    it 'すべての属性がある場合、有効である' do
      task = create(:task)
      expect(task).to be_valid　　　　　　　　　　　　　　　　　　　　　　　　　　#バリデーションかかるかの確認（ここはかからないのが正解なので、be_valid)
      expect(task.errors).to be_empty      #エラーメッセージの確認(ここはエラーメッセージないはずなので、be_empty)
    end
  
    it 'タイトルがない場合、無効である' do
      task_without_title = build(:task, title: nil)
      expect(task_without_title).to be_invalid
　　　　　　　 ⭕️expect(task_without_title.errors[:title]).to eq ["can't be blank"]
    end

    it 'ステータスがない場合、無効である' do
      task_without_status = build(:task, status: nil)
      expect(task_without_status).to be_invalid
      expect(task_without_status.errors[:status]).to eq ["can't be blank"]
    end

    it 'タイトルが重複している場合、無効である' do
      task = create(:task)
      task_with_duplicated_title = build(:task, title:task.title)
      expect(task_with_duplicated_title).to be_invalid
      expect(task_with_duplicated_title.errors[:title]).to eq ["has already been taken"]
    end

    it 'タイトルが別の場合、有効である' do
      task = create(:task)
      task_with_another_title = create(:task, title: "another_title")
      expect(task_with_another_title).to be_valid
      expect(task_with_another_title.errors).to be_empty
    end
  end
end
~~~
⭐️ rails_helper読み込ませるの忘れないように！
    
⭕️エラーメッセージの書き方
