# クラスの初期値
constructorで設定    
引数渡すとnewでインスタンス作られた時に渡せる
~~~
class Post {
  constructor(name,age) {
    this.name = name;
    this.age = age;
    this.likeCount = 0;
   }
   
  show() {
   console.log(`名前は：${this.name}、年齢は${this.age}です...${this.likeCount}いいね`);
  }
}

const posts = [
new Post("ドラえもん",120),
new Post("桜木花道",17),
]

posts[0].show(); //名前は：ドラえもん、年齢は120です...0いいね
posts[1].show(); //名前は：ドラえもん、年齢は120です...0いいね
~~~
このクラスから作られるインスタンスは this で表現する
***
