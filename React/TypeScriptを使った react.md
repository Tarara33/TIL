# TypeScriptを併用の react
JavaScriptを使った時と少し違う点があった。  
💡 `〇〇.tsx`という拡張子なら TypeScriptファイル

## ⭐️ こんな時に型定義が必要
TypeScriptを使うと、データの形や関数の引数、戻り値などに対して型を定義して、コードが期待するデータの構造を明確にする。

- 🌞 コンポーネントの propsを定義する時
コンポーネントに渡される propsの型を定義することで、期待されるデータ型がコンポーネントに渡されているかチェックできる。

- 🌝 関数やメソッドを定義する時
引数と戻り値に型を定義することで、関数の使い方を明確にし、誤った使い方を防げる。

- 🚀 オブジェクトや変数の型を定義する時
オブジェクトや変数の形を定義することで、コード内で正しく使われているかチェックできる。
***

# 例: 🌞 propsに型定義
propsを送る側は変化なし。
~~~
<TodoList todos={todos}/>
~~~

受け取る側(子コンポーネントが少し変更点あった)
~~~
// todoの型定義
type 💛Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

🩵type TodoListProps = {
  todos: Todo[];
};

const TodoList: 🩷React.FC<TodoListProps> = ({ todos }) => {
~~~
💛 オブジェクトの型定義

🩵 propsの型定義

🩷 `React.FC`は `Functional Component` の略で、
これを使うことでコンポーネントが Propsを受け取ることが期待されることがわかる。  
`<>`を使うことでデータ型を指定してる。([詳しくはここ](https://github.com/Tarara33/TIL/blob/main/React/React%20ver18/%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88/%E3%83%95%E3%83%83%E3%82%AF/useState/%E3%83%A1%E3%83%A2/%E5%88%9D%E6%9C%9F%E5%80%A4%E3%81%AE%E8%A8%AD%E5%AE%9A.md#%E3%83%87%E3%83%BC%E3%82%BF%E5%9E%8B%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B))
***

# 例: 🚀 useStateにも型定義
~~~
// todoの型定義
type Todo = {
  id: number;
  title: string;
  description: string;
  completed: boolean;
};

const  App = () => {

  // Todoリストの初期値を空の配列に設定
  const [todos, setTodos] = useState💚<Todo[]>([]);
~~~
💚 `<>`を使うことでデータ型を指定してる。([詳しくはここ](https://github.com/Tarara33/TIL/blob/main/React/React%20ver18/%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88/%E3%83%95%E3%83%83%E3%82%AF/useState/%E3%83%A1%E3%83%A2/%E5%88%9D%E6%9C%9F%E5%80%A4%E3%81%AE%E8%A8%AD%E5%AE%9A.md#%E3%83%87%E3%83%BC%E3%82%BF%E5%9E%8B%E3%82%92%E6%8C%87%E5%AE%9A%E3%81%99%E3%82%8B))
***

# 🌝 関数に型定義
~~~
  const handleSubmit = async 💜(e: React.FormEvent) => {
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
~~~
フォームイベントだよという型定義している。
***
