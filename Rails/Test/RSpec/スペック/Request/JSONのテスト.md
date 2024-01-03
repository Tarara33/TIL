# JSONのテスト
リクエストが JSON形式で返ってきてるかを確認するテスト。
~~~
RSpec.describe 'Api::V1::Articles', type: :request do
  describe 'GET /articles' do
    let(:article_num) { 10 }

    before do
      user = create(:user)
      create_list(:article, article_num, user: user)
    end

    it 'returns fincle articles in json format' do
      🩵get api_v1_articles_path, headers: { CONTENT_TYPE: 'application/json', ACCEPT: 'application/json', }

      💚expect(JSON.parse(body)['data'].count).to eq(article_num)
      expect(response).to be_successful
      expect(response).to have_http_status(:ok)
    end
  end
end
~~~
***

### 🩵
ヘッダーに CONTENT_TYPEと ACCEPTを application/jsonとして設定しているのは、  
リクエストが JSON形式であり、レスポンスも JSONを期待していることを指定している。

検証機能のレスポンスヘッダーに CONTENT_TYPE  
リクエストヘッダー側に ACCEPTがある。

ポストマンを使って確認すると、CONTENT_TYPEが application/jsonだった。
[![Image from Gyazo](https://i.gyazo.com/a72351590a279547f510ac7107337210.png)](https://gyazo.com/a72351590a279547f510ac7107337210)

リクエストヘッダー側の ACCEPTでは application/json確認できなかった...。
***

### 💚
レスポンスボディから JSONをパースして得られた'data'配列の要素数が、article_num変数の値と等しいかをチェックしている。  
つまり、期待される記事の数が実際に返ってきているかを確認している。
***
