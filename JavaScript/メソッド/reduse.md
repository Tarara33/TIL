# reduse()
配列の要素を1つの値にまとめるために使用されるJavaScriptの配列メソッド。    
このメソッドは、配列の各要素に対して指定されたコールバック関数を実行し、その結果を累積していくという特徴がある。
~~~
array.reduce(callback(accumulator, currentValue, currentIndex, array), initialValue)
~~~
- callback    
現在の要素に対して実行されるコールバック関数。次の4つのパラメータを取る。    
    
- accumulator    
現在までの累積値。コールバック関数の戻り値が次のループの accumulatorになる。    
    
- currentValue    
現在処理中の要素の値。    
    
- currentIndex (オプション)    
現在処理中の要素のインデックス。    
    
- array (オプション)    
.reduce()メソッドが呼び出された元の配列。    
    
- initialValue (オプション)    
初期の累積値。省略される場合は、配列の最初の要素が初期の累積値として使用される。    
***

## 例
~~~
const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0);

console.log(sum); // 出力: 15 (1 + 2 + 3 + 4 + 5)
~~~
【.reduce()メソッドの挙動】    
    
最初の要素から最後の要素まで順番にコールバック関数が実行される。    
コールバック関数内で accumulatorと currentValueが操作され、新しい累積値が得られる。    
最後の要素までコールバック関数が適用された後、最終的な累積値が返される。    
***
