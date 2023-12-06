# propsのルール
## propsは読み取り専用
⭐️部分のように渡ってきた propsの値を書き換えることはできない。
~~~
[子]

const Hello = (props) => {
  ⭐️const props = 'Tarara';

  return (
    <div>
      <h3>Hello {props.name}</h3>
    </div>
  );
};
~~~
***

### 💡 読み取り専用を確認する方法
`Reflect.getOwnPropertyDescriptor(props, '検証したいプロパティ')`とする。
~~~
[親]

const Example = () => {
  return (
    <>
      <Hello name="RRR"/>
    </>
  );
};

------------------------------------------------------------------
[子]

const Hello = (props) => {
  const dev = Reflect.getOwnPropertyDescriptor(props, 'name')
  console.log(dev)

  return (
    <div>
      <h3>Hello {props.name}</h3>
    </div>
  );
};
~~~

[![Image from Gyazo](https://i.gyazo.com/d4e58039cf4ae86cf69d0c417e8e5c8a.png)](https://gyazo.com/d4e58039cf4ae86cf69d0c417e8e5c8a)

ここの「writable: false」が valueの値を書き換えできないという意味。
***

## propsの流れは一方通行
親から子へ渡すものなので、逆はない。
***
