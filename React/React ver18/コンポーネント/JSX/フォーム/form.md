# form
- formタグで囲むと、buttonタグに onClickをつけずに、 formにつけると、ボタンが押された時にイベント発火する。
- 属性 actionで指定したURLなどに飛ぶ。つけない場合は現在の画面をリロードする。
***

# 画面リロードをなくす
action属性つけない場合、リロードされるが、それを阻止したい時。
formにつけているイベント内で`preventDefault();`をつける。
~~~
export const Form = ({createTodo}) => {
  const [enteredTodo, setEnteredTodo] = useState("");

  const addTodo = (e) => {
    ⭐️e.preventDefault();

    const newTodo = {
      id: Math.floor(Math.random() * 1e5),
      content: enteredTodo
    };
    createTodo(newTodo);
    setEnteredTodo("");
  }
  return (
    <div>
      <form onSubmit={addTodo}>
        <input type="text" 
              value={enteredTodo}
              onChange={(e) => setEnteredTodo(e.target.value)} />
        <button>追加</button>
      </form>
    </div>
  )
};
~~~
***
