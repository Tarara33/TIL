[rubyでのまとめ](https://github.com/Tarara33/TIL/tree/main/Ruby/Minitest)  
  
# 使い方
~~~
$ rails test
~~~

# 見やすさ設定
minitest-reporters gemの利用がオススメ。    
以下２行を追加する。
~~~
[test/test_helper.rb]
require "minitest/reporters"
Minitest::Reporters.use!
~~~
***

# 書き方見本
~~~
require "test_helper"

class UserControllerTest < ActionDispatch::IntegrationTest

  test "should get index" do
    get users_url
    assert_response :success
    assert_select "title", "Users | Ruby on Rails Tutorial Sample App"
  end
end
~~~
***

## 意味
### test "テスト名" do ~ end
一つのテストをこの中に書く。   
***

### get 〇〇_url
getリクエストを出して指定ページにいく。
***

### assert
期待する結果。
***


# assertメソッド種類
- assert_text　"〇〇"...画面上に""に記述したテキストがある。    
- assert_no_text　"〇〇"...画面上に""に記述したテキストがない。    
- assert_equal(引数１,引数２)...引数１==引数２    
- assert_response :〇〇...レスポンスのステータスが〇〇    
- assert_select "タグ名" (+ "内容")...特定のHTMLタグ(と内容)が存在するかどうか
***
