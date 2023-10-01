# filter()
要素のうち条件に合うものを抽出する
~~~
const array = [1,11, 56, 104];
const evenNumber = array.filter((number) => {
  if (number % 2 === 0) {
    return true;
  }
  return false;
});
console.log(evenNumber); // [56,104]
~~~
短くもかける
~~~
const array = [1,11, 56, 104];
const evenNumber = array.filter(number => number % 2 === 0);
console.log(evenNumber); // [56,104]
~~~
***
