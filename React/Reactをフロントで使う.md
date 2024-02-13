# axiosインストールする
`$ npm install axios`でインストールして、  
src/配下に `api.js`ファイルを作成して、Webサービスに接続してデータを取得したり、新しいデータを作成したりする方法を実装する。

例として Todoアプリでバックエンドを railsにして、データーを持ってくるときはそれぞれのアクションで何をするかコード書く。
~~~
[api.js]

import axios from 'axios';

const API_BASE_URL = 'http://localhost:3000';

export const getTodos = async () => {
  try {
    const response = await axios.get(`${API_BASE_URL}/todos`);
    return response.data;
  } catch (error) {
    console.error('Error while fetching todos:', error);
    throw error;
  }
};

export const createTodo = async (todoData: any) => {
  try {
    const response = await axios.post(`${API_BASE_URL}/todos`, todoData);
    return response.data;
  } catch (error) {
    console.error('Error while creating todo:', error);
    throw error;
  }
};

export const updateTodo = async (todoId: number, todoData: any) => {
  try {
    const response = await axios.put(`${API_BASE_URL}/todos/${todoId}`, todoData);
    return response.data;
  } catch (error) {
    console.error('Error while updating todo:', error);
    throw error;
  }
};

export const deleteTodo = async (todoId: number) => {
  try {
    const response = await axios.delete(`${API_BASE_URL}/todos/${todoId}`);
    return response.data;
  } catch (error) {
    console.error('Error while deleting todo:', error);
    throw error;
  }
};
~~~
***
