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
