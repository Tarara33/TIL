# ❓ イベント発生時(event)引数はいつ必要？？

todoアプリでこのようなフォームとボタンを実装した時に、引数(event)をつけるときと付けない時があったので違いを調べた。

[![Image from Gyazo](https://i.gyazo.com/362ee6fa592b2f867dd13dd41991fd9e.png)](https://gyazo.com/362ee6fa592b2f867dd13dd41991fd9e)

~~~

const onChangeTodoText = ⭐️(event) => setTodoText(event.target.value);

const onClickAdd = ⭐️() => {
  if (todoText === "") return;
  const newTodos = [...incompleteTodos, todoText];
  setIncompleteTodos(newTodos);
  setTodoText("");
};

return (
  <>
    <input placeholder="TODOを入力" value={todoText} onChange={onChangeTodoText} />
    <button onClick={onClickAdd}>追加</button>
  </>
);
~~~
onChangeイベントの onChangeTodoTextでは発動時に引数(event)があり、  
onClickイベントの　onClickAddではない。  

違いは何か？？
***

# 💡 イベントオブジェクトを使うかどうか
onChangeTodoText関数では、ユーザーが入力した値を取得している。  
なので (event)引数を使ってイベントオブジェクトにアクセスする必要があるので使っている。  

一方、onClickAdd関数では、  
ただ、ボタンをクリックした時に todoText の値を使って新しいTodoを追加する処理を実行しているだけ。  
なので、イベントオブジェクトにアクセスする必要がないので (event)引数使ってない。
***

### イベントオブジェクト
[![Image from Gyazo](https://i.gyazo.com/e22aa364c7714312105df907c3ff3c23.png)](https://gyazo.com/e22aa364c7714312105df907c3ff3c23)
***
