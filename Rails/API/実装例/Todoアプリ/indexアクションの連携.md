# indexアクションの連携
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
## ① Todoデータを取得する関数の作成
【フロント側】  
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

## ② 作成した関数を使う
【フロント側】  
~~~
[src/App.tsx]

import { useEffect, useState } from 'react';
⭐️ import { getTodos } from './api';

🩷// todoの型定義
type Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

const  App = () => {

  // Todoリストの初期値を空の配列に設定
  const [todos, setTodos] = 💛useState<Todo[]>([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const todosData = await getTodos();
        setTodos(todosData);
      } catch (error) {
        console.error('Error while fetching todos:', error);
      }
    };

    💙fetchTodos();
  }, 💙[]);

  return (
    <div className="container">
      <h1>ToDo List</h1>
      <ul>
        {todos.map((🤍todo: any) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
    </div>
  );
};

export default App;
~~~
⭐️ 先ほど作った関数コンポーネントをインポートする。

🩷 TypeScript使うならデータ型定義しておくといい。

💛 useStateの初期値に[データ型](https://github.com/Tarara33/TIL/blob/main/React/React%20ver18/%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88/%E3%83%95%E3%83%83%E3%82%AF/useState/%E3%83%A1%E3%83%A2/%E5%88%9D%E6%9C%9F%E5%80%A4%E3%81%AE%E8%A8%AD%E5%AE%9A.md#%E3%83%87%E3%83%BC%E3%82%BF%E5%9E%8B%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B)を指定する。

💙 第二引数にただの`[]`を使用しているので、レンダリングの時のみ useEffectが呼ばれる。  
その時に、fetchTodos()を下に書くことで、初回呼び出された時にfetchTodosメソッドが発動される。  
(書いておかないと、useEffectは発動するが、fetchTodosメソッドは発動しない。)

🤍 TypeScriptにおいて、任意の型を表すキーワード。  
anyは、mapメソッドを使用する際に、todoの型が具体的には不明であることを示している。
***
