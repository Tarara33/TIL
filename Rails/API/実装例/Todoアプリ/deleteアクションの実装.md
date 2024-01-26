# deleteアクションの実装
【バックエンド側】  
APIコントローラーで deleteアクションを実装
~~~
[app/controllers/todo_controller.rb]

class TodosController < ApplicationController
  before_action :set_todo, only: %i[ show update destroy ]

  # DELETE /todos/1
  def destroy
    @todo.destroy
  end
~~~
***

# 方法
## ① Todoデータを削除する関数の作成
【フロント側】
~~~
[src/api.ts]

export const deleteTodo = async 💛(todoId: number) => {
  try {
    const response = await axios.delete(`${API_BASE_URL}/todos/${todoId}`);
    return response.data;
  } catch (error) {
    console.error('Error while deleting todo:', error);
    throw error;
  }
};
~~~
💛 URLに ID必要なので、引数に ID入れてる。
***

## ② 削除ボタンの実装
【フロント側】
~~~
[src/components/TodoItem.tsx]

import React, { useState } from 'react';
import TodoEditForm from './TodoEditForm';
⭐️import { deleteTodo } from '../api';

type Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

type TodoItemProps = {
  todo: Todo;
};

const TodoItem: React.FC<TodoItemProps> = ({ todo }) => {
  const [isEditing, setIsEditing] = useState(false);

  const handleEditClick = () => {
    setIsEditing(true);
  };

  const handleEditCancel = () => {
    setIsEditing(false);
  };

  🩵const handleDeleteClick = async () => {
    try {
      await deleteTodo(todo.id);
    } catch (error) {
      console.error(error);
    }
  };


  return (
    <div>
      {!isEditing ? (
        <div>
          <h3>{todo.title}</h3>
          <p>{todo.description}</p>
          <button onClick={handleEditClick}>Edit</button>
          🩵<button onClick={handleDeleteClick}>Delete</button>
        </div>
      ) : (
        <TodoEditForm todo={todo} onEdit={handleEditCancel} />
      )}
    </div>
  );
};

export default TodoItem;
~~~
⭐️ 先ほど作った関数コンポーネントをインポートする。

🩵 編集モードではない時に削除ボタンを実装
***

# ⚠️ ToDo削除後のリスト更新処理
現状だと ToDoを削除しても、リストには削除された ToDoが表示されたままになっている。  
そのため、ToDoを削除した後に、削除された ToDoを除いた ToDoリストを表示するようにする。
~~~
[src/App.tsx]

import { useEffect, useState } from 'react';
import './App.css';
import { getTodos } from './api';
import TodoForm from './components/TodoForm';
import TodoList from './components/TodoList';

// todoの型定義
type Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

const  App = () => {

  // Todoリストの初期値を空の配列に設定
  const [todos, setTodos] = useState<Todo[]>([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const todosData = await getTodos();
        setTodos(todosData);
      } catch (error) {
        console.error('Error while fetching todos:', error);
      }
    };

    fetchTodos();
  }, []);

  🧡// Todo削除後、削除されたTodoを除いたTodoリストを表示する関数
  const handleTodoDelete = async (todoId: number) => {
    try {
      setTodos(💚(prevTodos) => prevTodos.filter((todo) => todo.id !== todoId));
    } catch (error) {
      console.error(error);
    }
  };


  return (
    <div className="container">
      <h1>ToDo List</h1>
      <TodoForm />
      <TodoList todos={todos} 🧡onTodoDelete={handleTodoDelete}/>
    </div>
  );
};

export default App;
~~~
🧡 Todo削除後に表示する Todoリストを更新する。  
propsでリスト更新メソッドを渡している。

💚 prevTodosには現状の　todosが入る。  
それを　filterメソッドで削除した todo以外を再度リストにしてる。
***

## ③ Todoアイテム側の再編集
~~~
[src/components/TodoItem.tsx]

import React, { useState } from 'react';
import TodoEditForm from './TodoEditForm';
import { deleteTodo } from '../api';

type Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

type TodoItemProps = {
  todo: Todo;
  💜onDelete: (todoId: number) => void;
};

const TodoItem: React.FC<TodoItemProps> = ({ todo, 💜onDelete }) => {
  const [isEditing, setIsEditing] = useState(false);

  const handleEditClick = () => {
    setIsEditing(true);
  };

  const handleEditCancel = () => {
    setIsEditing(false);
  };

  const handleDeleteClick = async () => {
    try {
      await deleteTodo(todo.id);
      💜onDelete(todo.id); // 渡されてきた関数(削除後のリスト更新処理)を実行
    } catch (error) {
      console.error(error);
    }
  };


  return (
    <div>
      {!isEditing ? (
        <div>
          <h3>{todo.title}</h3>
          <p>{todo.description}</p>
          <button onClick={handleEditClick}>Edit</button>
          <button onClick={handleDeleteClick}>Delete</button>
        </div>
      ) : (
        <TodoEditForm todo={todo} onEdit={handleEditCancel} />
      )}
    </div>
  );
};

export default TodoItem;
~~~
💜 削除したタイミングで、todoの idを渡し、それを用いて削除するメソッド(handleTodoDelete)を実行する。
***
