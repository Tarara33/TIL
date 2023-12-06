# Children
[ここ](https://github.com/Tarara33/TIL/blob/main/React/React%20ver18/%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88/Props/%E5%9F%BA%E6%9C%AC.md#%E3%82%BF%E3%82%B0%E3%81%AE%E9%96%93%E3%81%AE%E8%A6%81%E7%B4%A0%E3%81%AE%E5%8F%96%E5%BE%97)でも少し書いたが、  
コンポーネントを呼んでるタグの間にある要素は Childrenとなる。

なので
~~~
[親]

<Children >
  <Conpo1 title="RRR"/>
</Children>

[子]
const Conpo1 = ({ title, children }) => {
  return (
    <div className="container">
      <h3>{title}</h3>
      {children}
    </div>
  );
};
~~~
とすると、子コンポーネントに Conpo1の要素が入る。
***
