# Rakeタスクの rspec
[参考](https://qiita.com/aeroastro/items/c97bd26ce8b8818b6bed)
「spec/lib/tasks」配下に作る。(手動)
***

## rake_helper.rbを用意
~~~
①require 'rails_helper'
②require 'rake'

RSpec.configure do |config|
  ③config.before(:suite) do
    ④Rails.application.load_tasks # Load all the tasks just as Rails does (`load 'Rakefile'` is another simple way)
  end

  ⑤config.before(:each) do
    ⑥Rake.application.tasks.each(&:reenable) # Remove persistency between examples
  end
end
~~~
① rspec全体の設定である rails_helperを読み込む。    
    
② Rakeタスクを操作するのに必要な Rakeライブラリを読み込む。  
  
③ テストスイート(:suite) を実行する前に行う処理を書く。  
    
④ Railsアプリケーションのタスクをすべて読み込みこむ。    
 　 これにより、テスト内で Rakeタスクを実行できるようになる。  
  
⑤ 各テストケース(:each) が実行される前に行う処理を書く。    
  
⑥ 各テストケース間でRakeタスクが共有されないようにするため、タスクをリセット（reenable）する。
***

# テストを書く
テストするタスクは、「１時間毎に記事の設定日時が現在時刻より過去だったら記事を公開する」というもの。
~~~
[lib/tasks/article_status.rake]

namespace :article_status do
  desc "「公開待ち」の中で、公開日時が過去になっているものがあれば、ステータスを「公開」に変更する"
  task change_to_be_published: :environment do
    Article.publish_wait.past_publish.find_each(&:published!)
  end
end
~~~
~~~
[spec/lib/tasks/article_status_spec.rb]

require 'rake_helper'
=> rakeの方を読み込む！

describe 'article_status:change_to_be_published' do
  ①subject(:task) { Rake.application['article_status:change_to_be_published'] }
  before do
    create(:article, state: :publish_wait, published_at: Time.current - 1.day)
    create(:article, state: :publish_wait, published_at: Time.current + 1.day)
    create(:article, state: :draft)
  end
 
  it 'change_to_be_published' do
    ②expect { task.invoke }.to change { Article.published.size }.from(0).to(1)
  end
end
~~~
① テスト内でRakeタスクを操作するための変数 task を作成する。    
  　Rake.application を介して article_status:change_to_be_published という名前の Rakeタスクを取得している。    
  　この task 変数を使用して、後でこのタスクを呼び出し、その実行結果をテストすることができる。  
     
  
② ⭐️invoke は、Rakeタスクを明示的に実行するためのメソッド。  
  　つまり、taskを実行したら掲示板の公開ステータスの記事数が0から1に変わることを期待している。  
  　[changeマッチャーはこちら](https://github.com/Tarara33/TIL/blob/main/Rails/Test/RSpec/%E3%83%9E%E3%83%83%E3%83%81%E3%83%A3%E3%83%BC/%E9%AB%98%E5%BA%A6%E3%81%AA%E3%83%9E%E3%83%83%E3%83%81%E3%83%A3%E3%83%BC.md)
   ***
