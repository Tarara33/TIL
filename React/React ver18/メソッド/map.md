# mapメソッド
基本的な mapメソッドについては[こちら](https://github.com/Tarara33/TIL/blob/main/JavaScript/%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89/map.md)

### 【普通の JSのmapと違うところ】
- コンポーネント内で JSを使うので`{}`で囲む
- keyを設定する必要がある
***

# 例題
todosというオブジェクトの中身を、HTMLの中に入れて、それをオブジェクト分量産するという感じ。
~~~
[コンポーネートファイル]

const Todo = () => {
  const todos = [
    {
      id: 1
      title: "勉強する"
    },
    {
      id: 2
      title: "筋トレする"
    }
  ];


  return (
    <>
      <div>
        <p className="title">未完了のTODO</p>
        <ul>
          🩵{todos.map((todo) => (
            <li ⭐️key={todo.id}>
              <div>
                <p>{todo.title}</p>
                <button>完了</button>
                <button>削除</button>
              </div>
            </li>
          ))}
        </ul>
      </div>
    </>
  );
~~~

なので、生成される HTMLは以下のようになる
~~~
[生成 HTML]

<div>
  <p className="title">未完了のTODO</p>
  <ul>
    </li>
      <div>
        <p>勉強する</p>
        <button>完了</button>
        <button>削除</button>
      </div>
    </li>
    </li>
      <div>
        <p>筋トレする</p>
        <button>完了</button>
        <button>削除</button>
      </div>
    </li>
  </ul>
</div>
~~~
***

# key
Key は、どの要素が変更、追加もしくは削除されたのかを Reactが識別するのに役立つ。  
配列内の項目に安定した識別性を与えるため、それぞれの項目に key を与える。

## ⭐️ keyに設定するものポイント
- キーには一意の値を設定する。(だいたいは idが付与される。)
- キーに設定した値は変えない。
- 配列のインデックスはなるべく使わない。  
***

## ❓ なぜ indexはダメなのか??
⚠️ mapメソッドで indexを生成できるが、これを keyとして割り振るのは推奨されていない。  
なぜなら、変わる可能性があるから。
先ほどの例題を使うと、indexはこのように振り分けられる。
~~~
<ul>
------------- index: 0-----------------
  </li>
    <div>
      <p>勉強する</p>
      <button>完了</button>
      <button>削除</button>
    </div>
  </li>
---------------------------------------
------------- index: 1-----------------
  </li>
  </li>
    <div>
      <p>筋トレする</p>
      <button>完了</button>
      <button>削除</button>
    </div>
  </li>
---------------------------------------
</ul>
~~~

ここで、例えば、 index: 0を消すと、
~~~
<ul>
------------- index: 0-----------------
  </li>
  </li>
    <div>
      <p>筋トレする</p>
      <button>完了</button>
      <button>削除</button>
    </div>
  </li>
---------------------------------------
</ul>
~~~
繰り上がって、先ほど index: 1だったものが index: 0になる。    
なので、例えば、index:１の投稿を消す。としても存在しない。  
Reactが認識しにくい！
***
