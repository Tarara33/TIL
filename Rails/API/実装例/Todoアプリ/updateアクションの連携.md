# updateアクションの連携
【バックエンド側】  
APIコントローラーで updateアクションを実装
~~~
[app/controllers/todo_controller.rb]

class TodosController < ApplicationController
  before_action :set_todo, only: %i[ show update destroy ]

# PATCH/PUT /todos/1
  def update
    if @todo.update(todo_params)
      render json: @todo
    else
      render json: @todo.errors, status: :unprocessable_entity
    end
  end
~~~
***

# 方法
## ① Todoデータを編集する関数の作成
【フロント側】  
⚠️ [indexアクション](https://github.com/Tarara33/TIL/blob/main/Rails/API/%E5%AE%9F%E8%A3%85%E4%BE%8B/Todo%E3%82%A2%E3%83%97%E3%83%AA/index%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E9%80%A3%E6%90%BA.md)の方により細かく書いてる。
~~~
[src/api.ts]

export const updateTodo = async (💛todoId: number, todoData: any) => {
  try {
    const response = await axios.put(`${API_BASE_URL}/todos/💛${todoId}`, todoData)
    return response.data;
  } catch (error) {
    console.error('Error while updating todo:', error);
    throw error;
  }
};
~~~
💛 URLに ID必要なので、引数に ID入れてる。
***

## ② 作成した関数を使ってフォーム作る
【フロント側】    
⚠️ [createアクション](https://github.com/Tarara33/TIL/blob/main/Rails/API/%E5%AE%9F%E8%A3%85%E4%BE%8B/Todo%E3%82%A2%E3%83%97%E3%83%AA/create%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E9%80%A3%E6%90%BA.md)の方にも詳しく書いてる。
~~~
[src/components/TodoEditForm.tsx]

import React, { useState } from 'react';
⭐️ import { updateTodo } from '../api';

🚀// todoオブジェクトの型定義
  type Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

🚀// propsの型定義
type TodoEditFormProps = {
  todo: Todo;
  🧡onEdit: () => void;
};

const TodoEditForm: React.FC<TodoEditFormProps> = ({ todo, onEdit }) => {
  const id = todo.id
  const [title, setTitle] = useState(todo.title);
  const [description, setDescription] = useState(todo.description);

  const handleSubmit = async (e: React.FormEvent) => {
    try {
      const todoData = {
        title,
        description,
      };

      await updateTodo(id, todoData);
      onEdit();
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <form className="edit-form-container" onSubmit={handleSubmit}>
      <h3>ToDo 編集</h3>
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
          <textarea
            value={description}
            onChange={(e) => setDescription(e.target.value)}
          />
        </label>
      </div>
      <button type="submit">Update</button>
    </form>
  );
}

export default TodoEditForm;
~~~
⭐️ 先ほど作った関数コンポーネントをインポートする。

🚀 TypeScript使ってるので[型定義](https://github.com/Tarara33/TIL/blob/main/React/TypeScript%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F%20react.md)する。

🧡 onEditは関数型であり、  
- ()...空の引数を受け取る関数であることを示している。
- void...関数が何も返さない（void）ことを示している。
***
