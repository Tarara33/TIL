# createアクションの連携
【バックエンド側】
APIコントローラーで createアクションを実装
~~~
[app/controllers/todo_controller.rb]

class TodosController < ApplicationController
  before_action :set_todo, only: %i[ show update destroy ]

# POST /todos
  def create
    @todo = Todo.new(todo_params)

    if @todo.save
      render json: @todo, status: :created, location: @todo
    else
      render json: @todo.errors, status: :unprocessable_entity
    end
  end
~~~
***

# 方法
① Todoデータを作成する関数の作成
【フロント側】
⚠️ [indexアクション](https://github.com/Tarara33/TIL/blob/main/Rails/API/%E5%AE%9F%E8%A3%85%E4%BE%8B/Todo%E3%82%A2%E3%83%97%E3%83%AA/index%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E9%80%A3%E6%90%BA.md)の方により細かく書いてる。
~~~
[src/api.ts]

export const createTodo = async 🤍(todoData: any) => {
  try {
    const response = await 🩷axios.post(`${API_BASE_URL}/todos`, todoData);
    return response.data;
  } catch (error) {
    console.error('Error while creating todo:', error);
    throw error;
  }
};
~~~
🤍 この関数の引数 todoDataに対して TypeScriptの型注釈を行っている。  
anyは TypeScriptで型が不明確であることを表すもの。  
ToDoの中身は数値で来たり、文字列で来たりわからないから anyにしている。

🩷 [axios](https://github.com/Tarara33/TIL/blob/main/JavaScript/API/axios.md#axiosposturl)のメソッド。  
第二引数に送るデーター入れる。
***

## ② 作成した関数を使ってフォーム作る
【フロント側】
~~~
[src/components/TodoForm.tsx]

import { useState } from "react";
⭐️ import { createTodo } from "../api";

const TodoForm = () => {
  const [title, setTitle] = useState('');
  const [description, setDescription] = useState('');

  const handleSubmit = async 💛(e: React.FormEvent) => {
    try {
      await createTodo ({
        title,
        description
      });

      // 作成成功時の処理
      setTitle('');
      setDescription('');
    } catch (error) {
      console.log(error);
    }
  };

  return (
    🩵<form className="form-container" onSubmit={handleSubmit}>
      <h2>Todo 作成</h2>
      <div>
        <label>
          タイトル:
          <input
            type="text"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            />
        </label>
      </div>

      <div>
        <label>
          内容:
          <input
            type="text"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            />
        </label>
      </div>
      <button type="submit">作成！</button>
    </form>
  )
}

export default TodoForm;
~~~
⭐️ 先ほど作った関数コンポーネントをインポートする。

💛 これは Reactが提供する型で、フォームに関連するイベントに特化した型。  
`e`はイベント(handleSubmit)のことで、このイベントはフォームに関連するイベントだよということを表している。

🩵 フォームタグに onSubmitボタンつけると、送信してくれる。(buttonタグにつけなくていい！)  
フォーム系のこと詳しくは[こちら](https://github.com/Tarara33/TIL/tree/main/React/React%20ver18/%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88/JSX/%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0)
***

## ③ 作ったフォームコンポーネントを呼ぶ
【フロント側】
~~~
[src/App.tsx]

⭕️import TodoForm from './components/TodoForm';

...index編に書いてあるので省略

  return (
    <div className="container">
      <h1>ToDo List</h1>
      ⭕️<TodoForm />
      <ul>
        {todos.map((todo: any) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
    </div>
  );
};
~~~
***
