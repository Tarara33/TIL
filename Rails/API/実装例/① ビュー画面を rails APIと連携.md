### 前提
- rails API new済み
- フロント側も new済み(今回は react)
- APIコントローラーで index, show, create, update, deleteアクション作成済み

# ① ビュー画面を rails APIと連携
【バックエンド側】  
APIコントローラーで indexアクションを実装
~~~
[app/controllers/todo_controller.rb]

class TodosController < ApplicationController
  before_action :set_todo, only: %i[ show update destroy ]

  # GET /todos
  def index
    @todos = Todo.all

    render json: @todos
  end
~~~
この取得した todosのデーターを表示するビューを作る。
***

# 方法
## Todoデータを取得する関数の作成
【フロント側】  
Todoデータを取得する関数の作成する
~~~
[src/api.ts]

🩵import axios from 'axios';

const API_BASE_URL = 'http://localhost:3000';

export const getTodos = ⭐️async () => {
  try {
    const response = ⭐️await 💜axios.get(`${API_BASE_URL}/todos`);
    return response.data;
  } catch (error) {
    💚console.error('Error while fetching todos:', error);
    🧡throw error;
  }
};
~~~
🩵 apiとデーターを送受信するため、 [axios](https://github.com/Tarara33/TIL/blob/main/JavaScript/API/axios.md)使用。

⭐️ [async](https://github.com/Tarara33/TIL/blob/main/JavaScript/%E5%87%A6%E7%90%86/%E9%9D%9E%E5%90%8C%E6%9C%9F%E5%87%A6%E7%90%86%E3%81%AE%E6%96%B9%E6%B3%95/Async%E3%83%BBAwait.md)関数で await部分の処理が終わるまで次の処理を待つ。

💜 axios導入で使えるメソッドで、引数の URLに HTTP通信(API通信)してサーバーからデータを取得する。

💚 console.errorはconsole.logよりも目立つようにエラーとわかる形式で出力される。

🧡 [throw](https://github.com/Tarara33/TIL/blob/main/JavaScript/JS%E4%BE%8B%E5%A4%96%E5%87%A6%E7%90%86.md#throw)についてはこちら。  
catch内に throwを書くことでそのエラーは catchブロックの外に投げられて、  
getTodosメソッドを呼び出した方にもエラーがあったと伝わる。
***

## ❓ catch内の throw??
もし catch内に throwがなかったらどうなる？？
~~~
throwがなかったら、エラーが起きてもその場で止まるだけで、getTodosを使ってる方にはエラーが起きたことが伝わらないんだ。

それは、友達が遊びに来る約束をしていて、もし友達が何かの理由で来られなくなったときに、何も連絡してこないのと似てるよ。
連絡がないと、「もしかしたらまだ来るのかな？」ってずっと待ってしまうよね。
だから、throwをすることできちんと「ごめん、来れないよ」と伝えることができるんだ。
これで、エラーが起きたことをちゃんと知らせることができるんだよ。
~~~
***

