# use系フックの注意点
reactが提供する use〇〇の use系フックは関数コンポーネントのいちばん上でしか呼べない。
~~~
[⭕️]

export const App = () => {
  const [num, setNum] = useState(0);

  const onClickCountUp = () => {
    setNum((num) => num + 1);
    });
  };


[❌]

export const App = () => {
  const onClickCountUp = () => {
    const [num, setNum] = useState(0);
    setNum((num) => num + 1);
    });
  };
~~~
***
