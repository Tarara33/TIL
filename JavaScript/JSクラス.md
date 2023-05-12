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
***

# this
呼び出した場所や方法によってその中身が変化する 
簡単に言うと呼び出したやつ自身という感じ
~~~
class Post {
  constructor(name,age) {
    this.name = name;
    this.age = age;
    this.likeCount = 0;
   }
 }
 
 const posts = [
new Post("ドラえもん",120),
new Post("桜木花道",17),
]
console.log(posts[0]); //Post {name: 'ドラえもん', age: 120, likeCount: 0}
~~~
new Postで渡した引数たちがそれぞれ自分の(this.)name = name...といった感じで入る
***

# 静的メソッド
クラスが使えるメソッド   
static メソッド名とする   
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
  
  static showInfo() {
    console.log("Post class version 1.1");
  }
}

const posts = [
new Post("ドラえもん",120),
new Post("桜木花道",17),
]

Post.showInfo(); //Post class version 1.1
posts[0].showInfo(); //エラー
~~~
***

# クラスの継承
class newクラス名　extends 継承元のクラス名    
継承元クラスのメソッド使える
⚠️this.を使うにはnewクラスでsuper()を使う必要あり
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

class NewPost extends Post {
  constructor(name,age,like) {
    super(name,age);
    this.like = like;
  }
  
  show() {
   super.show();
   console.log(`好きなお菓子は${this.like}です`);
  }
}

const posts = [
new Post("ドラえもん",120),
new Post("桜木花道",17),
new NewPost("しんちゃん",5,"チョコビ"),
]

posts[3].show(); //名前は：しんちゃん、年齢は5です...0いいね
                   好きなお菓子はチョコビです
~~~
***
